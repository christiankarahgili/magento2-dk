FROM mike/magento_config
MAINTAINER chandlerbing7@gmail.com

# PHP
RUN update-alternatives --set php /usr/bin/php7.1 ; \
	a2dismod php7.0 ; a2enmod php7.1 ; service apache2 restart

# Magento2 and Sample Data
RUN cd /var/www/html/magento2ce/ && \
	git checkout 2.2.8 --force && \ 
	git checkout -b magento_2_2_8 && \
	rm -rf .git && \
	git init && \
	git remote add origin https://github.com/magento/magento2-sample-data.git && \
	git fetch && \
	git checkout 2.2.8 --force && \
	git pull origin 2.2.8 && \
	rm -rf .git && \
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
	chmod -R 777 /var/www/html/magento2ce/

# Mysql
COPY ./mysql/my.cnf /etc/mysql/my.cnf
RUN /etc/init.d/mysql restart

RUN apt --purge autoremove -y ; apt clean

ENTRYPOINT 	/etc/init.d/mysql start && service sendmail restart && \
			service apache2 restart && bash

EXPOSE 80