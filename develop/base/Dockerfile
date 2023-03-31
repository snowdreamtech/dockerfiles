ARG SYSBASE=almalinux:9
FROM ${SYSBASE} as system-build

RUN mkdir /mnt/sys-root; \
    dnf install \
    --nogpgcheck --repoid=AppStream --repoid=BaseOS --repoid=VsCode \
    --repofrompath='BaseOS,https://repo.almalinux.org/almalinux/9/BaseOS/$basearch/os/' \
    --repofrompath='AppStream,https://repo.almalinux.org/almalinux/9/AppStream/$basearch/os/' \
    --repofrompath='VsCode,https://packages.microsoft.com/yumrepos/vscode' \
    --installroot /mnt/sys-root \
    --releasever 9 \
    --setopt install_weak_deps=false \
    --nodocs -y \
    which \ 
    git \ 
    openssh-server \ 
    openssh-clients \ 
    code \
    ; \
    dnf --installroot /mnt/sys-root clean all; \
    rm -rf /mnt/sys-root/var/log/dnf* /mnt/sys-root/var/log/yum.*; \
    rm -rf /mnt/sys-root/var/lib/dnf/history* /mnt/sys-root/var/log/hawkey.log  /mnt/sys-root/boot /mnt/sys-root/dev/null /mnt/sys-root/run/*; \
    mkdir -p /mnt/sys-root/run/lock; \
    # rm -rf /mnt/sys-root/var/cache/*  removed line
    # cp /etc/yum.repos.d/*.repo /mnt/sys-root/etc/yum.repos.d/; \
    # generate build time file for compatibility with CentOS
    /bin/date +%Y%m%d_%H%M > /mnt/sys-root/etc/BUILDTIME; \
    echo '%_install_langs C.utf8' > /mnt/sys-root/etc/rpm/macros.image-language-conf; \
    echo 'LANG="C.utf8"' >  /mnt/sys-root/etc/locale.conf; \
    echo 'container' > /mnt/sys-root/etc/dnf/vars/infra; \
    rm -f /mnt/sys-root/etc/machine-id; \
    touch /mnt/sys-root/etc/machine-id;

# Almalinux default build
FROM ${SYSBASE} 
COPY --from=system-build /mnt/sys-root/ /

LABEL maintainer="sn0wdr1am <sn0wdr1am@qq.com>"

ENV LANG=C.utf8 \
    DISPLAY=host.docker.internal:0.0 \
    DONT_PROMPT_WSL_INSTALL=true

RUN dnf groupinstall -y "Server with GUI"; \
    sed -i 's/#*#X11Forwarding yes/#X11Forwarding yes/g' /etc/ssh/sshd_config; \
    sed -i 's/#*MaxAuthTries 6/MaxAuthTries 6/g' /etc/ssh/sshd_config; \
    sed -i 's/#*MaxSessions 10/MaxAuthTries 100/g' /etc/ssh/sshd_config; \
    sed -i 's/#*StrictModes yes/StrictModes yes/g' /etc/ssh/sshd_config; \
    sed -i 's/#*PrintLastLog yes/PrintLastLog yes/g' /etc/ssh/sshd_config; \
    sed -i 's/#*ClientAliveInterval 0/ClientAliveInterval 0/g' /etc/ssh/sshd_config; \
    sed -i 's/#*ClientAliveCountMax 3/ClientAliveCountMax 3/g' /etc/ssh/sshd_config; \
    sed -i 's/#*PubkeyAuthentication yes/PubkeyAuthentication yes/g' /etc/ssh/sshd_config; \
    sed -i 's/#*PasswordAuthentication yes/PermitRootLogin yes/g' /etc/ssh/sshd_config; \
    sed -i 's/#*GSSAPIAuthentication no/GSSAPIAuthentication no/g' /etc/ssh/sshd_config; \
    sed -i 's/#*KerberosAuthentication no/KerberosAuthentication no/g' /etc/ssh/sshd_config; \
    sed -i 's/#*PermitRootLogin prohibit-password/PermitRootLogin yes/g' /etc/ssh/sshd_config; \
    sed -i 's/#*PermitEmptyPasswords no/PermitEmptyPasswords yes/g' /etc/ssh/sshd_config; \
    ssh-keygen -A; \
    code --no-sandbox --user-data-dir=${HOME}/.vscode --force --install-extension ms-vscode.vs-keybindings \
    && code --no-sandbox --user-data-dir=${HOME}/.vscode --force --install-extension vscode-icons-team.vscode-icons \
    && code --no-sandbox --user-data-dir=${HOME}/.vscode --force --install-extension ryanolsonx.solarized \
    && code --no-sandbox --user-data-dir=${HOME}/.vscode --force --install-extension wesbos.theme-cobalt2 \ 
    && code --no-sandbox --user-data-dir=${HOME}/.vscode --force --install-extension Equinusocio.vsc-material-theme \
    && code --no-sandbox --user-data-dir=${HOME}/.vscode --force --install-extension PKief.material-icon-theme \
    && code --no-sandbox --user-data-dir=${HOME}/.vscode --force --install-extension GitHub.github-vscode-theme \
    && code --no-sandbox --user-data-dir=${HOME}/.vscode --force --install-extension wayou.vscode-icons-mac \
    && code --no-sandbox --user-data-dir=${HOME}/.vscode --force --install-extension file-icons.file-icons \
    && code --no-sandbox --user-data-dir=${HOME}/.vscode --force --install-extension ms-vscode-remote.vscode-remote-extensionpack \
    && code --no-sandbox --user-data-dir=${HOME}/.vscode --force --install-extension MS-CEINTL.vscode-language-pack-zh-hans \ 
    && code --no-sandbox --user-data-dir=${HOME}/.vscode --force --install-extension MS-CEINTL.vscode-language-pack-zh-hant \
    && code --no-sandbox --user-data-dir=${HOME}/.vscode --force --install-extension kisstkondoros.vscode-gutter-preview \
    && code --no-sandbox --user-data-dir=${HOME}/.vscode --force --install-extension SimonSiefke.svg-preview \ 
    && code --no-sandbox --user-data-dir=${HOME}/.vscode --force --install-extension mhutchie.git-graph \
    && code --no-sandbox --user-data-dir=${HOME}/.vscode --force --install-extension mtxr.sqltools \
    && code --no-sandbox --user-data-dir=${HOME}/.vscode --force --install-extension yzhang.markdown-all-in-one \
    && code --no-sandbox --user-data-dir=${HOME}/.vscode --force --install-extension shd101wyy.markdown-preview-enhanced \
    && code --no-sandbox --user-data-dir=${HOME}/.vscode --force --install-extension aaron-bond.better-comments \
    ; \
    systemctl set-default graphical.target; \
    systemctl mask systemd-remount-fs.service \
    dev-hugepages.mount \
    sys-fs-fuse-connections.mount \
    systemd-logind.service \
    getty.target \
    console-getty.service

EXPOSE 22

CMD ["/usr/sbin/sshd","-D"]
