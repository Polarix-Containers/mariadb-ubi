ARG VERSION=11.4
ARG UID=200011
ARG GID=200011

FROM mariadb:${VERSION}-ubi

ARG UID
ARG GID

LABEL maintainer="Thien Tran contact@tommytran.io"

USER root

RUN microdnf -y upgrade \
    && rm -rf /usr/local/bin/gosu \
    && curl https://copr.fedorainfracloud.org/coprs/secureblue/hardened_malloc/repo/rhel-9/secureblue-hardened_malloc-rhel-9.repo \
        -o /etc/yum.repos.d/_copr:copr.fedorainfracloud.org:secureblue:hardened_malloc.repo \
    && microdnf install -y crypto-policies-scripts hardened_malloc shadow-utils \
    && update-crypto-policies --set FUTURE \
    && echo libhardened_malloc.so > /etc/ld.so.preload

RUN --network=none \
    usermod -u ${UID} mysql \
    && groupmod -g ${GID} mysql \
    && find / -path '/proc' -prune -user 999 -exec chown -h mysql {} \; \
    && find / -path '/proc' -prune -group 999 -exec chgrp -h mysql {} \; \
    && microdnf remove -y shadow-utils

USER mysql