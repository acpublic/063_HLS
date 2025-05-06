## HLS
### 拡張子（マニフェスト）
- .m3u8
### セグメント
- .ts

## MPEG-DASHコンテンツ生成
### FFmpeg インストール
```
sudo apt install ffmpeg
```

### サンプル動画ダウンロード(html/hlsを作成して実施)
```
wget https://download.blender.org/peach/bigbuckbunny_movies/BigBuckBunny_320x180.mp4 -O input.mp4
```

## HLSに変換（固定ビットレート）
```
ffmpeg -i input.mp4 -c:v libx264 -c:a aac -strict experimental -f hls -hls_time 10 -hls_playlist_type vod -hls_segment_filename "output%03d.ts" manifest.m3u8
```
- manifest.m3u8
- output*.ts

## HLSに変換（平均ビットレート）
### メディアプレイリスト
```
ffmpeg -y -i input.mp4 \
  -filter_complex "[0:v]split=3[v1][v2][v3]; \
    [v1]scale=w=640:h=360[v1out]; \
    [v2]scale=w=1280:h=720[v2out]; \
    [v3]scale=w=1920:h=1080[v3out]" \
  -map "[v1out]" -map a -c:v:0 libx264 -b:v:0 800k -c:a aac -b:a 96k -f hls -hls_time 6 -hls_playlist_type vod -hls_segment_filename "./360p_%03d.ts" output/360p.m3u8 \
  -map "[v2out]" -map a -c:v:1 libx264 -b:v:1 1500k -c:a aac -b:a 128k -f hls -hls_time 6 -hls_playlist_type vod -hls_segment_filename "./720p_%03d.ts" output/720p.m3u8 \
  -map "[v3out]" -map a -c:v:2 libx264 -b:v:2 3000k -c:a aac -b:a 160k -f hls -hls_time 6 -hls_playlist_type vod -hls_segment_filename "./1080p_%03d.ts" output/1080p.m3u8
```
- 360p.m3u8
- 360p_000.ts, ...
- 720p.m3u8
- 720p_000.ts, ...
- 1080p.m3u8
- 1080p_000.ts, ...

### マスタープレイリスト
```
#EXTM3U

#EXT-X-STREAM-INF:BANDWIDTH=900000,RESOLUTION=640x360
360p.m3u8

#EXT-X-STREAM-INF:BANDWIDTH=1628000,RESOLUTION=1280x720
720p.m3u8

#EXT-X-STREAM-INF:BANDWIDTH=3160000,RESOLUTION=1920x1080
1080p.m3u8
```
- manifest.m3u8
