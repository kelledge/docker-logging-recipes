FROM ubuntu:14.04
MAINTAINER Kevin Elledge <kevin.elledge@gmail.com>

RUN apt-get -yq update && apt-get -yq install socat
EXPOSE 514/udp
ENTRYPOINT socat udp4-listen:514,reuseaddr,fork,ignoreeof stdout,ignoreeof
