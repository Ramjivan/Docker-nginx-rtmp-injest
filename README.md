# Build cmd 
docker login
export tagname=v2
docker build -t jivanjangid/streamx-injest:${tagname} .
docker push jivanjangid/streamx-injest:${tagname}

# ENV VARIABLES

## INJEST_API_HOST 
To get a webhook hit on_publish on_done exec_publish and exec_publish_done events

## INJEST_ONLY
to make container injest only and deny all play requests. useful for testing
to enable set value to "true"

# OCI [inspect cmd carefully]
# Login
docker login uk-london-1.ocir.io
username - lrqqxehccngb/oracleidentitycloudservice/ramjeewanj@gmail.com
password - fD+YVqAKJ:]>X)_A{xu3
docker tag jivanjangid/streamx-injest:v2 uk-london-1.ocir.io/lrqqxehccngb/streamway-injest:v1 
docker push uk-london-1.ocir.io/lrqqxehccngb/streamway-injest:v1

# RUN Container
docker run -d -p 1935:1935 --name simple-rtmp-server uk-london-1.ocir.io/lrqqxehccngb/streamway-injest:v1

# ADDON
accepts ON_PUBLISH_URL to auth streams [optional]

# Forked from
jasonrivers/nginx-rtmp

# Docker-nginx-rtmp
Docker image for an RTMP/HLS server running on nginx

* NGINX Version 1.21.1
* nginx-rtmp-module Version 1.2.2

## Configurations
This image exposes port 1935 for RTMP Steams and has 2 default channels open "live" and "testing".

live (or your first stream name) is also accessable via HLS on port 8080

It also exposes 8080 so you can access http://<your server ip>:8080/stat to see the streaming statistics.

The configuration file is in /opt/nginx/conf/

## Running

To run the container and bind the port 1935 to the host machine; run the following:
```
docker run -p 1935:1935 -p 8080:8080 jasonrivers/nginx-rtmp
```

### Multiple Streams:
You can enable multiple streams on the container by setting RTMP_STREAM_NAMES when launching, This is a comma seperated list of names, E.G.
```
docker run      \
    -p 1935:1935        \
    -p 8080:8080        \
    -e ON_PUBLISH_URL=http://host:port/path       \
    -e RTMP_STREAM_NAMES=live,teststream1,teststream2   \
    jasonrivers/nginx-rtmp
```

### Pushing streams
You can ush your main stream out to other RTMP servers, Currently this is limited to only the first stream in RTMP_STREAM_NAMES (default is live) by setting RTMP_PUSH_URLS when launching, This is a comma seperated list of URLS, EG:
```
docker run      \
    -p 1935:1935        \
    -p 8080:8080        \
    -e RTMP_PUSH_URLS=rtmp://live.youtube.com/myname/streamkey,rtmp://live.twitch.tv/app/streamkey
    jasonrivers/nginx-rtmp
```

## OBS Configuration
Under broadcast settings, set the follwing parameters:
```
Streaming Service: Custom
Server: rtmp://<your server ip>/live
Play Path/Stream Key: mystream
```

## Watching the steam

In your favorite RTMP video player connect to the stream using the URL:
rtmp://&lt;your server ip&gt;/live/mystream
http://&lt;your server ip&gt;/hls/mystream.m3u8

## Tested players
 * VLC
 * omxplayer (Raspberry Pi)
