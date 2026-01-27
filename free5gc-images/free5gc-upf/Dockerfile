ARG VERSION=4.1.0
ARG REGISTRY=docker.io/nephio
ARG BASE_IMAGE=alpine

FROM $REGISTRY/free5gc-base:$VERSION AS builder

# GoGTP5G Tunnel for UPF
RUN git clone https://github.com/free5gc/go-gtp5gnl.git && \
    mkdir "go-gtp5gnl/bin" && \
    cd "go-gtp5gnl/cmd/gogtp5g-tunnel" && \
    go build -o "${GOPATH}/gtp5g-tunnel" . && \
    cd -


FROM $BASE_IMAGE

ENV GIN_MODE="release"

# Install UPF dependencies
RUN apk --no-cache add iproute2 iptables

WORKDIR /free5gc
RUN mkdir --parents ./config/

# Copy executable and default configs/certs
COPY --from=builder /free5gc/upf ./
COPY --from=builder /free5gc/config/upfcfg.yaml ./config/
COPY --from=builder /go/gtp5g-tunnel ./gtp5g-tunnel
COPY --from=builder /free5gc/cert/* ./cert/

VOLUME [ "/free5gc/config" ]

