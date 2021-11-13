# for format details, see https://docs.docker.com/engine/reference/builder/

# select image
ARG PYTHON_VERSION="latest"
FROM mcr.microsoft.com/vscode/devcontainers/python:${PYTHON_VERSION}
LABEL author="dariocurr@gmail.com" maintainer="dariocurr@gmail.com" version="0.0.1"

# update apt
RUN apt-get update \
	&& apt upgrade -y \
	&& export DEBIAN_FRONTEND=noninteractive

# add zsh configuration
COPY .devcontainer/.zshrc .

# allow the vscode user to pip install globally w/o sudo
ENV PIP_TARGET=/usr/local/pip-global
ENV PYTHONPATH=${PIP_TARGET}:${PYTHONPATH}
ENV PATH=${PIP_TARGET}/bin:${PATH}
RUN if ! cat /etc/group | grep -e "^pip-global:" > /dev/null 2>&1; then groupadd -r pip-global; fi \
	&& usermod -a -G pip-global vscode \
	&& umask 0002 && mkdir -p ${PIP_TARGET} \
	&& chown :pip-global ${PIP_TARGET} \
	&& ( [ ! -f "/etc/profile.d/00-restore-env.sh" ] || sed -i -e "s/export PATH=/export PATH=\/usr\/local\/pip-global:/" /etc/profile.d/00-restore-env.sh )

# update pip and install packages
COPY requirements*.txt /tmp/pip-tmp/
RUN pip install --upgrade pip \
	&& pip --no-cache-dir install -r /tmp/pip-tmp/requirements.txt -r /tmp/pip-tmp/requirements-dev.txt \
	&& rm -rf /tmp/pip-tmp