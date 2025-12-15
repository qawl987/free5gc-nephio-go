ARG VERSION=4.1.0
ARG REGISTRY=docker.io/nephio
ARG BASE_IMAGE=alpine

FROM $REGISTRY/free5gc-base:$VERSION AS builder
FROM $BASE_IMAGE

ENV GIN_MODE="release"

WORKDIR /free5gc
RUN mkdir --parents ./log/ ./config/ ./cert/

# Copy executable and default certs/configs
COPY --from=builder /free5gc/amf ./
COPY --from=builder /free5gc/config/amfcfg.yaml ./config/
COPY --from=builder /free5gc/cert/* ./cert/

VOLUME [ "/free5gc/config" ]
VOLUME [ "/free5gc/cert" ]


