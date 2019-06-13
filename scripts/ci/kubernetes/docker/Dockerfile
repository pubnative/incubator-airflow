#  Licensed to the Apache Software Foundation (ASF) under one   *
#  or more contributor license agreements.  See the NOTICE file *
#  distributed with this work for additional information        *
#  regarding copyright ownership.  The ASF licenses this file   *
#  to you under the Apache License, Version 2.0 (the            *
#  "License"); you may not use this file except in compliance   *
#  with the License.  You may obtain a copy of the License at   *
#                                                               *
#    http://www.apache.org/licenses/LICENSE-2.0                 *
#                                                               *
#  Unless required by applicable law or agreed to in writing,   *
#  software distributed under the License is distributed on an  *
#  "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY       *
#  KIND, either express or implied.  See the License for the    *
#  specific language governing permissions and limitations      *
#  under the License.                                           *


FROM ubuntu:18.04

ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update \
  && apt-get install -y \
    curl \
    wget \
    git \
    gnupg2 \
    llvm \
    make \
    zip \
    unzip \
    build-essential \
    bind9utils \
    inetutils-telnet \
    libbz2-dev \
    libcurlpp-dev \
    libczmq-dev \
    libffi-dev \
    liblzma-dev \
    libncurses5-dev \
    libncursesw5-dev \
    libreadline-dev \
    libsqlite3-dev \
    libssl-dev \
    python-openssl \
    tk-dev \
    xz-utils \
    zlib1g-dev \
  && apt-get clean

ENV NODE_VERSION "12"
RUN ["/bin/bash", "-c", "set -o pipefail \
    && curl -sL https://deb.nodesource.com/setup_${NODE_VERSION}.x \
    | bash - \
    && apt-get install \
      -y --no-install-recommends \
      nodejs"]

ENV PYENV_INSTALLER_GIT_COMMIT=9aac07fad9eabe78ff63632e95bd33eda33b0a12
RUN ["/bin/bash", "-c", "set -o pipefail \
    && curl -sSL \
      https://raw.githubusercontent.com/pyenv/pyenv-installer/${PYENV_INSTALLER_GIT_COMMIT}/bin/pyenv-installer \
    | bash"]
ENV PYENV_ROOT /root/.pyenv
ENV CFLAGS='-O2'
ENV PATH $PYENV_ROOT/shims:$PYENV_ROOT/bin:$PATH

ENV PYTHON_VERSION "3.7.3"
RUN pyenv install ${PYTHON_VERSION} \
  && pyenv global ${PYTHON_VERSION}

RUN pip install -U \
  cryptography==2.7 \
  GitPython==2.1.11 \
  kubernetes==9.0.0 \
  setuptools==41.0.1 \
  psycopg2-binary==2.8.2

COPY . /tmp/airflow
RUN cd /tmp/airflow \
  && pip install . \ 
  && rm -rf www_rbac/node_modules

RUN mv /tmp/airflow/scripts/ci/kubernetes/docker/airflow-test-env-init.sh /tmp/airflow-test-env-init.sh
RUN mv /tmp/airflow/scripts/ci/kubernetes/docker/bootstrap.sh /bootstrap.sh
RUN chmod +x /bootstrap.sh

ENTRYPOINT ["/bootstrap.sh"]
