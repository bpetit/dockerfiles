FROM bpetit/base:latest
MAINTAINER Benoit Petit <bpetit@learneth.fr>
# install nsd
RUN apt-get install nsd dnstop bind9utils -y
RUN wget https://gist.githubusercontent.com/bpetit/b385173238e402ed0a9f/raw/2671db590cb9181d908dfcefabf05ff244f795c8/nsd.conf -O /etc/nsd/nsd.conf.d/mysample.conf
RUN echo 'include: "/etc/nsd/nsd.conf.d/*.conf"' > /etc/nsd/nsd.conf
RUN mkdir /etc/nsd/zones
RUN wget https://gist.githubusercontent.com/bpetit/b385173238e402ed0a9f/raw/34f32068260f79ce2aa466f1a35adaa94b5b7413/nsd-zone-myzone.sample -O /etc/nsd/zones/myzone.sample
RUN wget https://gist.githubusercontent.com/bpetit/b385173238e402ed0a9f/raw/247f475e6c424e595ddfbdebe6ef2142664be992/nsd-reverse-myzone.sample -O /etc/nsd/zones/ip4.myzone.sample.reverse
EXPOSE 53
