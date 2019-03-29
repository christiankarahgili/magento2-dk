FROM mike/ubuntu_apache_mysql
MAINTAINER chandlerbing7@gmail.com

# Magento2
RUN git clone https://github.com/magento/magento2.git /var/www/html/magento2ce/

RUN cd /var/www/html/magento2ce/ && \
	echo " " >> .gitignore && \
	echo "/magento2-sample-data/" >> .gitignore

RUN apt --purge autoremove -y ; apt clean