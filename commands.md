# Programming quick reference

## Commands

### launchctl

```bash
# Launching applications from Spotlight with environment variables
launchctl setenv RSTUDIO_WHICH_R /Users/mattfeng/miniconda3/envs/R/bin/R
```

### YouTube

```bash
# download mp4 from youtube using yt-dlp
yt-dlp -S res,ext:mp4:m4a --recode mp4 https://www.youtube.com/watch?v=dQw4w9WgXcQ
```

### Audio

```bash
# extract audio stream without re-encoding
ffmpeg -i v.mp4 -c copy -map 0:a audio.wav

# extract first N seconds of audio from wav file
ffmpeg -ss 0 -t <N> -i audio.wav first30.wav

# resample audio
ffmpeg -i audio.wav -ar 22050 audio_22k.wav

# merge two audio channels into one
ffmpeg -i audio.wav -ac 1 audio_mono.wav

# extract audio stream with variable bitrate
# 	use this for enhancing audio with Adobe Podcast
ffmpeg -i v.mp4 -q:a 0 -map a audio.mp3

# replace audio from v.mp4 with audio from a.wav
# 	use this for enhancing audio with Adobe Podcast
ffmpeg -i v.mp4 -i a.wav -c:v copy -map 0:v:0 -map 1:a:0 new.mp4
````

## Docker

```bash
# build Docker image for Apple Silicon (arm64/v8) Macs
docker buildx build --platform linux/arm64/v8 --load -t IMAGE_NAME:arm64 ..

# build Docker image for Linux
docker buildx build --platform linux/amd64 --load -t IMAGE_NAME ..

# send Docker image to another computer
docker save IMAGE_NAME | gzip | pv | ssh user@host docker load

# save Docker image
docker save IMAGE_NAME | pv | gzip > OUTPUT.tar.gz

# commit Docker container to image
docker commit -m "MESSAGE" CONTAINER IMAGE_NAME:TAG
```

## SSH
- https://wiki.gentoo.org/wiki/SSH_jump_host
- https://stackoverflow.com/questions/16212816/setting-up-openssh-for-windows-using-public-key-authentication

## RAID
- https://www.prepressure.com/library/technology/raid

## GUI
- ubuntu: `cat /etc/X11/default-display-manager`
- opensuse: `cat /usr/lib/X11/displaymanagers/default-displaymanager`
- redhat/fedora: `cat /etc/sysconfig/desktop`

## Links

### Fonts
- https://modernfontstacks.com/
