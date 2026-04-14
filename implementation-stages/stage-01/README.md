# Reproduce bằng dump có sẵn
## Công việc
1. Clone repo, tải các memory dump theo như repo gốc để reproduce.
2. Cài volatility 3 và chạy plugin như repo gốc.

## Memory dump
> resource: https://fh-muenster.sciebo.de/s/tfaMyfkrWAs2XmZ
- xkeylogger_dump.lime
- xkeylogger_ext_dump.lime
- xkeylogger_xkb_dump.lime
- cam_dump.lime
- mic_pulse_dump.lime
## Cách chạy plugin
Chạy lệnh từ root folder của repo sau khi clone.
| Plugin | Description | Command |
|--------|-------------|---------|
| xevents | Extracts clients that capture events using X core events | `vol -r pretty -f dumps/xkeylogger_dump.lime -s plugins/symbols/ -p plugins/ -v xevents --name Xorg` |
| xinputextensions | Extracts clients that capture events using X input extensions | `vol -r pretty -f dumps/xkeylogger_ext_dump.lime -s plugins/symbols/ -p plugins/ -v xinputextensions --name Xorg` |
| xclients | Lists X11 client connections and window information | `vol -r pretty -f dumps/xkeylogger_xkb_dump.lime -s plugins/symbols/ -p plugins/ -v xclients --name Xorg` |
| v4l2 | Extracts processes that record camera and video streams using V4L2 | `vol -r pretty -f dumps/cam_dump.lime -s plugins/symbols/ -p plugins/ -v v4l2` |
| pipewire | Extracts PipeWire clients that record audio | `vol -r pretty -f dumps/mic_pulse_dump.lime -s plugins/symbols/ -p plugins/ -v pipewire --name pipewire` |S