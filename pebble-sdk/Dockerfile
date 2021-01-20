FROM alpine:latest

# update system and get base packages
RUN apk update && apk add \
    bash \
    curl \
    gcc \
    g++ \
    git \
    make \
    python2 \
    python2-dev \
    python3 \
    python3-dev \
    musl-dev \
    freetype-dev \
    bash-completion \
    sdl \
    libfdt \
    pixman \
    glib \
    gawk \
    npm

RUN apk add \
  --no-cache \
  --repository http://dl-cdn.alpinelinux.org/alpine/edge/testing \
  --repository http://dl-cdn.alpinelinux.org/alpine/edge/main \
  gcc-arm-none-eabi


# set the version of the pebble tool
ENV PEBBLE_TOOL_VERSION pebble-sdk-4.5-linux64
# set the version of pre installed
ENV PEBBLE_SDK_VERSION 4.3

# get pebble tool
RUN curl -sSL https://developer.rebble.io/s3.amazonaws.com/assets.getpebble.com/pebble-tool/${PEBBLE_TOOL_VERSION}.tar.bz2 \
        | tar -C /opt/ -xj


# prepare python environment 
RUN /bin/bash -c " \
    python -m ensurepip && \
    rm -r /usr/lib/python*/ensurepip && \
    pip install virtualenv"

    
WORKDIR /opt/${PEBBLE_TOOL_VERSION}
RUN /bin/bash -c " \
    virtualenv .env && \
    source .env/bin/activate && \
    pip install -r requirements.txt && \
    deactivate " && \
    rm -r /root/.cache/

#  disable analytics
RUN chmod -R 777 /opt/${PEBBLE_TOOL_VERSION} && \
    mkdir -p /root/.pebble-sdk/ && \
    touch /root/.pebble-sdk/NO_TRACKING

# set PATH
ENV PATH /opt/${PEBBLE_TOOL_VERSION}/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin

#fix alpine linux encoding
ENV PYTHONIOENCODING=utf-8

# install sdk
RUN yes | pebble sdk install https://github.com/aveao/PebbleArchive/raw/master/SDKCores/sdk-core-${PEBBLE_SDK_VERSION}.tar.bz2 && \
    pebble sdk activate ${PEBBLE_SDK_VERSION}

CMD /bin/bash
