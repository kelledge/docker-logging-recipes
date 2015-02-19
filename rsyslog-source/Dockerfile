FROM ubuntu:14.04
MAINTAINER Kevin Elledge <kevin.elledge@gmail.com>

# Tell dpkg and debconf to be quiet
ENV DEBIAN_FRONTEND noninteractive

# Fetch the needed packages.
# The RSyslog PPA is added because Ubuntu (and many others) drag their feet on 
# including more recent rsyslog releases.
RUN apt-get update && apt-get -yq install software-properties-common python-software-properties
RUN add-apt-repository ppa:adiscon/v8-stable
RUN apt-get update && apt-get -yq install rsyslog cron logrotate supervisor

# Cron is used to call logrotate. It also calls many other things ... 
# Clean out any unneeded cron tasks, as well as 
#
# Be cautious when installing additional packages. Many add configuration fragments
# that will complicate and burden logging your application with extra noise and scripts.
RUN find /etc/cron.* -type f -not -name logrotate -not -name 0anacron | xargs rm
RUN rm -f /etc/rsyslog.d/* && rm -f /etc/logrotate.d/*

# Copy over the shipped configuration files. They're far heavier than needed -- log and 
# rotate exactly what is needed.
ADD files/rsyslog.conf /etc/
ADD files/logrotate.conf /etc/
ADD bin/log-generator /usr/bin/

# Copy in configuration fragments.
ADD files/rsyslog.d/* /etc/rsyslog.d/
ADD files/logrotate.d/* /etc/logrotate.d/
ADD files/supervisor.d/* /etc/supervisor/conf.d/
ADD files/cron.d/* /etc/cron.d/
RUN chmod 644 /etc/cron.d/* && chmod -R 644 /etc/logrotate.*

# One example requires a directory to exist before running
RUN mkdir -p /app/log/

# 
ENTRYPOINT /usr/bin/supervisord -c /etc/supervisor/supervisord.conf
