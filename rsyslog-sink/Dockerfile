FROM ubuntu:14.04
MAINTAINER Kevin Elledge <kevin.elledge@gmail.com>

ENV DEBIAN_FRONTEND noninteractive

RUN apt-get update && apt-get -y -q install software-properties-common python-software-properties
RUN add-apt-repository ppa:adiscon/v8-stable
RUN apt-get update && apt-get -y -q install rsyslog

RUN rm -f /etc/rsyslog.d/*
ADD files/rsyslog.conf /etc/
ADD files/rsyslog.d/* /etc/rsyslog.d/

EXPOSE 514/tcp 514/udp

ENTRYPOINT /usr/sbin/rsyslogd -nd
