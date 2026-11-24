FROM alpine:3.24.4 AS download
ARG SOPS_VERSION=3.13.3
ARG TARGETARCH
WORKDIR /tmp
# hadolint ignore=DL3018
RUN apk add --no-cache ca-certificates \
    && wget -q "https://github.com/getsops/sops/releases/download/v${SOPS_VERSION}/sops-v${SOPS_VERSION}.linux.${TARGETARCH}" \
    && wget -q -O checksums.txt "https://github.com/getsops/sops/releases/download/v${SOPS_VERSION}/sops-v${SOPS_VERSION}.checksums.txt" \
    && grep " sops-v${SOPS_VERSION}.linux.${TARGETARCH}$" checksums.txt > sops-checksum.txt \
    && sha256sum -c sops-checksum.txt \
    && cp "sops-v${SOPS_VERSION}.linux.${TARGETARCH}" /tmp/sops

FROM ubuntu:26.04
COPY --chmod=755 --from=download /tmp/sops /usr/local/bin/sops
