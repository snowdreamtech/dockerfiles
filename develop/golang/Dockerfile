FROM ghcr.io/snowdreamtech/base-develop-amd64:0.0.2

LABEL maintainer="sn0wdr1am <sn0wdr1am@qq.com>"

ENV GOLANG_VERSION 1.20.2

RUN bash < <(curl -s -S -L https://raw.githubusercontent.com/moovweb/gvm/master/binscripts/gvm-installer) \
    && source ~/.bashrc \
    && gvm install go${GOLANG_VERSION} \
    && gvm use go${GOLANG_VERSION} --default;
