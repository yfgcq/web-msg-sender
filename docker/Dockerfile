FROM ubuntu:18.04
MAINTAINER yfgcq <cqguyunfeng@163.com>

# ①
RUN sed -i "s/archive\.ubuntu\.com/mirrors.aliyun.com/g" /etc/apt/sources.list
RUN apt update
RUN apt install -y gnupg
RUN apt-key adv --keyserver keyserver.ubuntu.com --recv-keys ABF5BD827BD9BF62
RUN apt install -y software-properties-common dirmngr apt-transport-https lsb-release ca-certificates make wget
RUN apt install -y language-pack-en
RUN locale-gen en_US.UTF-8
RUN export LANG=en_US.UTF-8
RUN LC_ALL=C.UTF-8 add-apt-repository ppa:ondrej/php

# ②
RUN apt update;\
      ln -s /usr/lib/x86_64-linux-gnu/libssl.so /usr/lib;\
      DEBIAN_FRONTEND=noninteractive apt install -y --no-install-recommends apt-utils tzdata \
      git gcc php7.2-cli php-pear php7.2-dev php7.2-xml libevent-dev;\
      wget -c http://pecl.php.net/get/event-2.5.3.tgz -P /usr/local/src;\
      cd /usr/local/src;\
      tar -zxvf event-2.5.3.tgz && cd event-2.5.3;\
      /usr/bin/phpize7.2;\
      ./configure --with-php-config=/usr/bin/php-config --with-event-libevent-dir=/usr/local/libevent-2.1.8/ --enable-event-debug=no --enable-event-sockets=yes --with-event-libevent-dir=/usr --with-event-pthreads=no --with-event-extra=yes --with-event-openssl=yes --with-event-ns=yes --with-openssl-dir=no;\
      make && make install;\
      echo extension=event.so > /etc/php/7.2/cli/conf.d/event.ini;

## ③
RUN ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
RUN echo "Asia/Shanghai" > /etc/timezone
RUN dpkg-reconfigure -f noninteractive tzdata
#
## ④
## Install and run composer
RUN apt install -y wget curl vim telnetd net-tools inetutils-ping supervisor
RUN php -r "copy('https://install.phpcomposer.com/installer', 'composer-setup.php');"
RUN php composer-setup.php
RUN php -r "unlink('composer-setup.php');"

RUN git clone https://github.com/walkor/web-msg-sender.git;\
    mv composer.phar web-msg-sender;\
    cd web-msg-sender;\
    ./composer.phar update;\


#
## ⑤
## RUN apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*
#
## ⑥
#ENV HOME  "/www"
#RUN mkdir ${HOME}
#WORKDIR ${HOME}/
#
#CMD ["/bin/sh","-c", " while true;do echo hello docker;sleep 1;done"]
#
#EXPOSE 2320
#EXPOSE 2321
#EXPOSE 2323

