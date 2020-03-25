FROM rebble/pebble-sdk:latest

RUN apt-get update && \
    apt-get install -y unzip && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

ENV NRF5_SDK_VERSION nRF5_SDK_15.3.0_59ac345

RUN curl -sSL -o /opt/${NRF5_SDK_VERSION}.zip https://www.nordicsemi.com/-/media/Software-and-other-downloads/SDKs/nRF5/Binaries/nRF5SDK153059ac345.zip && \
    cd /opt && \
    unzip -q ${NRF5_SDK_VERSION}.zip -x ${NRF5_SDK_VERSION}/examples/* ${NRF5_SDK_VERSION}/*.msi && \
    rm ${NRF5_SDK_VERSION}.zip
