# Overview
Dockerfile that allows you build hashcat inside a container of your choice.

This uses the code taken from the public hashcat repository, but I have made some modifications including seclists as a wordlist dump.

## Disclaimer

1. I do not take any credit for writing this Dockerfile outside of my own modifications - please support the original creators:
https://hashcat.net/hashcat/
https://github.com/hashcat/hashcat

2. This will not teach you how to setup the containers to allow for GPU Passthru, this is something you'll need to work out on your own - there's lots of guides and official documentation from both Podman and Docker to help you!

## Setup:

I use podman and this was the guide provided to allow it to work with Podman:
https://podman-desktop.io/docs/podman/gpu

For docker, there are loads of guides, this is quite an easy one to follow:
https://blog.roboflow.com/use-the-gpu-in-docker/

The minimum requirements to use this version are:
 - Nvidia Drivers in this case (580)
 - Nvidia Container Toolkit
 - Optionally - Nvidia CUDA Toolkit (this image is based on Nvidia's CUDA image in Ubuntu with the cuda drivers installed)

If on an SElinux enabled machine, and `getenforce` is set to "enforcing" then you will need to provide the `--security-opt label=disable` argument so your container has sufficient permissions.

## Alias:
To allow you to run the command as if you had it installed natively (e.g.`hashcat -I`) then setup the following alias in your `.bashrc` or `.zshrc` file:
```
hashcat () {
  podman run
  --rm \
  -it \
  --gpus all \
  --security-opt label=disable \
  -v [DESIRED MOUNT]:[MOUNT POINT] \
  IMAGE-NAME:TAG \
  ./hashcat.bin'
```
 - If using docker, replace podman with docker
 - If you wish to use hashcats utils then remove ./hashcat.bin with whatever entrypoint you want.

If you have modifications then please feel free to make them, or suggest them to me!
