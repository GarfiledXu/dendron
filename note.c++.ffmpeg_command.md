---
id: 8ccul365ykruox598ewmhto
title: Ffmpeg_command
desc: ''
updated: 1711092652439
created: 1710384981010
---
#### refe
[FFmpeg 实用命令](https://wklchris.github.io/blog/FFmpeg/FFmpeg.html)

#### tool command
```bash
## 用nvidia显卡的硬件 编码一个完整的yuv文件为mp4视频
ffmpeg -s 1920x1080 -pix_fmt yuv420p -i hh/hh.yuv -vcodec h264_nvenc -r 60 -y 2 out.mp4

## 普通编码
ffmpeg -s 1920x1080 -pix_fmt yuv420p -i hh/hh.yuv -v:c libx264  -r 60 -y ouput.mp4

## 将一个mp4视频提取出一个完整的yuv文件
ffmpeg -i input.mp4 -pix_fmt yuv420p hh/hh.yuv
```

