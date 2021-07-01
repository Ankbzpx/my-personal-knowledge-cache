---
tags: [Coding]
title: FFmpeg
created: '2021-07-01T02:14:01.554Z'
modified: '2021-07-01T02:18:33.056Z'
---

# FFmpeg

1. `alphamerge` first two video
2. create pure white video with given resolution, framerate and format
3. overlap alphamerged video with pure white for suration equal to shortest one
```
for i in *_1.mpg; do
  ffmpeg -i "$i" -i "${i%_*}_2.mpg" -vcodec libx264 -filter_complex "[0][1]alphamerge[ia];color=white:s=2560x1440:rate=30[bg];[bg][ia]overlay=shortest=1,format=yuv420p" "${i%.usm_1.mpg}.mp4"
done
```
scale video such that its width equal to 720
```
for i in *.mp4; do ffmpeg -i "$i" -vf scale=720:-1 "${i%.*}_720p.mp4";done
```
flip video upside down

```
for i in *.mp4; do ffmpeg -i "$i" -vf "transpose=2,transpose=2" "out/${i%.*}.mp4";done
```
