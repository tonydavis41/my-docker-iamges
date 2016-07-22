FROM fedora:latest
MAINTAINER Anthony J Davis <tony@justsomestuff.co.uk>

RUN dnf -y install httpd php php-common php-xml openssl mod_ssl tar && dnf -y clean all \
     && sed -i.ORIG 's/#ServerName/ServerName/' /etc/httpd/conf/httpd.conf

COPY test-proxy.conf /etc/httpd/conf.d

COPY run_apache_foreground /etc/httpd 

RUN mkdir /etc/httpd/ssl && \
    openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/httpd/ssl/apache.key -out /etc/httpd/ssl/apache.crt -subj "/C=GB/ST=London/L=London/O=JSS/OU=testing/CN=tony-proxy" && cd /var && rm -f lock && ln -s ../run lock

COPY ioncube_loaders_lin_x86-64_5.1.2.tar.gz /var/tmp

RUN cd /usr/local && tar xzf /var/tmp/ioncube_loaders_lin_x86-64_5.1.2.tar.gz && cd /usr/lib64/php/modules && cp /usr/local/ioncube/ioncube_loader_lin_5.6* . && chmod 755 ioncube* && cd /etc/php.d && echo "zend_extension = /usr/lib64/php/modules/ioncube_loader_lin_5.6.so" >> 00-ioncube.ini
 
ENTRYPOINT ["/etc/httpd/run_apache_foreground"]

