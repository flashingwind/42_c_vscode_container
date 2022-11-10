FROM python:3.10-alpine

ENV LC_ALL=C.UTF-8
ENV LANG=C.UTF-8

LABEL "maintainer"="miida <miida@student.42tokyo.jp>"
LABEL "repository"="https://github.com/flashingwind/"

RUN apk update && \
    apk add alpine-sdk cmake clang lldb clang-extra-tools libressl-dev bash zsh vim nano \
    gcompat libstdc++ curl grep dropbear-scp dropbear-ssh

RUN python3 -m pip install --upgrade setuptools \
&& python3 -m pip install --upgrade pip \
&& python3 -m pip install --disable-pip-version-check --no-cache-dir norminette \
&& python3 -m pip install --disable-pip-version-check --no-cache-dir c-formatter-42 \
&& mkdir /code

RUN rm /usr/local/lib/python3.10/site-packages/c_formatter_42/data/clang-format-linux \
&& ln -s /usr/bin/clang-format /usr/local/lib/python3.10/site-packages/c_formatter_42/data/clang-format-linux

#RUN wget -O- https://aka.ms/install-vscode-server/setup.sh | sh

WORKDIR /code

#ENTRYPOINT ["code-server", "serve-local", "--accept-server-license-terms", "--bind-addr", "0.0.0.0:8443"]
