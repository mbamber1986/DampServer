FROM httpd:2.4
RUN apt-get update
RUN apt-get install -y nano

ARG workdir
ARG web_root


WORKDIR $workdir

# Copy apache vhost file to proxy php requests to php-fpm container
COPY httpd.conf /usr/local/apache2/conf/httpd.conf

COPY apache.conf /usr/local/apache2/conf/demo.apache.conf
RUN echo "Include /usr/local/apache2/conf/demo.apache.conf" \
    >> /usr/local/apache2/conf/httpd.conf
