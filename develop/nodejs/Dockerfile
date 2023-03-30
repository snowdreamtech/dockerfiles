FROM ghcr.io/snowdreamtech/base-develop-amd64:0.0.3

LABEL maintainer="sn0wdr1am <sn0wdr1am@qq.com>"

ENV NODEJS_VERSION 18.15.0

RUN wget -qO- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.3/install.sh | bash \
    && source ~/.bashrc \
    && nvm install ${NODEJS_VERSION} \
    && nvm use ${NODEJS_VERSION} \
    && npm install -g pnpm \
    && pnpm setup \
    && source ~/.bashrc \
    && npm install -g yarn \
    && npm install -g nrm open@8.4.2 --save; \
    code --no-sandbox --user-data-dir=${HOME}/.vscode --force --install-extension dbaeumer.vscode-eslint \
    && code --no-sandbox --user-data-dir=${HOME}/.vscode --force --install-extension formulahendry.auto-close-tag \
    && code --no-sandbox --user-data-dir=${HOME}/.vscode --force --install-extension formulahendry.auto-rename-tag \
    && code --no-sandbox --user-data-dir=${HOME}/.vscode --force --install-extension xabikos.JavaScriptSnippets \
    && code --no-sandbox --user-data-dir=${HOME}/.vscode --force --install-extension christian-kohler.path-intellisense \
    && code --no-sandbox --user-data-dir=${HOME}/.vscode --force --install-extension ecmel.vscode-html-css \
    && code --no-sandbox --user-data-dir=${HOME}/.vscode --force --install-extension christian-kohler.npm-intellisense \
    && code --no-sandbox --user-data-dir=${HOME}/.vscode --force --install-extension esbenp.prettier-vscode \
    && code --no-sandbox --user-data-dir=${HOME}/.vscode --force --install-extension HookyQR.beautify \ 
    ;
