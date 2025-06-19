FROM ubuntu:24.04

RUN apt update && apt install -y --no-install-recommends bluez \
	&& rm -rf /var/lib/apt/lists/* \
	&& rm -rf /var/cache/apt/archives/*
