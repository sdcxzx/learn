FROM  python:3.9.11

ARG QL_MAINTAINER="whyour"
LABEL maintainer="${QL_MAINTAINER}"
ARG QL_URL=https://github.com/xiaoyaoke-zx/qinglong.git
ARG QL_BRANCH=master

ENV PNPM_HOME=/root/.local/share/pnpm \
    PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/root/.local/share/pnpm:/root/.local/share/pnpm/global/5/node_modules:$PNPM_HOME \
    SHELL=/bin/bash \
    PS1="\u@\h:\w \$ " \
    QL_DIR=/ql \
    QL_BRANCH=${QL_BRANCH}

COPY instantclient_11_2/ /opt/oracle/instantclient_11_2 


ENV ORACLE_HOME /opt/oracle/instantclient_11_2 \ 
    PATH $ORACLE_HOME:$PATH \
    TNS_ADMIN $ORACLE_HOME/network/admin \
    LD_LIBRARY_PATH $ORACLE_HOME:$LD_LIBRARY_PATH \
    NLS_LANG 'SIMPLIFIEDCHINESE_CHINA.AL32UTF8'

WORKDIR ${QL_DIR}

COPY  qinglong/ ${QL_DIR}

RUN curl -fsSL https://deb.nodesource.com/setup_18.x | bash -\
    && apt-get clean -y  \
    && apt-get update -y  \
    && apt-get upgrade -y \
    && apt-get  install -y bash \
    coreutils \
    moreutils \
    git \
    curl \
    wget \
    tzdata \
    perl \
    openssl \
    nginx \
    jq \
    openssh-server \
    nodejs \
    cron \
    vim \
    whereis \
    libaio* \
    && cron start \
    && ln -sf /usr/sbin/cron /usr/sbin/crond \
    && ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime \
    && echo "Asia/Shanghai" > /etc/timezone \
    && git config --global user.email "qinglong@@users.noreply.github.com" \
    && git config --global user.name "qinglong" \
    && npm install -g pnpm \
    && pnpm add -g pm2 ts-node typescript tslib cookie@^0.4.1 \
    && cd ${QL_DIR} \
    && cp -f .env.example .env \
    && chmod 777 ${QL_DIR}/shell/*.sh \
    && chmod 777 ${QL_DIR}/docker/*.sh \
    && pnpm install --prod \
    && rm -rf /root/.pnpm-store \
    && rm -rf /root/.local/share/pnpm/store \
    && rm -rf /root/.cache \
    && rm -rf /root/.npm \
    && git clone -b ${QL_BRANCH} https://github.com/${QL_MAINTAINER}/qinglong-static.git /static \
    && mkdir -p ${QL_DIR}/static \
    && cp -rf /static/* ${QL_DIR}/static \
    && rm -rf /static


ENTRYPOINT ["./docker/docker-entrypoint.sh"]