
# inherit from base debian image
FROM ubuntu:16.04

# define maintainer name and e-mail
MAINTAINER Rob Frawley 2nd <rmf@src.run>

# update apt cache
RUN DEBIAN_FRONTEND=noninteractive apt-get update

# install ppa tools and add nginx mainline ppa
RUN DEBIAN_FRONTEND=noninteractive apt-get -y install python-software-properties
RUN DEBIAN_FRONTEND=noninteractive apt-get -y install software-properties-common
RUN DEBIAN_FRONTEND=noninteractive add-apt-repository ppa:nginx/development

# upgrade packages and install nginx
RUN apt-get -y dist-upgrade
RUN apt-get install -y nginx-full vim nano

# add our nginx and symfony web server configuration files
ADD nginx.conf /etc/nginx/
ADD symfony.conf /etc/nginx/sites-available/

# enable our symfony site and remove default
RUN ln -s /etc/nginx/sites-available/symfony.conf /etc/nginx/sites-enabled/symfony
RUN rm /etc/nginx/sites-enabled/default

# define the upstream to use for php-fpm
RUN echo "upstream php-upstream { server php:9000; }" > /etc/nginx/conf.d/upstream.conf

# set the default user to be able to access web directory
RUN usermod -u 1000 www-data

# run nginx
CMD ["nginx"]

# expose http and https ports
EXPOSE 80
EXPOSE 443
