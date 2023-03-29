FROM ghcr.io/snowdreamtech/base-develop-amd64:0.0.2

LABEL maintainer="sn0wdr1am <sn0wdr1am@qq.com>"

ENV NODEJS_VERSION 18.15.0

RUN wget -qO- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.3/install.sh | bash \
    && source ~/.bashrc \
    && nvm install ${NODEJS_VERSION} \
    && nvm use ${NODEJS_VERSION} \
    && npm install -g pnpm \
    && npm install -g yarn \
    && npm install -g nrm;