FROM ubuntu:latest

ARG DEBIAN_FRONTEND=noninteractive
RUN set -x && \
    apt-get update && \
    apt-get install --yes --no-install-recommends apt-cacher-ng ca-certificates wget && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

ARG ENV_HTTP_PROXY=$http_proxy
ENV http_proxy="$ENV_HTTP_PROXY" \
    https_proxy="$ENV_HTTP_PROXY"

RUN set -x && \
    echo "ForeGround: 1" >> /etc/apt-cacher-ng/acng.conf && \
    echo "Proxy: $ENV_HTTP_PROXY" >> /etc/apt-cacher-ng/acng.conf && \
    echo "PassThroughPattern: .*" >> /etc/apt-cacher-ng/acng.conf && \
    ln -s /dev/stdout /var/log/apt-cacher-ng/apt-cacher.log && \
    ln -s /dev/stderr /var/log/apt-cacher-ng/apt-cacher.err

HEALTHCHECK --interval=10s --timeout=2s --retries=3 \
    CMD http_proxy= wget -q -t1 -O /dev/null http://localhost:3142/acng-report.html || exit 1

CMD ["/usr/sbin/apt-cacher-ng", "-c", "/etc/apt-cacher-ng"]
