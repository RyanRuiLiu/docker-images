FROM alpine:3.24.4 AS download
ARG SOPS_VERSION=3.13.3
RUN apk add --no-cache ca-certificates \
    && cd /tmp \
    && wget -q "https://github.com/getsops/sops/releases/download/v${SOPS_VERSION}/sops-v${SOPS_VERSION}.linux.amd64" \
    && wget -q -O checksums.txt "https://github.com/getsops/sops/releases/download/v${SOPS_VERSION}/sops-v${SOPS_VERSION}.checksums.txt" \
    && grep " sops-v${SOPS_VERSION}.linux.amd64$" checksums.txt | sha256sum -c - \
    && mkdir -p /out \
    && cp "sops-v${SOPS_VERSION}.linux.amd64" /out/sops \
    && chmod 755 /out/sops

FROM ubuntu:noble
COPY --from=download /out/sops /usr/local/bin/sops
