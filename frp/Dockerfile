FROM --platform=$BUILDPLATFORM golang:alpine AS builder

ARG TARGETPLATFORM
ARG TARGETOS
ARG TARGETARCH=
ARG VERSION
ARG CGO_ENABLED=on

ENV GOOS=$TARGETOS \
    GOARCH=$TARGETARCH

RUN apk add --no-cache curl build-base git && \
    mkdir -p /root/frp && cd /root/frp && \
    curl -sSL https://github.com/fatedier/frp/archive/v$VERSION.tar.gz | \
    tar xz --strip 1 && \
    go build -v -ldflags "-s -w" -o /frps ./cmd/frps && \
    go build -v -ldflags "-s -w" -o /frpc ./cmd/frpc && \
    cp ./conf/frp* /etc/

FROM alpine:3.11

COPY --from=builder /frp* /usr/bin/
COPY --from=builder /etc/frp* /etc/

CMD ["/usr/bin/frps", "-c", "/etc/frps.ini"]