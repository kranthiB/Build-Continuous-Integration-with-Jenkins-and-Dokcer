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
        
        2.	Double-Click the “Docker Quick start Terminal” shortcut icon, execute the command “pwd “will come to know which       directory mapped on windows.
        
        3.	Create a directory for workspace by executing the command “mkdir docker-workspace”
        
        4.	All the successive steps mentioned will be followed by considering the above mentioned directory as base directory. 
            (For instance, here base directory - /c/users/Administrator/docker-workspace) 



  
