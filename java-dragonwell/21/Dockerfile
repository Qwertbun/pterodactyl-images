FROM alibabadragonwell/dragonwell:21-ubuntu

LABEL author="Softwarenoob" maintainer="admin@softwarenoob.com"
LABEL org.opencontainers.image.source="https://github.com/Software-Noob/pterodactyl-images"
LABEL org.opencontainers.image.licenses="MIT"

# Определяем версию Ubuntu для правильной установки libssl3
RUN if grep -q "20.04" /etc/os-release; then \
        echo "🔧 Ubuntu 20.04 detected. Adding repository for libssl3..." && \
        apt-get update && \
        apt-get install -y --no-install-recommends software-properties-common && \
        add-apt-repository ppa:ondrej/php -y && \
        apt-get update; \
    elif grep -q "22.04" /etc/os-release; then \
        echo "✅ Ubuntu 22.04 detected. libssl3 available in default repositories."; \
    fi && \
    \
    # Установка всех зависимостей
    DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends \
        curl ca-certificates \
        libssl3 openssl \
        git tar sqlite3 fontconfig libfreetype6 libstdc++6 lsof \
        build-essential tzdata iproute2 locales \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* \
    && useradd -m -d /home/container container \
    && locale-gen en_US.UTF-8 \
    && \
    # Проверка установки libssl.so.3
    if find /usr -name "libssl.so.3" | grep -q "libssl.so.3"; then \
        echo "✅ libssl.so.3 успешно установлена"; \
    else \
        echo "❌ libssl.so.3 не найдена! Пытаемся установить вручную из исходников..."; \
        apt-get update && \
        apt-get install -y --no-install-recommends wget build-essential zlib1g-dev && \
        cd /tmp && \
        wget https://www.openssl.org/source/openssl-3.0.14.tar.gz && \
        tar -xzf openssl-3.0.14.tar.gz && \
        cd openssl-3.0.14 && \
        ./config --prefix=/usr/local/ssl --openssldir=/usr/local/ssl shared && \
        make -j$(nproc) && \
        make install && \
        echo '/usr/local/ssl/lib64' > /etc/ld.so.conf.d/openssl-3.conf && \
        ldconfig && \
        ln -sf /usr/local/ssl/lib64/libssl.so.3 /usr/lib/x86_64-linux-gnu/libssl.so.3 && \
        ln -sf /usr/local/ssl/lib64/libcrypto.so.3 /usr/lib/x86_64-linux-gnu/libcrypto.so.3 && \
        echo "✅ OpenSSL 3 успешно скомпилирован и установлен"; \
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
