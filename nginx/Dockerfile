FROM docker.io/library/alpine:latest

RUN apk add --no-cache wget supervisor nginx 

RUN mkdir -p /opt/nginx/conf.d
RUN mkdir /opt/nginx/certs
RUN mkdir /opt/nginx/html

RUN echo "daemon off;" >> /etc/nginx/nginx.conf
RUN sed -i -e "103a\        include /opt/nginx/conf.d/*.conf;" /etc/nginx/nginx.conf

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

EXPOSE 80/tcp
EXPOSE 443/tcp

VOLUME ["/opt/nginx"]

WORKDIR /
ENTRYPOINT ["/usr/bin/supervisord", "-n", "-c",  "/etc/supervisord.conf"]
