FROM bitnami/minideb:stretch as buildenv

RUN install_packages ca-certificates wget
RUN wget -nc -P /tmp/bitnami/pkg/cache/ https://downloads.bitnami.com/files/stacksmith/alertmanager-0.15.0-0-linux-amd64-debian-9.tar.gz && \
    echo "7004edc8245bec63685fe9d09aaac3da31e9216ff88d1a415f9e71fc7b14286c  /tmp/bitnami/pkg/cache/alertmanager-0.15.0-0-linux-amd64-debian-9.tar.gz" | sha256sum -c - && \
    tar -zxf /tmp/bitnami/pkg/cache/alertmanager-0.15.0-0-linux-amd64-debian-9.tar.gz -P --transform 's|^[^/]*/files|/opt/bitnami|' --wildcards '*/files' && \
    rm -rf /tmp/bitnami/pkg/cache/alertmanager-0.15.0-0-linux-amd64-debian-9.tar.gz

##################

FROM bitnami/minideb:stretch
LABEL maintainer "Bitnami <containers@bitnami.com>"

COPY --from=buildenv /opt/bitnami/alertmanager /opt/bitnami/alertmanager
ENV BITNAMI_APP_NAME="alertmanager" \
    BITNAMI_IMAGE_VERSION="0.15.0-debian-9-r2" \
    PATH="/opt/bitnami/alertmanager/bin:$PATH"

RUN mkdir -p /opt/bitnami/alertmanager/data/ && chmod g+rwX /opt/bitnami/alertmanager/data/

EXPOSE 9093

WORKDIR /opt/bitnami/alertmanager
USER 1001
ENTRYPOINT ["/opt/bitnami/alertmanager/bin/alertmanager"]
CMD ["--config.file=/opt/bitnami/alertmanager/conf/config.yml","--storage.path=/opt/bitnami/alertmanager/data"]