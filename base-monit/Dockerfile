FROM debian:testing
MAINTAINER Benoit Petit <bpetit@learneth.fr>
USER root
RUN apt-get update && apt-get upgrade -y
RUN apt-get install -y vim mtr-tiny htop wget curl zsh openssh-server apt-utils dnsutils makepasswd tcpdump python-software-properties less git-core
RUN chsh -s /bin/zsh root
ENV HOME /root
# set zshrc
ADD zshrc /root/.zshrc
RUN cp /root/.zshrc /etc/zsh/zshrc
# set vimrc
ADD vimrc /root/.vimrc
RUN cp /root/.vimrc /etc/vim/vimrc
# setup ssh first access
ADD sshd_config /etc/ssh/sshd_config
RUN makepasswd --minchars 10 --maxchars 30 > /root/default-passwd
RUN echo root:$(cat /root/default-passwd) | chpasswd
RUN apt-get install monit -y
RUN rm /etc/monit/monitrc
ADD monitrc /etc/monit/monitrc
RUN chmod 700 /etc/monit/monitrc
CMD monit reload && monit monitor all && monit start all && service monit start && monit && su
