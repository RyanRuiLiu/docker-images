FROM ubuntu:noble
ARG SOPS_VERSION=3.13.3
RUN apt-get update \
    && apt-get install -y --no-install-recommends ca-certificates curl \
    && curl -fsSL "https://github.com/getsops/sops/releases/download/v${SOPS_VERSION}/sops_${SOPS_VERSION}_amd64.deb" -o /tmp/sops.deb \
    && dpkg -i /tmp/sops.deb \
    && rm /tmp/sops.deb \
    && apt-get purge -y curl \
    && apt-get autoremove -y \
    && rm -rf /var/lib/apt/lists/*
