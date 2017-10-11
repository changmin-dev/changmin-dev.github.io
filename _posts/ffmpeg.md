ffmpeg -i TT.mp4 -map 0 \
-codec:v libx264 -codec:a aac \    
-f ssegment -segment_list playlist.m3u8 \
-segment_list_flags +live -segment_time 10 \
out%03d.ts
