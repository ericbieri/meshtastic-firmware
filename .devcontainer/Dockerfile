# trunk-ignore-all(terrascan/AC_DOCKER_0002): Known terrascan issue
# trunk-ignore-all(hadolint/DL3008): Do not pin apt package versions
# trunk-ignore-all(hadolint/DL3013): Do not pin pip package versions
FROM mcr.microsoft.com/devcontainers/cpp:2-debian-13

USER root

RUN apt-get update && export DEBIAN_FRONTEND=noninteractive \
    && apt-get -y install --no-install-recommends \
    ca-certificates \
    g++ \
    git \
    libbluetooth-dev \
    libgpiod-dev \
    liborcania-dev \
    libssl-dev \
    libulfius-dev \
    libyaml-cpp-dev \
    pipx \
    pkg-config \
    python3 \
    python3-pip \
    python3-venv \
    python3-wheel \
    wget \
    zip \
    usbutils \
    hwdata \
    gpg \
    gnupg2 \
    libusb-1.0-0-dev \
    libuv1-dev \
    libi2c-dev \
    libxcb-xkb-dev \
    libxkbcommon-dev \
    libinput-dev \
    && apt-get clean && rm -rf /var/lib/apt/lists/*

# Add Post internal CA certificates    
RUN curl "https://artifactory.tools.post.ch/artifactory/list/config-local/ch/post/it/pki/cacerts/rootca/RootCA-PEM.zip" -o RootCA-PEM.zip \
    && curl "https://artifactory.tools.post.ch/artifactory/list/config-local/ch/post/it/pki/cacerts/intermediateca/IntermediateCA-PEM.zip" -o IntermediateCA-PEM.zip \
    && mkdir /usr/local/share/ca-certificates/pem/ \
    && unzip RootCA-PEM.zip -d /usr/local/share/ca-certificates/pem \
    && unzip IntermediateCA-PEM.zip -d /usr/local/share/ca-certificates/pem
RUN for c in `find /usr/local/share/ca-certificates/pem -type f -name "*.pem"`; \
    do \
      newFileName=$(basename $c | sed 's/.pem/.crt/g'); \
      openssl x509 -inform PEM -in ${c} -out /usr/local/share/ca-certificates/${newFileName}; \
    done \
    && rm -rf /usr/local/share/ca-certificates/pem \
    && rm RootCA-PEM.zip IntermediateCA-PEM.zip \
    && update-ca-certificates

RUN pipx install platformio

COPY 99-platformio-udev.rules /etc/udev/rules.d/99-platformio-udev.rules

USER vscode

HEALTHCHECK NONE