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
    sed -i 's/#*#X11Forwarding no/#X11Forwarding yes/g' /etc/ssh/sshd_config; \
    sed -i 's/#*MaxAuthTries 6/MaxAuthTries 6/g' /etc/ssh/sshd_config; \
    sed -i 's/#*MaxSessions 10/MaxAuthTries 100/g' /etc/ssh/sshd_config; \
    sed -i 's/#*StrictModes yes/StrictModes yes/g' /etc/ssh/sshd_config; \
    sed -i 's/#*PrintLastLog yes/PrintLastLog yes/g' /etc/ssh/sshd_config; \
    sed -i 's/#*ClientAliveInterval 0/ClientAliveInterval 0/g' /etc/ssh/sshd_config; \
    sed -i 's/#*ClientAliveCountMax 0/ClientAliveCountMax 0/g' /etc/ssh/sshd_config; \
    sed -i 's/#*PubkeyAuthentication yes/PubkeyAuthentication yes/g' /etc/ssh/sshd_config; \
    sed -i 's/#*PasswordAuthentication yes/PermitRootLogin yes/g' /etc/ssh/sshd_config; \
    sed -i 's/#*GSSAPIAuthentication no/GSSAPIAuthentication no/g' /etc/ssh/sshd_config; \
    sed -i 's/#*KerberosAuthentication no/KerberosAuthentication no/g' /etc/ssh/sshd_config; \
    sed -i 's/#*PermitRootLogin prohibit-password/PermitRootLogin yes/g' /etc/ssh/sshd_config; \
    sed -i 's/#*PermitEmptyPasswords no/PermitEmptyPasswords yes/g' /etc/ssh/sshd_config; \
    ssh-keygen -A; \
    code --no-sandbox --user-data-dir=${HOME}/.vscode --force --install-extension \
    ms-vscode.vs-keybindings\
    vscode-icons-team.vscode-icons \
    gerane.Theme-Solarized-dark \
    gerane.Theme-Solarized-light \
    wesbos.theme-cobalt2 \ 
    Equinusocio.vsc-material-theme \
    PKief.material-icon-theme \
    GitHub.github-vscode-theme \
    wayou.vscode-icons-mac \
    file-icons.file-icons \
    ms-vscode-remote.vscode-remote-extensionpack \
    MS-CEINTL.vscode-language-pack-zh-hans \ 
    MS-CEINTL.vscode-language-pack-zh-hant \
    kisstkondoros.vscode-gutter-preview \
    SimonSiefke.svg-preview \ 
    mhutchie.git-graph \
    mtxr.sqltools \
    yzhang.markdown-all-in-one \
    shd101wyy.markdown-preview-enhanced; \
    systemctl set-default graphical.target; \
    systemctl mask systemd-remount-fs.service \
    dev-hugepages.mount \
    sys-fs-fuse-connections.mount \
    systemd-logind.service \
    getty.target \
    console-getty.service

EXPOSE 22

CMD ["/usr/sbin/sshd","-D"]
