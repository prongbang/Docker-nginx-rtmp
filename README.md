# Docker Nginx RTMP

This Dockerfile uses `nginx-rtmp-module` and forked from [docker-nginx-rtmp](https://github.com/TakuSemba/docker-nginx-rtmp). Server receives RTMP streams and publish HLS.
Note that this publishes only one resolution on HLS.

## Run with docker-compose

#### 1. Run in background

```sh
docker-compose up -d
```

### 1.1 To rebuild this image you must use
```sh
docker-compose up --build
```

#### 2. Stop
```sh
docker-compose down
```

## Run without docker-compose

#### 1. Build the Dockerfile

```sh
docker build -t nginx-rtmp .
```

#### 2. run the Container

```sh
docker run -t -i -p 1935:1935 -p 8000:8000 --rm nginx-rtmp
```

#### 3. send RTMP stream from client

send stream to `rtmp://ip-address:1935/live/stream_name`. 
you can also receive RTMP stream with the same url.

#### 4. play hls

media playlist is `http://ip-address:8000/hls/stream_name.m3u8`

#### To publish a stream, invoke ffmpeg like this:
```sh
$ ffmpeg -re -i ~/Movies/stream_name.mp4 -bsf:v h264_mp4toannexb
         -c copy -f mpegts http://127.0.0.1:8000/live/stream_name
```

#### HLS can be played primarily in Safari and mobile devices using the following HTML:
```html
<body>
  <video width="640" height="480" controls autoplay
         src="http://127.0.0.1:8000/hls/stream_name.m3u8">
  </video>
</body>
```

#### MPEG-DASH is now supported on most browsers including Chrome, Firefox, Safari. To play the stream, the dash.js player can be used like this:
```html
<script src="http://cdn.dashjs.org/latest/dash.all.min.js"></script>

<body>
  <video data-dashjs-player
         width="640" height="480" controls autoplay
         src="http://127.0.0.1:8000/hls/stream_name.mpd">
  </video>
</body>
```