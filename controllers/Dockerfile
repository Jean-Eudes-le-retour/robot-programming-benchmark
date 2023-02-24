FROM leoduggan/webots.cloud:develop as webots-downloader

FROM ubuntu:22.04

# Install Python 3.10
RUN apt-get update && apt-get install -y software-properties-common \
    && add-apt-repository ppa:deadsnakes/ppa \
    && apt-get update && apt-get install -y python3.10 && rm -rf /var/lib/apt/lists/*

COPY --from=webots-downloader /usr/local/webots/lib /usr/local/webots/lib
COPY --from=webots-downloader /usr/local/webots/webots-controller /usr/local/webots/webots-controller

ENV WEBOTS_HOME=/usr/local/webots

# Environment variables needed for Webots
# https://cyberbotics.com/doc/guide/running-extern-robot-controllers?version=develop&tab-os=linux
# ENV PYTHONPATH=${WEBOTS_HOME}/lib/controller/python
# ENV PYTHONIOENCODING=UTF-8
# ENV LD_LIBRARY_PATH=${WEBOTS_HOME}/lib/controller:${LD_LIBRARY_PATH}
#ARG WEBOTS_CONTROLLER_URL
#ENV WEBOTS_CONTROLLER_URL=${WEBOTS_CONTROLLER_URL}

# Copies all the files of the controllers folder into the docker container
RUN mkdir -p /usr/local/webots-project/controllers
COPY . /usr/local/webots-project/controllers

# Entrypoint command to launch default Python controller script
# (In shell form to allow variable expansion)
WORKDIR /usr/local/webots-project/controllers/participant
CMD ["/usr/local/webots/webots-controller", "--robot-name=Thymio%20II", "participant.py"]
