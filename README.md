# openpose_jupyter_docker
Dockerfile repository for a docker image which opens a jupyter notebook server, configured to preprocess image/video data using OpenPose.

Based on https://hub.docker.com/r/cwaffles/openpose

Steps to use:
1. Install a version of the NVIDIA graphics driver compatible with CUDA 10.0+
2. Install nvidia-docker


## INSTRUCTIONS FOR CREATING THE IMAGE+CONTAINER

#command used to build the image from the dockerfile

docker build -f openpose_preprocess.Docker -t openpose_preprocess:latest .

#command used to create the container from the image

#replace [PORT] and [PATH] before running

nvidia-docker run -it -p [PORT]:8888 -v [PATH]:/external --name mpcr_openpose --shm-size 16G openpose_preprocess


## GENERAL DOCKER COMMANDS:


#view the docker images on your computer

docker image ls


#view the currently running containers

docker container ls


#view all containers, even if they are stopped

docker container ls -a

## SETUP NVIDIA DOCKER ON UBUNTU 16:

#install nvidia drivers, version 410 (change numbers for another version)



sudo add-apt-repository ppa:graphics-drivers/ppa

sudo apt-get update

sudo apt-get install nvidia-410



#install docker



sudo apt-get remove docker docker-engine docker.io containerd runc

sudo apt-get update

sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg-agent \
    software-properties-common
    
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
   
sudo apt-get update

sudo apt-get install docker-ce docker-ce-cli containerd.io



#give non-root access for docker



sudo groupadd docker

sudo gpasswd -a rachelwong docker

newgrp docker



#move docker image folder to home folder (or place with more storage)
#in /lib/systemd/system/docker.service , make the following change



sudo nano /lib/systemd/system/docker.service



FROM:

ExecStart=/usr/bin/docker daemon -H fd://

TO:

ExecStart=/usr/bin/docker daemon -g /home/rachelwong/docker_images -H fd://



#then restart docker



sudo systemctl stop docker

sudo systemctl daemon-reload

mkdir /home/rachelwong/docker_images

sudo rsync -aqxP /var/lib/docker/ /home/rachelwong/docker_images

sudo systemctl start docker



#install nvidia-docker



curl -s -L https://nvidia.github.io/nvidia-docker/gpgkey | \
  sudo apt-key add -
distribution=$(. /etc/os-release;echo $ID$VERSION_ID)

curl -s -L https://nvidia.github.io/nvidia-docker/$distribution/nvidia-docker.list | \
  sudo tee /etc/apt/sources.list.d/nvidia-docker.list
  
sudo apt-get update



sudo apt-get install -y nvidia-docker2

sudo pkill -SIGHUP dockerd

