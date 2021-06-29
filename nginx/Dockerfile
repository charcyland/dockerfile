FROM docker.io/library/alpine:3.14.0

ARG nginx_path /etc/nginx
ARG nginx_conf ${nginx_path}/nginx.conf

RUN apk add --no-cache nginx supervisor

RUN mkdir -p /opt/nginx/conf.d
RUN mkdir /opt/nginx/certs
RUN mkdir /opt/nginx/html

RUN echo "daemon off;" >> ${nginx_conf}
RUN sed -i -e "100a\    include /opt/nginx/conf.d/*.conf" ${nginx_conf}

# forward request and error logs to docker log collector
RUN ln -sf /dev/stdout /var/log/nginx/access.log
RUN ln -sf /dev/stderr /var/log/nginx/error.log

# Configurations files
WORKDIR /tmp
RUN wget https://raw.githubusercontent.com/charcyland/dockerfile/master/nginx/supervisord.conf
RUN mv /tmp/supervisord.conf /etc/supervisord.conf

# Nginx logs to Docker log collector
RUN ln -sf /dev/stdout /var/log/nginx/access.log && \
	ln -sf /dev/stderr /var/log/nginx/error.log

EXPOSE 80
EXPOSE 443

VOLUME ["/opt/nginx"]

WORKDIR /
ENTRYPOINT ["/usr/bin/supervisord", "-n", "-c",  "/etc/supervisord.conf"]

