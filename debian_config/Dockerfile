FROM debian:latest
MAINTAINER chandlerbing7@gmail.com

ARG SSH_PRIVATE_KEY
ARG SSH_PUBLIC_KEY

# System
RUN	apt update
RUN apt install -y 	curl nano vim sendmail
RUN echo "127.0.0.1 localhost magento2u.loc" >> /etc/hosts

# Apache
RUN apt install -y 	apache2 \
						libapache2-mod-php && \
						apt-get clean
RUN apt install apt-transport-https lsb-release ca-certificates -y ; \
	apt install wget -y ; \
	apt install mc -y ; \
	wget -O /etc/apt/trusted.gpg.d/php.gpg https://packages.sury.org/php/apt.gpg ; \
	echo "deb https://packages.sury.org/php/ $(lsb_release -sc) main" > \
	/etc/apt/sources.list.d/php.list ; \
	apt update ; apt install php7.1 -y
RUN echo "ServerName localhost" >> /etc/apache2/apache2.conf
RUN a2enmod rewrite ; \
	a2enmod mpm_prefork
#	a2enmod php7.0

# Create user and assign to web server group
RUN adduser magento
RUN usermod -aG www-data magento

ENV APACHE_RUN_USER     magento
ENV APACHE_RUN_GROUP    www-data
ENV APACHE_LOG_DIR      /var/log/apache2
ENV APACHE_PID_FILE     /var/run/apache2.pid
ENV APACHE_RUN_DIR      /var/run/apache2
ENV APACHE_LOCK_DIR     /var/lock/apache2

COPY ./conf/000-default.conf /etc/apache2/sites-available/000-default.conf

RUN a2ensite 000-default

# PHP
RUN	apt install -y 	php7.1-bcmath php7.1-bz2 php7.1-cli php7.1-common \
					php7.1-curl php7.1-dba php7.1-gd php7.1-gmp php7.1-imap \
					php7.1-intl php7.1-ldap php7.1-mbstring php7.1-mcrypt \
					php7.1-mysql php7.1-odbc php7.1-pgsql php7.1-recode \
					php7.1-snmp php7.1-soap php7.1-sqlite php7.1-tidy \
					php7.1-xml php7.1-xmlrpc php7.1-xsl php7.1-zip

# MySQL
RUN apt update && DEBIAN_FRONTEND=noninteractive apt install -y mariadb-server
RUN /etc/init.d/mysql start && \
        mysql -u root -e \
        "use mysql;update user set plugin='' where User='root'; \
        flush privileges;create database m2\q"

# Composer
RUN curl -sS https://getcomposer.org/installer | php && \
    mv composer.phar /usr/local/bin/composer

# NodeJs
RUN curl -sL https://deb.nodesource.com/setup_12.x | bash -

# Git
RUN apt install -y git

RUN apt --purge autoremove -y ; apt clean