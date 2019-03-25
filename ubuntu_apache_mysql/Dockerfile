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
						php7.0-mysql \
						php7.0-mbstring \
						libapache2-mod-php && \
						apt-get clean
RUN echo "ServerName localhost" >> /etc/apache2/apache2.conf
RUN a2enmod rewrite ; \
	a2enmod mpm_prefork ; \
	a2enmod php7.0

ENV APACHE_RUN_USER     www-data
ENV APACHE_RUN_GROUP    www-data
ENV APACHE_LOG_DIR      /var/log/apache2
ENV APACHE_PID_FILE     /var/run/apache2.pid
ENV APACHE_RUN_DIR      /var/run/apache2
ENV APACHE_LOCK_DIR     /var/lock/apache2

RUN sed -i "s,/html,/html/magento2ce/ \n <Directory /var/www/html/magento2ce/> \
			\n Options FollowSymLinks MultiViews \n AllowOverride All \n Order allow\,deny \
			\n allow from all \n Require all granted \n </Directory> \n,g" \
			/etc/apache2/sites-available/000-default.conf 

# PHP
RUN	apt install -y 	php7.0-bcmath php7.0-bz2 php7.0-cli php7.0-common \
					php7.0-curl php7.0-dba php7.0-gd php7.0-gmp php7.0-imap \
					php7.0-intl php7.0-ldap php7.0-mbstring php7.0-mcrypt \
					php7.0-mysql php7.0-odbc php7.0-pgsql php7.0-recode \
					php7.0-snmp php7.0-soap php7.0-sqlite php7.0-tidy \
					php7.0-xml php7.0-xmlrpc php7.0-xsl php7.0-zip 

# MySQL
RUN apt update && DEBIAN_FRONTEND=noninteractive apt install -y mysql-server
RUN /etc/init.d/mysql start && \
        mysql -u root -e \
        "use mysql;update user set plugin='' where User='root'; \
        flush privileges;create database m2\q"

# Composer
RUN curl -sS https://getcomposer.org/installer | php && \
    mv composer.phar /usr/local/bin/composer

# Git
RUN apt install -y git
RUN mkdir /root/.ssh/
RUN echo "$SSH_PRIVATE_KEY" > /root/.ssh/id_rsa && \
    echo "$SSH_PUBLIC_KEY" > /root/.ssh/id_rsa.pub && \
    chmod 600 /root/.ssh/id_rsa && \
    chmod 600 /root/.ssh/id_rsa.pub
RUN touch /root/.ssh/known_hosts
RUN ssh-keyscan github.com >> /root/.ssh/known_hosts
RUN cd /var/www/html && rm index.html && git init && \
	git config --global user.email "chandlerbing7@gmail.com" && \
	git config --global user.name "Mykhailo Molochko"

RUN apt --purge autoremove -y ; apt clean