FROM mike/magento_config
MAINTAINER chandlerbing7@gmail.com

# PHP
RUN apt install apt-transport-https lsb-release ca-certificates -y ; \
	apt install wget -y ; \
	wget -O /etc/apt/trusted.gpg.d/php.gpg https://packages.sury.org/php/apt.gpg ; \
	echo 	"deb https://packages.sury.org/php/ $(lsb_release -sc) main" > \
			/etc/apt/sources.list.d/php.list ; \
	apt update ; apt install php7.2 -y
RUN	apt install -y 	php7.2-bcmath php7.2-bz2 php7.2-cli php7.2-common \
					php7.2-curl php7.2-dba php7.2-gd php7.2-gmp php7.2-imap \
					php7.2-intl php7.2-ldap php7.2-mbstring  \
					php7.2-mysql php7.2-odbc php7.2-pgsql php7.2-recode \
					php7.2-snmp php7.2-soap php7.2-sqlite php7.2-tidy \
					php7.2-xml php7.2-xmlrpc php7.2-xsl php7.2-zip 
RUN update-alternatives --set php /usr/bin/php7.2 ; \
	a2dismod php7.0 ; a2enmod php7.2 ; service apache2 restart

# Magento2 and Sample Data
RUN cd /var/www/html/magento2ce/ && \
	git checkout 2.3.1 --force && \
	git checkout -b magento_2_3_1 && \
	rm -rf .git && \
	git init && \
	git remote add origin https://github.com/magento/magento2-sample-data.git && \
	git fetch && \
	git checkout 2.3.1 --force && \
	git pull origin 2.3.1 && \
	composer install && \
	composer update ; bin/magento setup:upgrade ; \
	chmod -R 777 .
RUN /etc/init.d/mysql start && \
	cd /var/www/html/magento2ce/ &&  \
	php bin/magento setup:install --base-url=http://magento2u.loc:83 \
	--admin-firstname=admin --admin-lastname=admin --backend-frontname=admin \
	--admin-email=chandlerbing7@gmail.com --admin-user=admin \
	--admin-password=mageU123 --db-name=m2 --db-host=localhost --db-user=root \
	--use-rewrites=1 --admin-use-security-key=0 ; \
	php bin/magento cache:disable full_page ; \ 
	chmod -R 777 /var/www/html/magento2ce/

# Mysql
COPY ./mysql/my.cnf /etc/mysql/my.cnf
RUN /etc/init.d/mysql restart

RUN apt --purge autoremove -y ; apt clean

ENTRYPOINT 	/etc/init.d/mysql start && service sendmail restart && \
			service apache2 restart && bash

EXPOSE 80