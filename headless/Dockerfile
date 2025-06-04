# Main instance, building
# upon a basic python 2.7 image
FROM python:2.7

# LABEL about the custom image
LABEL maintainer="BlueGreen Labs"
LABEL version="1.0"

# Create data directory to use
# in mounting volumes
RUN mkdir /data/

# Disable Prompt During Packages Installation
ARG DEBIAN_FRONTEND=noninteractive

# Update Ubuntu Software repository
RUN apt update

# install additional components for
# downloading and compiling code
# later on. Not sure what is already
# included in the base image, but this
# covers stuff regardless
RUN apt-get -q update && \
    apt-get install -q -y --no-install-recommends \
      unzip \
      wget \
      gcc \
      git \
      make \
      gfortran \
      curl && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# Download the MCR from MathWorks site an install with -mode silent
RUN mkdir /mcr-install && \
    mkdir /opt/mcr && \
    cd /mcr-install && \
    wget --no-check-certificate -q https://ssd.mathworks.com/supportfiles/downloads/R2018a/deployment_files/R2018a/installers/glnxa64/MCR_R2018a_glnxa64_installer.zip && \
    unzip -q MCR_R2018a_glnxa64_installer.zip && \
    rm -f MCR_R2018a_glnxa64_installer.zip && \
    ./install -destinationFolder /opt/mcr -agreeToLicense yes -mode silent && \
    cd / && \
    rm -rf mcr-install

# Installing oneflux from github
RUN export GIT_SSL_NO_VERIFY=1 && \
    git clone -b v0.6.0-rc-pinned-dependencies --single-branch https://github.com/owain-rutherford/ONEFlux.git && \
    cd ONEFlux && \
    make 

# headless install
# move into the ONEFlux directory
WORKDIR /ONEFlux

# set entrypoint to run the python script
ENTRYPOINT [ "python", "./runoneflux.py"]

