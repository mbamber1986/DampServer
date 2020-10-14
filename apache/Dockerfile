FROM httpd:2.4.46
RUN apt-get update

ENV workdir = ${workdir}
ENV web_root = ${web_root}


# Copy apache vhost file to proxy php requests to php-fpm container
COPY httpd.conf /usr/local/apache2/conf/httpd.conf

COPY vhosts.conf /usr/local/apache2/conf/vhosts.conf
RUN echo "Include /usr/local/apache2/conf/vhosts.conf" \
    >> /usr/local/apache2/conf/httpd.conf
