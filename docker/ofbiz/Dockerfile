from  ubuntu:18.04
ENV DEBIAN_FRONTEND noninteractive
run   apt-get update && \
      apt-get install -y --no-install-recommends  wget  unzip default-jre default-jdk   net-tools && \
      apt-get install -y subversion
MAINTAINER ami cohen "amitca71@gmail.com"
RUN svn co http://svn.apache.org/repos/asf/ofbiz/ofbiz-framework/branches/release18.12 ofbiz.18.12
workdir ofbiz.18.12
RUN ./gradlew loadall
cmd  ./gradlew ofbiz
