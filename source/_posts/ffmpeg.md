---
title: ffmpeg基本使用
date: 2020-05-15 10:32:39
tags:
---

# ffmpeg
## 官网
http://ffmpeg.org

## 转换音频

### 将 mp3 ape 转换成 awv
```shell
ffmpeg -i input.mp3  -f  wav  output.wav

ffmpeg -i input.ape  -f  wav  output.wav
```