# docker Drupal
#
# Drupal VERSION       7.39
# DOCKER-VERSION       1.7.1

FROM       daocloud.io/library/ubuntu:latest
MAINTAINER xiongjun dockerxman  <fenyunxx@163.com>

# Env
ENV TZ "Asia/Shanghai"
ENV TERM xterm

# Add sources.list
ADD sources.list /etc/apt/sources.list

# Install.
RUN apt-get update
  
RUN apt-get install -y byobu curl git htop unzip vim wget

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

ADD apache2.conf /etc/apache2/apache2.conf
RUN ln -s /etc/apache2/sites-available/000-default.conf /etc/apache2/sites-available/default

RUN easy_install supervisor
ADD ./start.sh /start.sh
ADD ./foreground.sh /etc/apache2/foreground.sh
ADD ./supervisord.conf /etc/supervisord.conf

# Retrieve drupal
RUN rm -rf /var/www/ ; cd /var ; drush dl drupal ; mv /var/drupal*/ /var/www/

RUN wget -O /tmp/phpmyadmin.zip http://files.phpmyadmin.net/phpMyAdmin/4.4.14.1/phpMyAdmin-4.4.14.1-all-languages.zip
RUN unzip /tmp/phpmyadmin.zip -d /var/www/
RUN rm /tmp/phpmyadmin.zip
RUN mv /var/www/phpMyAdmin-4.4.14.1-all-languages/ /var/www/phpmyadmin 

RUN chmod a+w /var/www/sites/default ; mkdir /var/www/sites/default/files ; chown -R www-data:www-data /var/www/

VOLUME ["/var/lib/mysql/"]

RUN chmod 755 /start.sh /etc/apache2/foreground.sh

EXPOSE 80

CMD ["/bin/bash", "/start.sh"]
