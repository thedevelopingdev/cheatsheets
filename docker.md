# Docker cheatsheet

## Command line interface

## Debugging
* You cannot kill PID 1 in a container (it is the init process). ([Source: Stack Overflow](https://unix.stackexchange.com/questions/457649/unable-to-kill-process-with-pid-1-in-docker-container))


## Using Nvidia GPUs

1. Install the Nvidia drivers.
2. Install the Nvidia container runtime.
  - [Github](https://github.com/NVIDIA/nvidia-container-runtime)
  - [Original](https://nvidia.github.io/nvidia-container-runtime/)
  - [Archive](https://archive.is/GgiOE)

```sh
# for debian based systems (e.g. ubuntu)

curl -s -L https://nvidia.github.io/nvidia-container-runtime/gpgkey | \
  sudo apt-key add -

distribution=$(. /etc/os-release;echo $ID$VERSION_ID)

curl -s -L https://nvidia.github.io/nvidia-container-runtime/$distribution/nvidia-container-runtime.list | \
  sudo tee /etc/apt/sources.list.d/nvidia-container-runtime.list

sudo apt-get update
sudo apt-get install nvidia-container-runtime

sudo systemctl restart docker.service
```
