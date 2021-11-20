# for format details, see https://docs.docker.com/engine/reference/builder/

# select image
ARG PYTHON_VERSION="latest"
FROM mcr.microsoft.com/vscode/devcontainers/python:${PYTHON_VERSION}
LABEL author="dariocurr@gmail.com" maintainer="dariocurr@gmail.com" version="0.0.1"

# update apt
RUN apt -qq update \
	&& apt -qq upgrade -y \
	&& export DEBIAN_FRONTEND=noninteractive

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
RUN pip --no-cache-dir -q install --upgrade pip \
	&& pip --no-cache-dir -q install -r /tmp/pip-tmp/requirements.txt -r /tmp/pip-tmp/requirements-dev.txt \
	&& rm -rf /tmp/pip-tmp

# add zsh configuration and replace oh-my-zsh with antigen
USER vscode
WORKDIR /home/vscode/
RUN rm -r .oh-my-zsh \
	&& git clone -q https://github.com/zsh-users/antigen.git .antigen
COPY .devcontainer/.zshrc .