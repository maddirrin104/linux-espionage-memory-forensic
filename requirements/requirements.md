# Yêu cầu đối với đồ án 
## Thực hiện
1) Không chỉ đọc, dịch, hoặc tóm tắt paper.
2) Bắt buộc phải có prototype / mô phỏng / reproduction thu gọn / thực nghiệm thực hành forensics.
3) Có thể thực hiện theo một trong các hướng:
    * Tái hiện một phần ý tưởng chính của paper trên dataset/case study nhỏ;
    * Xây dựng công cụ hoặc quy trình forensic đơn giản hóa;
    * So sánh phương pháp của paper với baseline;
    * Đề xuất một cải tiến nhỏ rồi kiểm chứng bằng thực nghiệm.
4) Sản phẩm nên thể hiện được tính chất của môn Digital Forensics, ví dụ: thu thập – bảo toàn – phân tích – diễn giải chứng cứ số hoặc hỗ trợ điều tra/phát hiện/phân loại hiện tượng phục vụ điều tra số.

## Đánh giá 
1) Phải có thực nghiệm rõ ràng, mô tả được dữ liệu, công cụ, quy trình và kết quả.
2) Phải có baseline hoặc đối chứng để so sánh.
3) Sử dụng metric phù hợp với bài toán, ví dụ: accuracy, precision, recall, F1, detection rate, false positive rate, latency, throughput, artifact recovery rate, time-to-analysis, completeness, hoặc robustness.
4) Phải có phần phân tích ưu điểm, hạn chế, rủi ro sai lệch, và phạm vi áp dụng trong bối cảnh điều tra số.
5) Khuyến khích thảo luận thêm về giá trị thực tiễn đối với quy trình điều tra, khả năng mở rộng, khả năng dùng trong lab học thuật, và những giới hạn khi áp dụng vào hiện trường thực tế.

## Kĩ thuật 
1) Phải mô tả rõ môi trường triển khai, gồm: hệ điều hành, phần cứng cơ bản, ngôn ngữ lập trình, thư viện, framework, công cụ forensic hoặc công cụ hỗ trợ phân tích.
2) Phải nêu rõ nguồn dữ liệu sử dụng: dataset công khai, dữ liệu mô phỏng, log tự sinh, pcap, file mẫu, image forensics, memory dump, disk image, email header, malware sample an toàn, hoặc artifact thu được từ kịch bản giả lập.
3) Phải trình bày quy trình tiền xử lý dữ liệu nếu có, ví dụ: lọc log, tách phiên mạng, trích xuất artifact, chuẩn hóa đặc trưng, gán nhãn, ẩn thông tin nhạy cảm.
4) Bắt buộc có sơ đồ hệ thống / pipeline / workflow thể hiện rõ các bước chính, ví dụ: thu thập dữ liệu $\rightarrow$ bảo toàn/chuẩn bị dữ liệu $\rightarrow$ phân tích/trích xuất đặc trưng $\rightarrow$ phát hiện/khôi phục/suy luận $\rightarrow$ đánh giá kết quả.
5) Nếu đề tài có xây dựng công cụ, cần mô tả rõ đầu vào – xử lý – đầu ra của hệ thống.
6) Nếu đề tài tái hiện paper, phải chỉ ra rõ phần nào giữ nguyên theo paper, phần nào đơn giản hóa, và phần nào nhóm tự bổ sung hoặc cải tiến.
7) Khuyến khích minh họa bằng hình kiến trúc, lưu đồ, bảng mô tả thành phần, hoặc pipeline thực nghiệm để người đọc dễ theo dõi và có thể tái lập lại.