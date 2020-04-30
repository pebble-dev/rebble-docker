FROM rebble/pebble-sdk:latest

RUN apt-get update && \
    apt-get install -y unzip && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

ENV NRF5_SDK_VERSION nRF5SDK160098a08e2

RUN curl -sSL -o /opt/${NRF5_SDK_VERSION}.zip https://www.nordicsemi.com/-/media/Software-and-other-downloads/SDKs/nRF5/Binaries/nRF5SDK160098a08e2.zip && \
    cd /opt && mkdir ${NRF5_SDK_VERSION} && \
    cd ${NRF5_SDK_VERSION} && unzip -q /opt/${NRF5_SDK_VERSION}.zip -x examples/* *.msi && \
    rm /opt/${NRF5_SDK_VERSION}.zip
