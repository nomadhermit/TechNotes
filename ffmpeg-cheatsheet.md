# Bulk convert avi files to mp4
```
for f in *.AVI; do ffmpeg -i "$f" "${f%.AVI}.mp4"; done
```
