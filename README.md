# openpose_jupyter_docker
Dockerfile repository for a docker image which opens a jupyter notebook server, configured to preprocess image/video data using OpenPose.

Based on https://hub.docker.com/r/cwaffles/openpose

Steps to use:
1. Install a version of the NVIDIA graphics driver compatible with CUDA 10.0+
2. Install nvidia-docker

INSTRUCTIONS FOR CREATING THE IMAGE+CONTAINER

#command used to build the image from the dockerfile
docker build -f openpose_preprocess.Docker -t openpose_preprocess:latest .

#command used to create the container from the image
#replace [PORT] and [PATH] before running
nvidia-docker run -it -p [PORT]:8888 -v [PATH]:/external --name mpcr_openpose --shm-size 16G openpose_preprocess


GENERAL DOCKER COMMANDS:

#view the docker images on your computer
docker image ls

#view the currently running containers
docker container ls

#view all containers, even if they are stopped
docker container ls -a
