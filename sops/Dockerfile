FROM alpine:3.24.4 AS download
ARG SOPS_VERSION=3.13.3
RUN apk add --no-cache ca-certificates \
    && cd /tmp \
    && wget -q "https://github.com/getsops/sops/releases/download/v${SOPS_VERSION}/sops-v${SOPS_VERSION}.linux.amd64" \
    && wget -q -O checksums.txt "https://github.com/getsops/sops/releases/download/v${SOPS_VERSION}/sops-v${SOPS_VERSION}.checksums.txt" \
    && grep " sops-v${SOPS_VERSION}.linux.amd64$" checksums.txt | sha256sum -c - \
    && cp "sops-v${SOPS_VERSION}.linux.amd64" /tmp/sops

FROM ubuntu:noble
COPY --chmod=755 --from=download /tmp/sops /usr/local/bin/sops
