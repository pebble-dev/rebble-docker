# Rebble Dockerfile

[Docker](http://docker.com) container to build Pebble apps and RebbleOS


## Usage

### Install

Pull `rebble/pebble-sdk` from the Docker repository:

    docker pull rebble/pebble-sdk


### Run

Run the image

    docker run --name=RebbleSDK -it rebble/pebble-sdk

### Emulator

In order to use the emulator, Docker must know the display to output to.
We can proxy the display by setting environment variables and bind mounts.

    docker run --name=RebbleSDK -it \
        -e DISPLAY=$DISPLAY \
        -v /tmp/.X11-unix:/tmp/.X11-unix \
        rebble/pebble-sdk

On linux hosts that only allow access to the X server from valid user accounts
(Arch linux, Manajaro, etc) you will need to add a link to xauthority.

    docker run --name=RebbleSDK -it \
        -e DISPLAY=$DISPLAY \
        -v /tmp/.X11-unix:/tmp/.X11-unix \
        -v ~/.Xauthority:/home/pebble/.Xauthority \
        rebble/pebble-sdk
