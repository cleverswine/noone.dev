FROM mcr.microsoft.com/vscode/devcontainers/base:debian-10

# Base
ENV DEBIAN_FRONTEND=noninteractive
RUN apt-get update && apt-get upgrade -y
RUN apt-get -y install --no-install-recommends curl vim git

# ARG USERNAME=vscode
# ARG USER_UID=1000
# ARG USER_GID=$USER_UID

# Create the user
# https://code.visualstudio.com/docs/remote/containers-advanced#_creating-a-nonroot-user
# RUN groupadd --gid $USER_GID $USERNAME \
#     && useradd --uid $USER_UID --gid $USER_GID -m $USERNAME \
#     && apt-get update \
#     && apt-get install -y sudo \
#     && echo $USERNAME ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/$USERNAME \
#     && chmod 0440 /etc/sudoers.d/$USERNAME

# Persist bash history between runs
# https://code.visualstudio.com/docs/remote/containers-advanced#_persist-bash-history-between-runs
# RUN SNIPPET="export PROMPT_COMMAND='history -a' && export HISTFILE=/commandhistory/.bash_history" \
#     && echo $SNIPPET >> "/root/.bashrc" \
#     && mkdir /commandhistory \
#     && touch /commandhistory/.bash_history \
#     && chown -R $USERNAME /commandhistory \
#     && echo $SNIPPET >> "/home/$USERNAME/.bashrc"

# Set the default user
# USER $USERNAME

# Hugo
# https://github.com/gohugoio/hugo/releases
RUN curl -O -L -C - https://github.com/gohugoio/hugo/releases/download/v0.165.0/hugo_extended_0.165.0_linux-amd64.deb \
    && dpkg -i hugo_extended_0.165.0_linux-amd64.deb \
    && rm hugo_extended_0.165.0_linux-amd64.deb
# Cleanup
RUN apt-get autoremove -y
ENV DEBIAN_FRONTEND=dialog

# Persist bash history between sessions
RUN SNIPPET="export PROMPT_COMMAND='history -a' && export HISTFILE=/commandhistory/.bash_history" \
    && echo $SNIPPET >> "/root/.bashrc" \
    && mkdir /commandhistory \
    && touch /commandhistory/.bash_history

# SSH Keys
# https://code.visualstudio.com/docs/remote/containers#_using-ssh-keys