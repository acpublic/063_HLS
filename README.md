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

### サンプル動画ダウンロード(html/hlsを作成)
```
wget https://download.blender.org/peach/bigbuckbunny_movies/BigBuckBunny_320x180.mp4 -O input.mp4
```

### HLSに変換
```
ffmpeg -i input.mp4 -c:v libx264 -c:a aac -strict experimental -f hls -hls_time 10 -hls_playlist_type vod -hls_segment_filename "output%03d.ts" manifest.m3u8
```
- manifest.m3u8
- output*.ts
