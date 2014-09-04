FROM debian:testing
MAINTAINER Benoit Petit <bpetit@learneth.fr>
# set the machine up to date
RUN apt-get update && apt-get upgrade -y
# install useful tools
RUN apt-get install -y vim mtr-tiny htop wget curl zsh openssh-server apt-utils dnsutils python-pip
# set zsh as the default shell
RUN chsh -s /bin/zsh root
