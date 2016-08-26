FROM java:8-jdk
MAINTAINER Kranthi Kumar Bitra <kranthi.b76@gmail.com>

ENV HOME /var/jenkins_home
RUN useradd -c "Jenkins user" -d $HOME -m jenkins
RUN usermod -G users jenkins

USER root
RUN apt-get update \
      && apt-get install -y sudo supervisor \
      && rm -rf /var/lib/apt/lists/*

RUN curl -sSL https://get.docker.com/ | sh

RUN usermod -aG docker jenkins


USER root
RUN mkdir -p /var/log/supervisor
RUN mkdir -p /var/log/jenkins
COPY supervisord.conf /etc/supervisor/conf.d/supervisord.conf
CMD /usr/bin/supervisord -c /etc/supervisor/conf.d/supervisord.conf
RUN curl -L "https://cli.run.pivotal.io/stable?release=linux64-binary&source=github" | tar -zx
RUN /cf install-plugin https://static-ice.ng.bluemix.net/ibm-containers-linux_x86 -f

RUN curl -A "Mozilla/5.0" --create-dirs -sSLo /usr/share/jenkins/slave.jar http://repo.jenkins-ci.org/public/org/jenkins-ci/main/remoting/2.53/remoting-2.53.jar  && chmod 755 /usr/share/jenkins  && chmod 644 /usr/share/jenkins/slave.jar

COPY jenkins-slave /usr/local/bin/jenkins-slave

WORKDIR /var/jenkins_home
USER jenkins
