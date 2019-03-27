FROM mike/ubuntu_apache_mysql
MAINTAINER chandlerbing7@gmail.com

# Magento2 and Sample Data
RUN git clone https://github.com/magento/magento2.git /var/www/html/magento2ce/
#	cd /var/www/html/magento2ce/ && \
#	git clone https://github.com/magento/magento2-sample-data.git

RUN cd /var/www/html/magento2ce/ && \
	echo " " >> .gitignore && \
	echo "/magento2-sample-data/" >> .gitignore

RUN apt --purge autoremove -y ; apt clean