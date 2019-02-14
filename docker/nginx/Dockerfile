from  ubuntu:18.04
#copy sources.list.trusty  /etc/apt/sources.list
ENV DEBIAN_FRONTEND noninteractive
run   apt-get update && \
      apt-get install -y --no-install-recommends wget unzip make git dpkg-dev  build-essential software-properties-common openssl
#run echo "deb http://ppa.launchpad.net/nginx/stable/ubuntu/ xenial main" >/etc/apt/sources.list.d/nginx-stable-xenial.list 
#run add-apt-repository ppa:nginx/stable  
run apt-get update
#run apt-get build-dep nginx
MAINTAINER amit cahanovich "amitca71@gmail.com"
RUN wget https://nginx.org/download/nginx-1.15.5.tar.gz && tar zxvf nginx-1.15.5.tar.gz
# PCRE version 4.4 - 8.40
run wget https://ftp.pcre.org/pub/pcre/pcre-8.41.tar.gz && tar xzvf pcre-8.41.tar.gz
workdir /pcre-8.41
run ./configure && make && make install
# zlib version 1.1.3 - 1.2.11
run wget http://www.zlib.net/zlib-1.2.11.tar.gz && tar xzvf zlib-1.2.11.tar.gz
workdir zlib-1.2.11
run ./configure && make && make install
# OpenSSL version 1.0.2 - 1.1.0
workdir /
RUN git clone https://github.com/yaoweibin/nginx_ajp_module.git
RUN git clone https://github.com/AirisX/nginx_cookie_flag_module.git
workdir /nginx-1.15.5
run ./configure --add-module=/nginx_ajp_module --add-module=/nginx_cookie_flag_module --with-stream --with-http_realip_module
run make
run make install
run /usr/local/nginx/sbin/nginx -t
ARG CACHEBUST=1
ADD nginx.conf /usr/local/nginx/conf/nginx.conf
workdir /usr/local/nginx
run cp /usr/local/nginx/sbin/nginx /usr/sbin/nginx
CMD /usr/sbin/nginx -c /usr/local/nginx/conf/nginx.conf -g 'daemon off;'
