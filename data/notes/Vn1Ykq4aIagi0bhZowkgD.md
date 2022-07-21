
## Scale video such that its width equal to 720
```
for i in *.mp4; do ffmpeg -i "$i" -vf scale=720:-1 "${i%.*}_720p.mp4";done
```
## Flip video upside down

```
for i in *.mp4; do ffmpeg -i "$i" -vf "transpose=2,transpose=2" "out/${i%.*}.mp4";done
```
## Extract single frame as an image at given timestamp
```
ffmpeg -i DeskPortal_State_03_PC_1440p_Idle_SitAd_30fps_40534656.m2v -ss 00:00:11 -vframes 1 output.png
```

## Extract all frame at given frame rate
```
ffmpeg -i VID_20220126_151107.mp4 -r 4 images/frame%04d.png
```

## Pad video
```
ffmpeg -i tom_1080.mp4 -vf "pad=width=1920:height=1080:x=(1920-1080)/2:y=0:color=white" tom_pad.mp4
```

## Loop video
```
ffmpeg -stream_loop 2 -i tbrender.mp4 -c copy tbrender_loop.mp4
```

## Chain
1. `scale` video to `1080x1080`
2. `pad` video to `1920x1080`
```
ffmpeg -i tom.mp4 -vf "scale=1080:1080,pad=1920:height=1080:x=(1920-iw)/2:y=(1080-ih)/2:color=white" tom_pad_chain.mp4
```

## Complex scenario

### Example 1
1. green screen to transparent (similarity 0.1, blend 0.2)
2. overlay

```
ffmpeg -i flower.mp4 -i women.mp4 -vcodec libx264 -filter_complex "[0]chromakey=green:0.1:0.2[ia];[1][ia]overlay" out.mp4
```

### Example 2
1. `alphamerge` first two video
2. create pure white video with given resolution, framerate and format
3. overlap alphamerged video with pure white for suration equal to shortest one
4. crop video to `2240x1260` with upper left corner at `(160, 0)`
```
for i in *_1.mpg; do
  ffmpeg -i "$i" -i "${i%_*}_2.mpg" -vcodec libx264 -filter_complex "[0][1]alphamerge[ia];color=white:s=2560x1440:rate=30[bg];[bg][ia]overlay=shortest=1,format=yuv420p[v];[v]crop=2240:1260:160:0" "${i%.usm_1.mpg}.mp4"
done
```
