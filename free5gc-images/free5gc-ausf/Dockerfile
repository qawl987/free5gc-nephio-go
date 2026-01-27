ARG VERSION=4.1.0
ARG REGISTRY=docker.io/nephio
ARG BASE_IMAGE=alpine

FROM $REGISTRY/free5gc-base:$VERSION AS builder
FROM $BASE_IMAGE

LABEL description="Free5GC open source 5G Core Network" \
    version="Stage 3"

ENV GIN_MODE="release"

WORKDIR /free5gc
RUN mkdir --parents ./log/ ./cert/

# Copy executable and default certs
COPY --from=builder /free5gc/ausf ./
COPY --from=builder /free5gc/config/ausfcfg.yaml ./config/
COPY --from=builder /free5gc/cert/* ./cert/

VOLUME [ "/free5gc/config" ]
VOLUME [ "/free5gc/cert" ]

