---
title: 如何将视频转化为Gif
encrypt: false
enc_pwd: askding
categories:
  - Tools
date: 2022-04-11 21:04:13
top:
tags:
layout:
---
编写进度
- [x] 


# Table of Contents

1.  [How to convert .mov or .mp4 to .gif using the command line](#orgc4eb406)
    1.  [How to convert](#org3e63297)


<a id="orgc4eb406"></a>

# How to convert .mov or .mp4 to .gif using the command line

Requirements

    brew install ffmpeg
    brew install gifsicle


<a id="org3e63297"></a>

## How to convert

    ffmpeg -i in.mov -s 600x400 -pix_fmt rgb24 -r 10 -f gif - | gifsicle --optimize=3 --delay=3 > out.gif

Arguments:

    -r 10 to reduce the frame rate from 25 fps to 10 fps
    -s 600x400 to determine the output size.
    --delay=3 to have a delay of 30ms between each gif
    --optimize=3 to use the most file-size optimized algorithm

