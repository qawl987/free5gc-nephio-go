ARG BUILDER_IMAGE=golang
ARG BASE_IMAGE=golang

# See: https://github.com/free5gc/free5gc/wiki/Installation

#
# Builder
#

FROM $BUILDER_IMAGE AS builder
ARG VERSION=4.1.0
ENV VERSION=$VERSION


# Note: "golang" builder image is based on Debian
RUN curl --silent --show-error https://dl.yarnpkg.com/debian/pubkey.gpg | gpg --dearmor > /etc/apt/trusted.gpg.d/yarn.gpg

RUN apt-get update

RUN curl -fsSL https://deb.nodesource.com/setup_20.x | bash - \
    && apt update \
    && apt install nodejs -y

RUN npm install -g yarn

RUN cd "$GOPATH/src" \
    && git -c advice.detachedHead=false clone --branch "v$VERSION" --recursive --jobs "$(nproc)" https://github.com/free5gc/free5gc.git

RUN cd "$GOPATH/src/free5gc" \
    && make

RUN apt-get -y install sudo

RUN cd "$GOPATH/src/free5gc" \
    && make webconsole

#
# Image
#

FROM $BUILDER_IMAGE

WORKDIR /free5gc

RUN mkdir --parents ./config ./public ./cert

# Copy executables
COPY --from=builder /go/src/free5gc/bin/* ./
COPY --from=builder /go/src/free5gc/webconsole/bin/webconsole ./webui

# Copy static files (webconsole frontend)
COPY --from=builder /go/src/free5gc/webconsole/public ./public

# Copy certificates and config files
COPY --from=builder /go/src/free5gc/config/* ./config/
COPY --from=builder /go/src/free5gc/cert/* ./cert/

