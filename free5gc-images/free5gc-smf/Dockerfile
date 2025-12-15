ARG VERSION=4.1.0
ARG REGISTRY=docker.io/nephio
ARG BASE_IMAGE=alpine

FROM $REGISTRY/free5gc-base:$VERSION AS builder
FROM $BASE_IMAGE

ENV GIN_MODE="release"

WORKDIR /free5gc
RUN mkdir --parents ./log/ ./config/ ./cert/

# Copy executable and default certs
COPY --from=builder /free5gc/smf ./
COPY --from=builder /free5gc/config/smfcfg.yaml ./config/
COPY --from=builder /free5gc/config/uerouting.yaml ./config/
COPY --from=builder /free5gc/cert/* ./cert/

VOLUME [ "/free5gc/config" ]
VOLUME [ "/free5gc/cert" ]

