FROM ubuntu:20.04

ENV DEBIAN_FRONTEND=noninteractive

ARG USERNAME=example
ARG UID=1000
ARG GID=1000
ARG AUTHORIZED_KEY=""

RUN cp /etc/apt/sources.list /etc/apt/sources.list.bak && \
    sed -i 's/archive.ubuntu.com/mirrors.tuna.tsinghua.edu.cn/g' /etc/apt/sources.list

RUN apt update -y && apt install -y ack antlr3 asciidoc autoconf automake autopoint binutils bison build-essential \
          bzip2 ccache clang clangd cmake cpio curl device-tree-compiler ecj fastjar flex gawk gettext gcc-multilib \
          g++-multilib git gnutls-dev gperf haveged help2man intltool lib32gcc-s1 libc6-dev-i386 libelf-dev \
          libglib2.0-dev libgmp3-dev libltdl-dev libmpc-dev libmpfr-dev libncurses5-dev libncursesw5 \
          libncursesw5-dev libncurses-dev libpython3-dev libreadline-dev libssl-dev libtool libyaml-dev libz-dev \
          lld lldb llvm lrzsz mkisofs msmtp nano ninja-build p7zip p7zip-full patch pkgconf python2.7 python3 \
          python3-pip python3-ply python3-docutils python3-pyelftools qemu-utils re2c rsync scons squashfs-tools \
          subversion swig texinfo uglifyjs upx-ucl unzip vim wget xmlto xxd zlib1g-dev zstd

RUN apt install -y openssh-server sudo

RUN mkdir /var/run/sshd

RUN ssh-keygen -A

RUN groupadd -g ${GID} ${USERNAME} && \
    useradd -m -u ${UID} -g ${GID} -s /bin/bash ${USERNAME} && \
    echo "${USERNAME} ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers

RUN mkdir -p /home/${USERNAME}/.ssh && \
    chmod 700 /home/${USERNAME}/.ssh && \
    chown -R ${USERNAME}:${USERNAME} /home/${USERNAME}/.ssh

RUN sed -i \
    -e 's/#PasswordAuthentication yes/PasswordAuthentication no/' \
    -e 's/#PermitRootLogin prohibit-password/PermitRootLogin no/' \
    /etc/ssh/sshd_config

RUN sed -i 's@session\s*required\s*pam_loginuid.so@session optional pam_loginuid.so@g' \
    /etc/pam.d/sshd

RUN echo "${AUTHORIZED_KEY}" > /home/${USERNAME}/.ssh/authorized_keys && \
    chown ${USERNAME}:${USERNAME} /home/${USERNAME}/.ssh/authorized_keys && \
    chmod 600 /home/${USERNAME}/.ssh/authorized_keys

EXPOSE 22

CMD ["/usr/sbin/sshd", "-D"]