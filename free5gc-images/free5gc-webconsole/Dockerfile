ARG VERSION=4.1.0
ARG REGISTRY=docker.io/nephio
ARG BASE_IMAGE=alpine

FROM $REGISTRY/free5gc-base:$VERSION AS builder

FROM $BASE_IMAGE

ENV GIN_MODE="release"

RUN apk --no-cache add libc6-compat

WORKDIR /free5gc
RUN mkdir --parents ./config/ ./cert/ ./public

# Copy executable and default configs/statics
COPY --from=builder /free5gc/webui ./
COPY --from=builder /free5gc/config/webuicfg.yaml ./config/
COPY --from=builder /free5gc/public ./public
COPY --from=builder /free5gc/cert/* ./cert/

VOLUME [ "/free5gc/config" ]
VOLUME [ "/free5gc/cert" ]

