# syntax=docker/dockerfile:1.2
ARG SRCVERSION=18
# update based on https://hub.docker.com/r/dataeditors/stata17/tags
ARG SRCTAG=2024-04-30
ARG SRCHUBID=dataeditors

FROM ${SRCHUBID}/stata${SRCVERSION}:${SRCTAG}

# The next parts need to be run as root ========================================== ROOT
USER root

# Install packages needed for aea-scripts
RUN apt-get update \
    && DEBIAN_FRONTEND=noninteractive apt-get install -y \
         curl \
         pandoc \
         wkhtmltopdf \
         python3-pip \
         rsync \
         openssh-server openssh-client \
         python3.11-venv \
         imagemagick \
         sudo \
    && rm -rf /var/lib/apt/lists/* \
    && adduser statauser sudo \ 
    && sed -i 's+domain="coder" rights="none"+domain="coder" rights="read | write"+' /etc/ImageMagick-6/policy.xml \
    && echo '%sudo ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers

# Install Latex packages

RUN tlmgr install eurosym threeparttable adjustbox caption float supertabular

# install the gh binaries
RUN \ 
   curl -fsSL https://cli.github.com/packages/githubcli-archive-keyring.gpg |  dd of=/usr/share/keyrings/githubcli-archive-keyring.gpg \
   && echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/githubcli-archive-keyring.gpg] https://cli.github.com/packages stable main" > /etc/apt/sources.list.d/github-cli.list 

# install the AEA scripts into /usr/local/sbin
RUN git clone https://github.com/AEADataEditor/editor-scripts.git /usr/local/bin 

# Install rclone to get at Box and others

RUN curl https://rclone.org/install.sh | bash

# get latest python requirements
RUN curl https://raw.githubusercontent.com/AEADataEditor/replication-template-development/development/requirements.txt > /requirements.txt \
   && pip install -r /requirements.txt 

# Now switch back to the regular user (statauser) ================================ STATAUSER
USER statauser

# install any packages into the home directory as the user,

WORKDIR /home/statauser
COPY setup.do setup.do


# Configure git to store credentials
# NOTE: do not use this in general, as it stores your credentials!
# NOTE: Create a personal access token specifically for use on Codespaces, 
#       so you can invalidate it if needed!

RUN git config --global credential.helper store

#  then delete the license again - this is not needed on Codespaces but MUST <<<<<<<<<<<<<<<<
#  be done if you intend to publish a Docker image like this one!!
#USER root
#RUN rm /usr/local/stata/stata.lic

# Setup for standard operation
SHELL [ "/bin/bash", "-l", "-c" ]
USER statauser
#WORKDIR /workspaces
#ENTRYPOINT ["stata-mp"]
