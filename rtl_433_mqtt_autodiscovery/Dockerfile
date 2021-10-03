ARG BUILD_FROM=ghcr.io/hassio-addons/base/amd64:10.1.0
FROM ${BUILD_FROM}

# Set shell
SHELL ["/bin/bash", "-o", "pipefail", "-c"]

# Copy Python requirements file
COPY requirements.txt /tmp/

# Setup base
ARG BUILD_ARCH=amd64
# hadolint ignore=DL3003
RUN \
    apk add --no-cache --virtual .build-dependencies \
        build-base=0.5-r2 \
        libffi-dev=3.3-r2 \
        openssl-dev=1.1.1l-r0 \
        py3-wheel=0.36.2-r2 \
        python3-dev=3.9.5-r1 \
    \
    && apk add --no-cache \
        py3-pip=20.3.4-r1 \
        py3-bcrypt=3.2.0-r3 \
        python3=3.9.5-r1 \
    \
    && pip install \
        --no-cache-dir \
        --prefer-binary \
        --find-links "https://wheels.home-assistant.io/alpine-3.14/${BUILD_ARCH}/" \
        -r /tmp/requirements.txt \
    \
    && find /usr/local \
        \( -type d -a -name test -o -name tests -o -name '__pycache__' \) \
        -o \( -type f -a -name '*.pyc' -o -name '*.pyo' \) \
        -exec rm -rf '{}' + \
    \
    && apk del --no-cache --purge .build-dependencies


ENV LANG C.UTF-8

# Run script
# COPY run.sh /
COPY run.sh rtl_433_mqtt_hass.py /
RUN chmod a+x /run.sh

CMD [ "/run.sh" ]
