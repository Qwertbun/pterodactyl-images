FROM alibabadragonwell/dragonwell:21-ubuntu

LABEL author="Softwarenoob" maintainer="admin@softwarenoob.com"
LABEL org.opencontainers.image.source="https://github.com/Software-Noob/pterodactyl-images"
LABEL org.opencontainers.image.licenses="MIT"

# Установка всех необходимых зависимостей, включая libssl3
RUN apt-get update -y \
    && DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends \
        curl ca-certificates \
        libssl3 openssl \
        git tar sqlite3 fontconfig libfreetype6 libstdc++6 lsof \
        build-essential tzdata iproute2 locales \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* \
    && useradd -m -d /home/container container \
    && locale-gen en_US.UTF-8 \
    # Проверка установки libssl3
    && if [ -f "/usr/lib/x86_64-linux-gnu/libssl.so.3" ]; then \
        echo "✅ libssl.so.3 успешно установлена"; \
    else \
        echo "❌ libssl.so.3 не найдена! Проверьте установку."; \
        exit 1; \
    fi

ENV LC_ALL=en_US.UTF-8
ENV LANG=en_US.UTF-8
ENV LANGUAGE=en_US.UTF-8

USER container
ENV USER=container HOME=/home/container
WORKDIR /home/container

COPY ./entrypoint.sh /entrypoint.sh
RUN chmod +x /entrypoint.sh

CMD ["/entrypoint.sh"]
