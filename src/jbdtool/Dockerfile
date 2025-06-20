FROM ubuntu:24.04 AS build

WORKDIR /build

RUN apt update && apt install -y --no-install-recommends \
	build-essential \
	cmake \
	devscripts \
	dh-make \
	fakeroot \
	gcc \
	git \
	libglib2.0-dev \
	libssl-dev \
	lsb-release \
	make \
	pkg-config \
	sudo \
	&& rm -rf /var/lib/apt/lists/* \
	&& rm -rf /var/cache/apt/archives/*

RUN git clone https://github.com/eclipse-paho/paho.mqtt.c.git \
	&& cmake \
	-DPAHO_WITH_SSL=TRUE \
	-DPAHO_BUILD_SHARED=TRUE \
	-DPAHO_BUILD_STATIC=TRUE \
	-DPAHO_ENABLE_TESTING=FALSE \
	-DPAHO_BUILD_DOCUMENTATION=FALSE \
	-DPAHO_BUILD_SAMPLES=FALSE paho.mqtt.c \
	&& make \
	&& make install

RUN git clone https://github.com/sshoecraft/jbdtool.git && cd jbdtool && make


FROM ubuntu:24.04

RUN apt update && apt install -y --no-install-recommends \
	ca-certificates \
	libglib2.0-dev \
	libssl-dev \
	openjdk-21-jre-headless \
	openssh-client \
	wget \
	&& rm -rf /var/lib/apt/lists/* \
	&& rm -rf /var/cache/apt/archives/* \
	&& mkdir -p ~/.ssh \
	&& ssh-keyscan -t rsa github.com >> ~/.ssh/known_hosts \
	&& ssh-keyscan -t ecdsa github.com >> ~/.ssh/known_hosts \
	&& ssh-keyscan -t ed25519 github.com >> ~/.ssh/known_hosts \
	&& wget -q https://github.com/hivemq/mqtt-cli/releases/download/v4.40.0/mqtt-cli-4.40.0.deb \
	&& apt install -y ./mqtt-cli-4.40.0.deb \
	&& rm mqtt-cli-4.40.0.deb \
	&& mqtt --version

COPY --from=build /build/src/libpaho-mqtt3c.so.1 /usr/lib/libpaho-mqtt3c.so.1
COPY --from=build /build/jbdtool/jbdtool /usr/bin/jbdtool
