FROM ghcr.io/snowdreamtech/base-develop-amd64:0.0.2

LABEL maintainer="sn0wdr1am <sn0wdr1am@qq.com>"

ENV GOLANG_VERSION 1.20.2

RUN  dnf install \
    --nogpgcheck --repoid=AppStream --repoid=BaseOS --repoid=Mercurial \
    --repofrompath='BaseOS,https://repo.almalinux.org/almalinux/9/BaseOS/$basearch/os/' \
    --repofrompath='AppStream,https://repo.almalinux.org/almalinux/9/AppStream/$basearch/os/' \
    --repofrompath='Mercurial,https://www.mercurial-scm.org/release/centos8/' \
    -y \
    curl \
    git \
    make \
    bison \
    gcc \
    glibc-devel \
    mercurial; \
    bash < <(curl -s -S -L https://raw.githubusercontent.com/moovweb/gvm/master/binscripts/gvm-installer) \
    && source ~/.bashrc \
    && gvm install go${GOLANG_VERSION} -B \
    && gvm use go${GOLANG_VERSION} --default;
