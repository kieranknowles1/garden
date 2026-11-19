---
categories:
  - "[[Resouces]]"
dg-publish: true
---
Cheat sheet for common [FFMpeg](https://ffmpeg.org/) commands. `$VAR` signifies a user-defined value that should be replaced as needed.
# Concatenate Videos
This assumes that all inputs have the same codec, resolutions, and encoding settings. Files will be concatenated without re-encoding. meaning no quality loss.

```sh
cat <<EOF > list.txt
  file video1.mkv
  file video2.mkv
  ...
EOF

ffmpeg -f concat -i list.txt -c copy out.mkv
```
# Trim Video
Trim a video without re-encoding it, meaning no quality loss and very high speed. `$START` may not be perfectly accurate due to keyframes.

Replacing `-to $END` with `-t $DURATION` will instead trim to a duration starting from `$START`. `-ss` `-to` can be omitted to use the start/end of the video
```sh
ffmpeg -ss $START -to $END -i $INPUT -c copy out.mkv
```
