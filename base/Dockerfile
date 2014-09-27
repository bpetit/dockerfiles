FROM debian:testing
MAINTAINER Benoit Petit <bpetit@learneth.fr>
# are you root ?
USER root
# set the machine up to date
RUN apt-get update && apt-get upgrade -y
# install useful tools
RUN apt-get install -y vim mtr-tiny htop wget curl zsh openssh-server apt-utils dnsutils makepasswd tcpdump python-software-properties less
# set zsh as the default shell
RUN chsh -s /bin/zsh root
ENV HOME /root
# set zshrc
RUN wget https://gist.githubusercontent.com/bpetit/b385173238e402ed0a9f/raw/24a37687c308d2e134d1842da48aa3fe5b2bcb28/zshrc -O /root/.zshrc
RUN cp /root/.zshrc /etc/zsh/zshrc
# set vimrc
RUN wget https://gist.githubusercontent.com/bpetit/b385173238e402ed0a9f/raw/293f5a03740c736913b1518705c5f95dba440739/vimrc -O /root/.vimrc
RUN cp /root/.vimrc /etc/vim/vimrc
# setup ssh first access
RUN wget https://gist.githubusercontent.com/bpetit/b385173238e402ed0a9f/raw/0aee6e11ed902717dece39911f77ad4b7640d6fc/sshd_config_dummy -O /etc/ssh/sshd_config
RUN makepasswd --minchars 10 --maxchars 30 > /root/default-passwd
RUN echo root:$(cat /root/default-passwd) | chpasswd
RUN mkdir /var/run/sshd
# prepare supervisord to run services
RUN apt-get install supervisor -y
RUN wget https://gist.githubusercontent.com/bpetit/b385173238e402ed0a9f/raw/54296b8b254e84360293f484a4fcfc49058cc17d/supervisord.conf -O /etc/supervisor/conf.d/supervisord.conf
# add ssh to services to run
RUN echo '[program:sshd]' >> /etc/supervisor/conf.d/supervisord.conf
RUN echo 'command=/usr/sbin/sshd -D -f /etc/ssh/sshd_config' >> /etc/supervisor/conf.d/supervisord.conf
EXPOSE 22
RUN echo '#!/bin/bash' > /root/init-passwd.sh
RUN echo 'makepasswd --minchars 10 --maxchars 30 > /root/default-passwd && echo "random initial root passwd: $(cat /root/default-passwd)" && echo root:$(cat /root/default-passwd) | chpasswd' >> /root/init-passwd.sh
ONBUILD RUN bash /root/init-passwd.sh
CMD echo "initial root passwd: $(cat /root/default-passwd)" && /usr/bin/supervisord
