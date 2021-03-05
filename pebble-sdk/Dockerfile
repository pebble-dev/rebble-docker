FROM debian:stable

# update system and get base packages
RUN apt-get update && \
    apt-get install -y curl gcc git make python2.7 python2.7-dev python-dev python3-dev  python3-pip python python-virtualenv python3-virtualenv libfreetype6-dev bash-completion libsdl1.2debian \
                       libfdt1 libpixman-1-0 libglib2.0-dev gawk && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

#manually install latest gcc-arm
RUN curl -sSL https://developer.arm.com/-/media/Files/downloads/gnu-rm/9-2019q4/gcc-arm-none-eabi-9-2019-q4-major-x86_64-linux.tar.bz2 \
   | tar -C /opt/ -xj

#hacky workaround to make rebbleos makefile happy
RUN ln -s /opt/gcc-arm-none-eabi-9-2019-q4-major/bin/* /usr/bin/

# set the version of the pebble tool
ENV PEBBLE_TOOL_VERSION pebble-sdk-4.5-linux64
# set the version of pre installed
ENV PEBBLE_SDK_VERSION 4.3

# get pebble tool
RUN curl -sSL https://developer.rebble.io/s3.amazonaws.com/assets.getpebble.com/pebble-tool/${PEBBLE_TOOL_VERSION}.tar.bz2 \
        | tar -C /opt/ -xj

# prepare python environment 
WORKDIR /opt/${PEBBLE_TOOL_VERSION}
RUN /bin/bash -c " \
        virtualenv .env && \
        source .env/bin/activate && \
        pip install -r requirements.txt && \
        deactivate " && \
    rm -r /root/.cache/

# disable analytics & add pebble user - necesary for Arch linux hosts
RUN adduser --disabled-password --gecos "" --ingroup users pebble && \
    echo "pebble ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers && \
    chmod -R 777 /opt/${PEBBLE_TOOL_VERSION} && \
    mkdir -p /home/pebble/.pebble-sdk/ && \
    chown -R pebble:users /home/pebble/.pebble-sdk && \
    touch /home/pebble/.pebble-sdk/NO_TRACKING

#switch user
USER pebble

#install nvm
ENV NODE_VERSION 10.16.2
ENV NVM_DIR /home/pebble/.nvm
RUN mkdir -p $NVM_DIR && \
    curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.0/install.sh | bash && \
    . $NVM_DIR/nvm.sh && \
    nvm install $NODE_VERSION

# set PATH
ENV PATH /opt/gcc-arm-none-eabi-9-2019-q4-major/bin/:${NVM_DIR}/versions/node/v${NODE_VERSION}/bin:/opt/${PEBBLE_TOOL_VERSION}/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin

# check path
RUN /bin/bash -c "arm-none-eabi-gcc -v"

# install sdk
RUN yes | pebble sdk install https://github.com/aveao/PebbleArchive/raw/master/SDKCores/sdk-core-${PEBBLE_SDK_VERSION}.tar.bz2 && \
    pebble sdk activate ${PEBBLE_SDK_VERSION}

# set mount path
VOLUME /pebble/

#run command
WORKDIR /pebble/
CMD /bin/bash
