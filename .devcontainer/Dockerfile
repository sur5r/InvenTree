# See here for image contents: https://github.com/microsoft/vscode-dev-containers/tree/v0.241.1/containers/python-3/.devcontainer/base.Dockerfile

# [Choice] Python version (use -bullseye variants on local arm64/Apple Silicon): 3, 3.10, 3.9, 3.8, 3.7, 3.6, 3-bullseye, 3.10-bullseye, 3.9-bullseye, 3.8-bullseye, 3.7-bullseye, 3.6-bullseye, 3-buster, 3.10-buster, 3.9-buster, 3.8-buster, 3.7-buster, 3.6-buster
ARG VARIANT="3.10-bullseye"
FROM mcr.microsoft.com/vscode/devcontainers/python:0-${VARIANT}

# [Choice] Node.js version: none, lts/*, 16, 14, 12, 10
ARG NODE_VERSION="none"
RUN if [ "${NODE_VERSION}" != "none" ]; then su vscode -c "umask 0002 && . /usr/local/share/nvm/nvm.sh && nvm install ${NODE_VERSION} 2>&1"; fi

# [Optional] If your pip requirements rarely change, uncomment this section to add them to the image.
# COPY requirements.txt /tmp/pip-tmp/
# RUN pip3 --disable-pip-version-check --no-cache-dir install -r /tmp/pip-tmp/requirements.txt \
#    && rm -rf /tmp/pip-tmp

# [Optional] Uncomment this section to install additional OS packages.
RUN apt-get update && export DEBIAN_FRONTEND=noninteractive && \
    apt-get -y install --no-install-recommends \
    git gcc g++ gettext gnupg2 libffi-dev \
    # Weasyprint requirements : https://doc.courtbouillon.org/weasyprint/stable/first_steps.html#debian-11
    poppler-utils libpango-1.0-0 libpangoft2-1.0-0 \
    # Image format support
    libjpeg-dev webp \
    # SQLite support
    sqlite3 \
    # PostgreSQL support
    libpq-dev \
    # MySQL / MariaDB support
    default-libmysqlclient-dev mariadb-client && \
    apt-get autoclean && apt-get autoremove

# [Optional] Uncomment this line to install global node packages.
# RUN su vscode -c "source /usr/local/share/nvm/nvm.sh && npm install -g <your-package-here>" 2>&1

# Update pip
RUN pip install --upgrade pip

# Install required base-level python packages
COPY ./docker/requirements.txt base_requirements.txt
RUN pip install --disable-pip-version-check -U -r base_requirements.txt

# preserve command history between container starts
# Ref: https://code.visualstudio.com/remote/advancedcontainers/persist-bash-history
# Folder will be created in 'postCreateCommand' in devcontainer.json as it's not preserved due to the bind mount
RUN echo "export PROMPT_COMMAND='history -a' && export HISTFILE=/workspaces/InvenTree/dev/commandhistory/.bash_history" >> "/home/vscode/.bashrc"

WORKDIR /workspaces/InvenTree
