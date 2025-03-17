# Overview
A repository to install cytometry R dependencies either locally or via a Docker container.  
  
This repository allows to manage and update R dependencies to develop locally and within a
Docker container simultaneously. For example, to move analyses to a HPC or share it with others via Docker image.

# Explanation
This Docker image is meant to create one container per project because dependencies are managed globally in the container. 
  
Use case 1:
Ideally, you clone this repository and use it as a project folder. You can either use the Docker container 
to develop, or install dependencies locally (see below), or do both. If used as intended, updating dependencies
in Docker will update Renv files locally and vice versa, keeping your dependency management in synchronised.  
If not cloning the repository, it's recommended have the renv.lock file in your analysis folder,
which is by default the case if running the Docker container without a volume mount to the Docker analysis folder.  
Only like this, dependencies can be updated locally and in Docker.
  
Use case 2:
This Docker image can also be used as standalone to run single-cell libraries in the Docker container without mounting any volumes 
or managing any dependencies. All R libraries are available in the Docker container.

# Building Docker image
## Automatic build
This repository automatically builds a docker image based on Dockerfile upon pushing changes.  
Docker image can be access through Dockerhub: https://hub.docker.com/repository/docker/maltebo/cytometry. 

## Manual build
Clone repository and build docker image: "docker build -t cytometry ."  

# Create container
"docker run -v /PATH_TO_PROJECT:/analysis --name PROJECT_NAME cytometry". 
Don't want R environment? Just comment out renv installation from Dockerfile.  

# Dependencies
You can use the Renv dependencies also without the Docker container. Clone repository and install 
all dependencies using: renv::restore()  
  
Even with Docker, it's recommended to keep a renv.lock file in your analysis folder, so you can update Docker and local dependencies at the same time.  
If you mount a volume to the docker analysis folder, the original renv.lock will be overwritten.

## R
R dependencies are managed using Renv. The Docker image installs renv dependencies globally to /etc/R/renv/library. 
These libraries are available in R. To install new packages, use "renv::install("PACKAGE")". 
If a folder is mounted that contains a renv environment, the local renv environment will automatically be used. 
It's possible that renv::restore() has to be rerun to make all packages available for the local environment.
