FROM mike/magento_config
MAINTAINER chandlerbing7@gmail.com

# PHP
RUN apt install apt-transport-https lsb-release ca-certificates -y ; \
	apt install wget -y ; \
	wget -O /etc/apt/trusted.gpg.d/php.gpg https://packages.sury.org/php/apt.gpg ; \
	echo "deb https://packages.sury.org/php/ $(lsb_release -sc) main" > \
	/etc/apt/sources.list.d/php.list ; \
	apt update ; apt install php7.1 -y
RUN	apt install -y 	php7.1-bcmath php7.1-bz2 php7.1-cli php7.1-common \
					php7.1-curl php7.1-dba php7.1-gd php7.1-gmp php7.1-imap \
					php7.1-intl php7.1-ldap php7.1-mbstring php7.1-mcrypt \
					php7.1-mysql php7.1-odbc php7.1-pgsql php7.1-recode \
					php7.1-snmp php7.1-soap php7.1-sqlite php7.1-tidy \
					php7.1-xml php7.1-xmlrpc php7.1-xsl php7.1-zip 
RUN update-alternatives --set php /usr/bin/php7.1 ; \
	a2dismod php7.0 ; a2enmod php7.1 ; service apache2 restart

# Magento2 and Sample Data
RUN cd /var/www/html/magento2ce/ && \
	git checkout 2.2.7 --force && \ 
	git checkout -b magento_2_2_7 && \
	rm -rf .git && \
	git init && \
	git remote add origin https://github.com/magento/magento2-sample-data.git && \
	git fetch && \
	git checkout 2.2.7 --force && \
	git pull origin 2.2.7 && \
	composer install && \
	composer update ; bin/magento setup:upgrade ; \
	chmod -R 777 .
RUN /etc/init.d/mysql start && \
	cd /var/www/html/magento2ce/ &&  \
	php bin/magento setup:install --base-url=http://magento2u.loc:82 \
	--admin-firstname=admin --admin-lastname=admin --backend-frontname=admin \
	--admin-email=chandlerbing7@gmail.com --admin-user=admin \
	--admin-password=mageU123 --db-name=m2 --db-host=localhost \
	--db-user=root --use-rewrites=1 --admin-use-security-key=0 ; \
	php bin/magento cache:disable full_page ; \
	chmod -R 777 /var/www/html/magento2ce/

# Mysql
COPY ./mysql/my.cnf /etc/mysql/my.cnf
RUN /etc/init.d/mysql restart

RUN apt --purge autoremove -y ; apt clean

ENTRYPOINT 	/etc/init.d/mysql start && service sendmail restart && \
			service apache2 restart && bash

EXPOSE 80