FROM bpetit/base:latest
MAINTAINER Benoit Petit <bpetit@learneth.fr>
# blog installation folder path is tunable
ENV BLOG_DIR /var/www/blog
# let's install the packages we need
RUN apt-get install python-pip git-core nginx -y
RUN pip install pelican Markdown typogrify
# create needed folders and the git repository
RUN mkdir -p $BLOG_DIR
RUN mkdir /var/www/cache
RUN useradd -b /srv -U -s /bin/bash -m git
RUN chown -R git:git /var/www/cache
RUN mkdir /srv/git/.ssh
RUN mkdir /srv/git/blog.git
RUN git init --bare /srv/git/blog.git
RUN echo "#!/bin/bash\nGIT_WORK_TREE=$BLOG_DIR/content git checkout -f\npelican -s $BLOG_DIR -o $BLOG_DIR/output $BLOG_DIR" >> /srv/git/blog.git/hooks/post-receive
RUN chmod +x /srv/git/blog.git/hooks/post-receive
RUN chown -R git:git /srv/git/
RUN usermod -a -G www-data git
# let's compute a defaul root password (change it ASAP)
RUN bash /root/init-passwd.sh
# this is a tiny script which intializes blog folder
RUN echo "#!/bin/bash\npelican-quickstart -p $BLOG_DIR\nchown -R git:git $BLOG_DIR\nchown -R www-data:www-data $BLOG_DIR/output\nchmod -R g+w $BLOG_DIR/output" >> /root/generate-blog.sh
# configure nginx to serve files from the output folder
RUN rm /etc/nginx/sites-available/default && rm /etc/nginx/sites-enabled/default
RUN echo "server{listen 80 default_server;listen [::]:80 default_server ipv6only=on;location /{ root /var/www/blog/output;}}" > /etc/nginx/sites-available/default
RUN ln -s /etc/nginx/sites-available/default /etc/nginx/sites-enabled/default
# nginx has to be running
RUN echo "[program:nginx]" >> /etc/supervisor/conf.d/supervisord.conf
RUN echo "command=/usr/sbin/nginx" >> /etc/supervisor/conf.d/supervisord.conf
# here's what will be done at run
CMD echo "root initial password: $(cat /root/default-passwd)" && bash -c 'if [ ! -e $BLOG_DIR/output ]; then bash /root/generate-blog.sh ; fi;' && /usr/bin/supervisord
