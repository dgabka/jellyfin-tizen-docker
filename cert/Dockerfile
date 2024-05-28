FROM ubuntu:22.04 as build

ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update \
    && apt-get upgrade -y \
    && apt-get install -y --no-install-recommends \
    wget=1.21.2-2ubuntu1 \
    ca-certificates=20211016 \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# Create user
RUN useradd -m jellyfin -s /bin/bash
USER jellyfin

# Install tizen-studio
RUN wget --progress=dot:giga https://download.tizen.org/sdk/Installer/tizen-studio_5.5/web-cli_Tizen_Studio_5.5_ubuntu-64.bin -P /home/jellyfin \
    && chmod a+x /home/jellyfin/web-cli_Tizen_Studio_5.5_ubuntu-64.bin \
    && ./home/jellyfin/web-cli_Tizen_Studio_5.5_ubuntu-64.bin --accept-license /home/jellyfin/tizen-studio

ENV PATH=${PATH}:/home/jellyfin/tizen-studio/tools/ide/bin:/home/jellyfin/tizen-studio/tools

ARG CERT_ALIAS
ARG CERT_PASSWORD
ARG CERT_COUNTRY
ARG CERT_NAME
ARG CERT_FILENAME

# Create certificates
RUN tizen certificate \
    -a ${CERT_ALIAS} \
    -p ${CERT_PASSWORD} \
    -c ${CERT_COUNTRY} \
    -ct ${CERT_COUNTRY} \
    -n ${CERT_NAME} \
    -f ${CERT_FILENAME}

CMD [ "/bin/bash" ]
