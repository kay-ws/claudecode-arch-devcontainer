FROM docker.io/library/archlinux:latest

RUN pacman -Syu --noconfirm \
 && pacman -S --noconfirm --needed \
      ca-certificates curl git openssh \
      sudo less which jq \
      ripgrep fd \
 && pacman -Scc --noconfirm \
 && rm -rf /var/cache/pacman/pkg/*

ARG USERNAME=claude
ARG USER_UID=1000
ARG USER_GID=1000
RUN groupadd --gid ${USER_GID} ${USERNAME} \
 && useradd  --uid ${USER_UID} --gid ${USER_GID} --create-home --shell /bin/bash ${USERNAME} \
 && install -d -o ${USERNAME} -g ${USERNAME} -m 0750 /home/${USERNAME}/workspace \
 && echo "${USERNAME} ALL=(ALL) NOPASSWD: ALL" > /etc/sudoers.d/${USERNAME} \
 && chmod 0440 /etc/sudoers.d/${USERNAME}

USER claude
WORKDIR /home/claude

ENV PATH=/home/claude/.local/bin:$PATH

RUN curl -fsSL https://claude.ai/install.sh | bash

CMD ["bash"]
