# Command-line quick reference

## launchctl

```bash
# Launching applications from Spotlight with environment variables
launchctl setenv RSTUDIO_WHICH_R /Users/mattfeng/miniconda3/envs/R/bin/R
```

## `stow`

```bash
# install software to /usr/local/stow/
# e.g. for a piece of software called mutagen
#      /usr/local/stow/mutagen/bin/mutagen

/usr/local/stow $ stow mutagen
```

## Databases

### MySQL

```bash
# connect to mysql database with the command line
mysql -u <USERNAME> -p<PASSWORD> -h <HOST> -P <PORT> -D <DATABASE>
```

```sql
;-- show all tables
SHOW TABLES;

;-- show schema of table;
SHOW COLUMNS FROM <table_name>;
DESCRIBE <table_name>;

;-- delete all rows from table
DELETE FROM <table_name>;

;-- delete select rows from table
DELETE FROM <table_name> WHERE <condition>;
```

## Archives

### `zip`

```bash
# Create zip file from folder
zip OUTPUT_ZIP_NAME.zip -r DIRECTORY_TO_ARCHIVE
```

## `sed`

```bash
# Delete lines based on pattern
sed '/<regex pattern>/d'

# In-place edit
#   macOS
sed -i "" SED_PATTERN_HERE YOUR_FILE

#   Linux (gnu-sed)
sed -i SED_PATTERN_HERE YOUR_FILE
```

## DNS (`dig`)

```bash
# grab all DNS records (except for CNAME)
dig +nocmd yourdomain.example any +multiline +noall +answer
```

## YouTube downloader

```bash
# download mp4 from youtube using yt-dlp
yt-dlp -S res,ext:mp4:m4a --recode mp4 https://www.youtube.com/watch?v=dQw4w9WgXcQ
```

## ffmpeg (audio and video)
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

# trim video from START_TIME to END_TIME
ffmpeg -i INPUT.mp4 -ss START_HR:START_MIN:START_SEC -to START_HR:END_MIN:END_SEC -c:v copy -c:a copy OUTPUT.mp4
# e.g. ffmpeg -i INPUT.mp4 -ss 00:10:15 -to 00:20:32 -c:v copy -c:a copy OUTPUT.mp4
```

## Bash

```bash
# give VARIABLE a default value
VARIABLE=${1:-<default val>}
```

## Networking and ports

```bash
# View which processes are bound to which ports
netstat -tulpn | grep :YOUR_PORT
ss -tulpn | grep :YOUR_PORT
```

## Filesystem

```bash
# Find largest files and directories
du -Sh | sort -rh
```

## Docker

```bash
# build Docker image for Apple Silicon (arm64/v8) Macs
docker buildx build --platform linux/arm64/v8 --load -t IMAGE_NAME:arm64 ..

# build Docker image for Linux
docker buildx build --platform linux/amd64 --load -t IMAGE_NAME ..

# building a multi-arch build with buildx
docker buildx build --platform=linux/amd64,linux/arm64 -t IMAGE_NAME:TAG .

# use plain text progress (i.e. to see `echo` outputs)
# https://dev.to/lboix/dockerfile-how-to-output-the-result-of-a-command-when-building-an-image-35dp
docker buildx build --progress=plain

# don't use cache
docker buildx build --no-cache

# run a Docker container with port forwarding
# and is removed on exit
docker run -d -p HOST_PORT:CONTAINER_PORT --rm --name DESIRED_CONTAINER_NAME IMAGE_NAME[:TAG]

# send Docker image to another computer
docker save IMAGE_NAME | gzip | pv | ssh USER@HOST docker load

# save Docker image
docker save IMAGE_NAME | pv | gzip > OUTPUT.tar.gz

# commit Docker container to image
docker commit -m "MESSAGE" CONTAINER IMAGE_NAME:TAG

# run Docker container with debugging capabilities (for gdb)
docker run -it --rm --cap-add=SYS_PTRACE --security-opt \
  seccomp=unconfined -v $(pwd):/mnt cpp-debug:0.0.1
```

## Kubernetes

See [`kube.md`](./kube.md).

## Vagrant

```bash
# Create a new virtual machine
vagrant init <box type>

# Shutdown a virtual machine
vagrant halt

# Get Vagrant ssh details
vagrant ssh
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
