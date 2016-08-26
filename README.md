# Build-Continuous-Integration-with-Jenkins-and-Dokcer

## Prerequisite
    •	Install Ubuntu and Docker  - https://docs.docker.com/engine/installation/linux/ (OR)
  
    •	Install Docker Toolbox in windows - https://docs.docker.com/docker-for-windows/
  
## Jenkins + DInD (Docker – In – Docker)

    •	This includes Docker installation inside of Jenkins.

    •	This maintains the created containers as children.

    •	This would be preferable if want to manage a complete clean Docker environment inside Jenkins.
    
## Jenkins + DOOD (Docker – Outside – Of – Docker):

    •	This uses underlying host’s Docker installation.
    
    •	This maintains the created containers as siblings.
    
    Advantages:
    
        •	Enables sharing of images with host OS
        
              o	Eliminates storing images multiple times
              
              o	Makes it possible for Jenkins to automate local image creation
              
        •	Eliminates a virtualization layer (lxc)
        
        •	Any settings in the host’s Docker daemon will apply to Jenkins container as well
        
        •	Easier to set up, just need to map the host’s Docker executable and daemon socket on to the container
        
        •	Host and Jenkins container will use the same version of Docker, always.
        
        •	No privileged mode needed
        
        •	Permits the jenkins user to run docker without the sudo prefix.
        
        •	Allows greater flexibility at runtime.
        
        •	Ability to reuse the image cache from the host.
        
Note: In general, for testing and production environment, DOOD is chosen instead of DinD    

## Docker Toolbox Overview:

        1.	Once Docker Toolbox installed in windows , will be seen two shortcuts on desktop as below

![1](https://cloud.githubusercontent.com/assets/20100300/17997903/145950c6-6b37-11e6-85ea-387c4feadc5b.JPG)

        2.	Double-Click the “Docker Quick start Terminal” shortcut icon, execute the command 
        “pwd “will come to know which directory mapped on windows.

![2](https://cloud.githubusercontent.com/assets/20100300/17997906/19905a80-6b37-11e6-92fe-11b13c0a9ad0.JPG)

        3.	Create a directory for workspace by executing the command “mkdir docker-workspace”

![3](https://cloud.githubusercontent.com/assets/20100300/17997907/19beeec2-6b37-11e6-8c1b-2eea779bf4e9.JPG)

        4.	All the successive steps mentioned will be followed by considering the above mentioned directory 
        as base directory. (For instance, here base directory - /c/users/Administrator/docker-workspace) 
        
## Install Jenkins Master (with DOOD):

        1.	Create a directory “Jenkins” under “docker-workspace” directory by executing the command “mkdir Jenkins”
        
        2.	Create a directory “Master” under “Jenkins” directory by executing the command “mkdir Master”
        
        3.	Create a file with name “Dockerfile” under “Master” directory and fill with the below content

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
                    RUN curl -L "https://cli.run.pivotal.io/stable?release=linux64-binary&source=github" | tar -zx
                    RUN /cf install-plugin https://static-ice.ng.bluemix.net/ibm-containers-linux_x86 -f
                    COPY supervisord.conf /etc/supervisor/conf.d/supervisord.conf
                    CMD /usr/bin/supervisord -c /etc/supervisor/conf.d/supervisord.conf
                    
        4.	Create a file with name “supervisord.conf”  under “Master” directory , fill with the below content
        
                    [supervisord]
                    user=root
                    nodaemon=true

                    [program:chown]
                    priority=20
                    command=chown -R root:root /var/jenkins_home

                    [program:root]
                    user=root
                    autostart=true
                    autorestart=true
                    command=/usr/local/bin/jenkins.sh
                    redirect_stderr=true
                    stdout_logfile=/var/log/jenkins/%(program_name)s.log
                    stdout_logfile_maxbytes=10MB
                    stdout_logfile_backups=10
                    environment = JENKINS_HOME="/var/jenkins_home",HOME="/var/jenkins_home",USER="root"
        
        5.	Create a file with name “plugins.txt” under “Master” directory , fill with the below content
        
                    structs:latest
                    workflow-step-api:latest
                    workflow-scm-step:latest
                    scm-api:latest
                    scm-sync-configuration:latest
                    credentials:latest
                    mailer:latest
                    junit:latest
                    subversion:latest
                    script-security:latest
                    matrix-project:latest
                    ssh-credentials:latest
                    git-client:latest
                    git:latest
                    greenballs:latest
                    ssh-slaves:latest
                    token-macro:latest
                    durable-task:latest
                    docker-plugin:latest
        
        6.	Create a file with name “docker-compose.yml” under “Master” directory, fill with the below content
        
                    myjenkins:
                        image: myjenkins_cloudfoundary_dood 
                        volumes:
                            - /var/run/docker.sock:/var/run/docker.sock
                            - ./jenkins_home:/var/jenkins_home
                        ports:
                            - "8080:8080"
                            - "50000:50000"
        7.	In command prompt, go to the directory where the above four files exists, execute the below commands
        to start Jenkins with DOOD.
        
                a.	docker build –t myjenkins_cloudfoundary_dood .  (this will take few minutes time to build 
                the image – only one time should execute)
                
                b.	In order to confirm whether the image got created, execute the command -  “ docker images myjenkins_cloudfoundary_dood”
                
                c.	docker-compose up
                
                d.	In order to confirm whether the Jenkins process created or not , execute the command – “docker ps –a” which should be in “Up” status
 

        



  
