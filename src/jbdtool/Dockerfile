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
	libglib2.0-dev \
	libssl-dev \
	&& rm -rf /var/lib/apt/lists/* \
	&& rm -rf /var/cache/apt/archives/*

COPY --from=build /build/src/libpaho-mqtt3c.so.1 /usr/lib/libpaho-mqtt3c.so.1
COPY --from=build /build/jbdtool/jbdtool /usr/bin/jbdtool
