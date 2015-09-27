# docker Drupal
#
# Drupal VERSION       8.0
# DOCKER VERSION       1.7.1

FROM       daocloud.io/library/ubuntu:latest
MAINTAINER xiongjun dockerxman  <fenyunxx@163.com>

# Env
ENV TZ "Asia/Shanghai"
ENV TERM xterm

# Add sources.list
ADD sources.list /etc/apt/sources.list

# Install.
RUN apt-get update
  
RUN apt-get install -y byobu curl git htop unzip vim wget xz-utils

RUN dpkg-divert --local --rename --add /sbin/initctl
RUN ln -sf /bin/true /sbin/initctl  

RUN DEBIAN_FRONTEND=noninteractive apt-get -y install git mysql-client mysql-server apache2 libapache2-mod-php5 pwgen python-setuptools vim-tiny php5-mysql php-apc php5-gd php5-curl php5-memcache memcached drush mc
RUN DEBIAN_FRONTEND=noninteractive apt-get autoclean
RUN rm -rf /var/lib/apt/lists/*

# Add files.
ADD root/.bashrc /root/.bashrc
ADD root/.gitconfig /root/.gitconfig
ADD root/.scripts /root/.scripts

# Make mysql listen on the outside
RUN sed -i "s/^bind-address/#bind-address/" /etc/mysql/my.cnf
RUN sed -i "s/DocumentRoot\ \/var\/www\/html/DocumentRoot\ \/var\/www/g" /etc/apache2/sites-available/000-default.conf

RUN ln -s /etc/apache2/sites-available/000-default.conf /etc/apache2/sites-available/default

ADD apache2.conf /etc/apache2/apache2.conf

RUN easy_install supervisor
ADD ./start.sh /start.sh
ADD ./foreground.sh /etc/apache2/foreground.sh
ADD ./supervisord.conf /etc/supervisord.conf

# Retrieve drupal
RUN rm -rf /var/www/
RUN mkdir /var/www/
RUN wget -O /tmp/drupal.tar.gz http://ftp.drupal.org/files/projects/drupal-8.0.0-beta15.tar.gz
RUN tar -xzvf /tmp/drupal.tar.gz -C /var/www/
RUN cp -ar /var/www/drupal-8.0.0-beta15/. /var/www/
RUN rm -rf /var/www/drupal-8.0.0-beta15


RUN wget -O /tmp/phpmyadmin.tar.xz http://mirrors.aliyun.com/ubuntu/pool/universe/p/phpmyadmin/phpmyadmin_4.4.13.1.orig.tar.xz
RUN xz -d /tmp/phpmyadmin.tar.xz
RUN tar -xvf /tmp/phpmyadmin.tar -C /tmp
RUN mv /tmp/phpMyAdmin-4.4.13.1-all-languages /var/www/phpmyadmin
RUN rm -rf /tmp/*

RUN chmod a+w /var/www/sites/default ; mkdir /var/www/sites/default/files ; chown -R www-data:www-data /var/www/

VOLUME ["/var/lib/mysql/"]

RUN chmod 755 /start.sh /etc/apache2/foreground.sh

EXPOSE 80

CMD ["/bin/bash", "/start.sh"]
