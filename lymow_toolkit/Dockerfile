# Lymow Toolkit — Home Assistant add-on image. Built ON the user's HA machine at install time
# (no prebuilt registry): it downloads the OFFICIAL versioned docker bundle from the public
# GitHub release — the exact obfuscated bits every other platform ships — and applies the same
# proven steps as the standalone Docker build. python:3.12-slim is multi-arch, so this builds
# natively on amd64 / aarch64 (Raspberry Pi 4-5, ODROID) / armv7 HA boxes.
ARG BUILD_FROM=ghcr.io/home-assistant/amd64-base:latest
FROM python:3.12-slim

ARG LYMOW_VERSION=1.37.0-mapheight
WORKDIR /app

# Build from the user's fork/branch instead of the official release tarball
ARG LYMOW_REPO="https://github.com/MasterCATZ/lymow-toolkit-downloads.git"
ARG LYMOW_BRANCH="feat/map-height-slider"
RUN apt-get update && apt-get install -y --no-install-recommends git ca-certificates \
    && rm -rf /var/lib/apt/lists/* \
    && git clone --depth 1 --branch "$LYMOW_BRANCH" "$LYMOW_REPO" /tmp/lymow-src \
    && cp -a /tmp/lymow-src/. /app/ \
    && rm -rf /tmp/lymow-src \
    && pip install --no-cache-dir -r requirements.txt

# /data is Home Assistant's persistent add-on volume: login/config/backups survive updates & rebuilds
ENV LYMOW_HOST=0.0.0.0 LYMOW_PORT=8787 LYMOW_RELOAD=0 PYTHONUNBUFFERED=1 LYMOW_DATA=/data
EXPOSE 8787
CMD ["python", "-m", "app.run"]
