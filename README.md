#1. test 1
1. BUG: stream starts from beginning every time
2. BUG: not looped

```
ffmpeg `
-i video_0.mp4 `
-i video_1.mp4 `
-i video_2.mp4 `
-i "http://64.71.79.181:5234/stream" `
-i logo_small.png `
-filter_complex `
"[0:v]scale=1280x720:force_original_aspect_ratio=increase[v0]; `
[1:v]scale=1280x720:force_original_aspect_ratio=increase[v1]; `
[2:v]scale=1280x720:force_original_aspect_ratio=increase[v2]; `
[0:a]volume=0.0[av0]; `
[3:a]volume=1.0[aa0]; `
[1:a]volume=0.0[av1]; `
[3:a]volume=1.0[aa1]; `
[2:a]volume=1.0[av2]; `
[3:a]volume=0.0[aa2]; `
[av0][aa0]amix=inputs=2:duration=shortest[a0]; `
[av1][aa1]amix=inputs=2:duration=shortest[a1]; `
[av2][aa2]amix=inputs=2:duration=shortest[a2]; `
[v0][a0][v1][a1][v2][a2]concat=n=3:v=1:a=1[videoconcat][audioout]; `
[videoconcat][4:v]overlay=W-w-10:10[videoout]" `
-map [videoout] -map [audioout] `
-c:v libx264 -crf 21 -preset veryfast `
-c:a aac -b:a 128k -ac 2 `
-f mpegts udp://127.0.0.1:23000
```
