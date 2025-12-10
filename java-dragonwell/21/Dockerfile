# syntax=docker/dockerfile:1.7-labs

# ---------------------------------------------------------
#   BASE IMAGE: Alibaba Dragonwell 21 (Ubuntu Focal)
#   Multi-arch: amd64 / arm64 handled automatically by buildx
# ---------------------------------------------------------

FROM alibabadragonwell/dragonwell:21-ubuntu

# ---------------------------------------------------------
#   SYSTEM SETUP
# ---------------------------------------------------------

ENV DEBIAN_FRONTEND=noninteractive \
    LANG=en_US.UTF-8 \
    LC_ALL=en_US.UTF-8 \
    TZ=Etc/UTC

RUN --mount=type=cache,target=/var/cache/apt \
    --mount=type=cache,target=/var/lib/apt \
    apt-get update -y \
    && apt-get install -y --no-install-recommends \
        ca-certificates \
        curl \
        openssl \
        libssl3 \
        git \
        sqlite3 \
        tar \
        lsof \
        iproute2 \
        fontconfig \
        libfreetype6 \
        libstdc++6 \
        tzdata \
        locales \
    && locale-gen en_US.UTF-8 \
    && useradd -m -d /home/container container \
    && rm -rf /var/lib/apt/lists/*

# Проверка SSL (важно для Oxide / Pterodactyl)
RUN if [ ! -f "/usr/lib/$(uname -m)-linux-gnu/libssl.so.3" ]; then \
        echo "❌ libssl.so.3 not found!"; exit 1; \
    else \
        echo "✅ libssl.so.3 OK"; \
    fi

# ---------------------------------------------------------
#   Pterodactyl requirements
# ---------------------------------------------------------

USER container
WORKDIR /home/container

# Entry point
COPY ./entrypoint.sh /entrypoint.sh
RUN chmod +x /entrypoint.sh

ENTRYPOINT ["/entrypoint.sh"]
CMD ["bash"]
