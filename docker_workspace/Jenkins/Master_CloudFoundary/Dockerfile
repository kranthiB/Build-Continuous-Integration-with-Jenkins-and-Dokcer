FROM jenkins:latest
MAINTAINER Kranthi Kumar Bitra <kranthi.b76@gmail.com>

USER root
RUN apt-get update \
      && apt-get install -y sudo supervisor \
      && rm -rf /var/lib/apt/lists/*

RUN curl -sSL https://get.docker.com/ | sh

RUN usermod -aG docker root

USER root
RUN mkdir -p /var/log/supervisor
RUN mkdir -p /var/log/jenkins
RUN mkdir -p /cloudfoundary
RUN cd /cloudfoundary
RUN curl -L "https://cli.run.pivotal.io/stable?release=linux64-binary&source=github" | tar -zx
RUN /cf install-plugin https://static-ice.ng.bluemix.net/ibm-containers-linux_x86 -f

COPY supervisord.conf /etc/supervisor/conf.d/supervisord.conf
CMD /usr/bin/supervisord -c /etc/supervisor/conf.d/supervisord.conf

