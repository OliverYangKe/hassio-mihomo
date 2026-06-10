FROM alpine:latest

# Install required packages
RUN apk add --no-cache ca-certificates bash

# Create app directory
WORKDIR /app

# Copy architecture-specific binary
RUN apk add --no-cache curl
   ARG BUILD_ARCH
   RUN set -e; \
       case "${BUILD_ARCH}" in amd64) M=amd64;; aarch64) M=arm64;; armv7) M=armv7;; *) M=amd64;; esac; \
       VER=$(curl -sL -o /dev/null -w '%{url_effective}' https://github.com/MetaCubeX/mihomo/releases/latest | sed 's#.*/##'); \
       curl -fsSL "https://github.com/MetaCubeX/mihomo/releases/download/${VER}/mihomo-linux-${M}-${VER}.gz" -o /tmp/m.gz; \
       gunzip -c /tmp/m.gz > /app/mihomo; rm -f /tmp/m.gz
COPY start-mihomo.sh /app/start-mihomo.sh
COPY mihomo-default.yaml /app/mihomo-default.yaml

# Make scripts executable
RUN chmod +x /app/mihomo /app/start-mihomo.sh

# Run the startup script
CMD ["/app/start-mihomo.sh"]
