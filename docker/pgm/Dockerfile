FROM ubuntu:22.04
LABEL maintainer="altriabot@gmail.com"
ENV PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin \
    SHELL=/bin/bash \
    PS1="\u@\h:\w \$ " \
    DEBIAN_FRONTEND=noninteractive

WORKDIR /pagermaid_pyro

RUN \
    apt-get update \
    && apt-get upgrade -y \
    && apt-get install --no-install-recommends -y \
        init \
        nano \
        python3-pip \
        git \
        wget \
        curl \
        neofetch \
        speedtest-cli \
        redis-server \
        tzdata \
    && cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime \
    && echo "Asia/Shanghai" > /etc/timezone \
    ## 克隆仓库
    && git clone -b master https://github.com/TeamPGM/PagerMaid-Pyro.git /pagermaid_pyro \
    && git config --global pull.ff only \
    && cd /pagermaid_pyro \
    ## pip install
    && pip install -r requirements.txt \
    && apt-get clean -y \
    && rm -rf \
        ## 删除apt和pip的安装缓存
        /tmp/* \
        /var/lib/apt/lists/* \
        /var/tmp/* \
        ~/.cache

COPY config.yml  /pagermaid_pyro

ENTRYPOINT ["init"]
