FROM ubuntu:14.04
MAINTAINER Kevin Elledge <kevin.elledge@gmail.com>

ENV DEBIAN_FRONTEND noninteractive

RUN \
apt-get update && \
apt-get -yq install \
  software-properties-common \
  python-software-properties \
  reprepro \
  git \
  git-buildpackage

RUN add-apt-repository -s ppa:adiscon/v8-stable && apt-get update && apt-get -yq build-dep rsyslog
RUN git clone https://github.com/kelledge/rsyslog.git /tmp/rsyslog

WORKDIR /tmp/rsyslog
RUN git-buildpackage --git-export-dir=/tmp/build

RUN mkdir -p /reprepro/conf
ADD files/* /reprepro/conf/
WORKDIR /reprepro

RUN reprepro update rsyslog && reprepro --component main includedeb rsyslog /tmp/build/*.deb && reprepro export

EXPOSE 80
ENTRYPOINT python -m SimpleHTTPServer 80
