
- Run Docker container with debugging capabilities (for gdb)

```sh
docker run -it --rm --cap-add=SYS_PTRACE --security-opt \
  seccomp=unconfined -v $(pwd):/mnt cpp-debug:0.0.1
```

