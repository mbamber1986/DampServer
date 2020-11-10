FROM httpd:2.4.46
RUN apt-get update 
RUN apt-get install nano -y

ENV workdir = ${workdir}
ENV web_root = ${web_root}

RUN useradd -G www-data,root -u 1000 -d /home/user user
RUN mkdir -p /home/user/.composer && \
    chown -R user:user /home/user

# Copy apache vhost file to proxy php requests to php-fpm container
COPY httpd.conf /usr/local/apache2/conf/httpd.conf

COPY vhosts.conf /usr/local/apache2/conf/vhosts.conf
RUN echo "Include /usr/local/apache2/conf/vhosts.conf" \
    >> /usr/local/apache2/conf/httpd.conf
