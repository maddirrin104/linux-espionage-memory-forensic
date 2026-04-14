# Tự dựng case study
## Công việc
Dựng 1 VM rồi làm lần lượt:
1. chụp snapshot sạch
2. chạy 1 attack PoC
3. mô phỏng hành vi người dùng vài phút
4. acquire memory
5. hash dump
6. phân tích dump
7. quay về snapshot sạch cho case tiếp theo

Paper nêu tư tưởng đánh vào desktop services lõi như display server, audio server, video device thay vì quét toàn bộ hệ thống, nên cách dựng case cũng phải bám vào các hành vi này
## Thiết lập môi trường
OS: `Xubuntu 24.04`
**Tại sao lại chọn `Xubuntu`?** Trong bảng 3, paper cho thấy `Xubuntu 24.04` với `Xorg` là môi trường dễ ăn nhất. Các kỹ thuật `key`, `mouse`, `screen`, `camera`, `microphone` đều hoạt động đầy đủ hơn so với các distro `Wayland`, trong khi các hệ `Wayland` chỉ đạt mức một phần cho một số kỹ thuật liên quan input,screen trên ứng dụng không native `Wayland`.
Bảng cấu hình phần cứng và phần mềm: 
| Hardware | Thông số | Software | Phiên bản |
| :--- | :--- | :--- | :--- |
| **CPU** | 2 vCPU | **VM Ware WorkStation** | 17.6.4  |
| **RAM** | 4 GB | **Mem. Acquisition** | AVML 0.14.0 |
| **Disk Size** | 30 GB | **Mem. Analysis** | Volatility 3 2.27 |

## Files Attack
> Resource: https://github.com/FHMS-ITS/uncovering-linux-desktop-espionage/tree/main/attacks
- xkeylogger_ext.c
- xscreenshotter.c
- mic_pulse.c
- cam_v4l2.c
## Kịch bản cụ thể
### Case 01 - Keylogging
**Mục tiêu:** chứng minh có thể phát hiện tiến trình lắng nghe input từ X server.

**Cách làm:**
- dùng PoC keylogger trong repo
- mở terminal, gõ một số chuỗi test như student123, forensics-demo
- dump RAM
- chạy xevents, xinputextensions hoặc xclients

Paper và README đều cho thấy hai hướng phát hiện chính cho keylogging: qua X core events hoặc qua X input extensions

### Case 02 — Screen capture
**Mục tiêu:** chứng minh có thể phát hiện hành vi chụp màn hình.
**Cách làm:**
- dùng PoC screenshotter trong repo hoặc chương trình đơn giản gọi API X11/GDK
- mở vài cửa sổ có dữ liệu giả lập
- chụp màn hình nhiều lần
- dump RAM
- chạy `xclients` và đối chiếu plugin phù hợp

Paper mô tả screen capture trên Linux desktop chủ yếu quy về các API X11 như `XOpenDisplay`, `XGetImage`, hoặc wrapper như `GDK/Qt`, với Wayland có hạn chế hơn.

### Case 3 — Microphone capture
**Mục tiêu:** chứng minh có thể phát hiện client ghi âm thông qua audio server.

**Cách làm:**
- dùng PoC microphone trong repo
- ghi âm microphone vài chục giây
- dump RAM
- chạy plugin `pipewire`

Paper và README đều chỉ ra plugin `pipewire` dùng để trích xuất client ghi âm từ memory của PipeWire, kết quả paper còn cho thấy hướng này có tính thực tiễn cao vì các distro được đánh giá đều dùng `PipeWire` làm audio server.

### Case 04 — Camera capture

**Mục tiêu:** chứng minh có thể phát hiện tiến trình truy cập thiết bị camera/video capture trên Linux desktop.

**Cách làm:**

* dùng PoC `cam_v4l2.c` trong repo `attacks/cam`
* trên Xubuntu, bảo đảm VM có **camera passthrough** hoặc một **thiết bị V4L2/virtual webcam** để chương trình có thể mở `/dev/video*`
* chạy PoC camera và để quá trình capture diễn ra trong khoảng 20–60 giây
* trong lúc đó, tạo một số hoạt động “người dùng thật” để bối cảnh điều tra hợp lý hơn, ví dụ:

  * mở trình duyệt hoặc terminal
  * mở cửa sổ Notes/Text Editor có dữ liệu giả lập
  * thao tác chuột/bàn phím ngắn
* dump RAM
* chạy plugin `v4l2`
* đối chiếu output để xác định process/client nào đang mở thiết bị video capture

Paper nêu rõ họ có plugin Volatility3 cho **camera recording malware**, và hình minh họa `v4l2` được mô tả trực tiếp là plugin dùng để phát hiện **capture of video devices**. Repo của tác giả cũng tách riêng attack `cam_v4l2.c` và README liệt kê cặp mẫu `cam_dump.lime` → `v4l2` trong quy trình reproduce.

## Workflow chung cho cả 4 case 
1. Revert VM về snapshot sạch: Snapshot này là trạng thái Xubuntu đã cài đủ tool, chưa chạy PoC, chưa có artefact từ case trước.
2. Boot máy, đăng nhập GUI, đợi ổn định 1–2 phút
Để session Xfce/Xorg, PipeWire, applet hệ thống chạy ổn định, tránh dump quá sớm khi session còn nhiều tiến trình khởi động.
3. Mở một bộ ứng dụng người dùng thật cố định: Firefox, Terminal, Mousepad hoặc bất kỳ text editor nhẹ nào.
Giữ bộ này gần như giống nhau giữa các case để báo cáo nhìn nhất quán hơn.
4. Khởi chạy PoC tấn công
5. Thực hiện user behavior trong khi PoC đang hoạt động
6. Dump RAM khi PoC vẫn còn sống

7. Ghi lại timestamp ngắn
Ví dụ:
10:21:15 start PoC
10:21:30 user bắt đầu gõ
10:21:55 chạy AVML
10:22:20 dừng PoC
8. Tắt PoC và revert snapshot

## Quy trình 4 bước:
Làm theo quy trình 4 bước: `thu thập – bảo toàn – phân tích – diễn giải`




