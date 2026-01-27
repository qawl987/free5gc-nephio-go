ARG VERSION=4.1.0
ARG REGISTRY=docker.io/nephio
ARG BASE_IMAGE=alpine

FROM $REGISTRY/free5gc-base:$VERSION AS builder
FROM $BASE_IMAGE

ENV GIN_MODE="release"

# Install N3IWF dependencies
RUN apk add -U iproute2 

WORKDIR /free5gc
RUN mkdir --parents ./log/ ./cert/

# Copy executable and default certs/configs
COPY --from=builder /free5gc/n3iwf ./
COPY --from=builder /free5gc/config/n3iwfcfg.yaml ./config/
COPY --from=builder /free5gc/cert/* ./cert/

VOLUME [ "/free5gc/config" ]
VOLUME [ "/free5gc/cert" ]

