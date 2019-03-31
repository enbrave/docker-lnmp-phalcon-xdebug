FROM nginx:1.15.8
RUN mv /etc/nginx/conf.d/default.conf /etc/nginx/conf.d/default.conf.org
COPY ./config-file/default.conf /etc/nginx/conf.d/default.conf
COPY ./config-file/phalcon.conf /etc/nginx/conf.d/phalcon.conf
