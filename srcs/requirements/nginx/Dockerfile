# # Base image
# FROM		debian:oldstable

# # Define build arguments passed from docker-compose.yml
# ARG			DOMAIN_NAME
# ARG			CERTS_KEY
# ARG			CERTS_CRT

# # Update and upgrade system & install Nginx and OpenSSL
# RUN			apt -y update && apt -y upgrade
# RUN			apt -y install nginx openssl

# # Generate self-signed certificate
# RUN			openssl req -x509 -sha256 -nodes -newkey rsa:4096 -days 365 \
# 			-subj "/C=BR/ST=Sao Paulo/L=Sao Paulo/O=42SP/OU=Inception/CN=macarval.42.fr" \
# 			-keyout ${CERTS_KEY} -out ${CERTS_CRT}

# # Guarantee that Nginx is using TSLv1.3
# RUN			sed -i 's/ssl_protocols.*/ssl_protocols TLSv1.3;/' /etc/nginx/nginx.conf

# # Copy configuration file
# COPY		./conf/https.conf /etc/nginx/conf.d/
# RUN			sed -i "s|INSERT_DOMAIN_NAME_HERE|${DOMAIN_NAME}|g" /etc/nginx/conf.d/https.conf
# RUN			sed -i "s|INSERT_KEY_PATH_HERE|${CERTS_KEY}|g" /etc/nginx/conf.d/https.conf
# RUN			sed -i "s|INSERT_CRT_PATH_HERE|${CERTS_CRT}|g" /etc/nginx/conf.d/https.conf

# # Expose ports
# EXPOSE		443

# # Run NGINX
# ENTRYPOINT	[ "nginx", "-g", "daemon off;" ]

FROM debian:oldstable

ARG			CERTS_KEY
ARG			CERTS_CRT

RUN apt update &&  apt upgrade -y && \
	apt install -y \
	nginx \
	openssl && \
	rm -rf /var/lib/apt/lists/*

RUN openssl req -batch -x509 -sha256 -nodes -newkey rsa:2048 -days 365 \
	-keyout /etc/ssl/private/${CERTS_KEY} \
	-out /etc/ssl/certs/${CERTS_CRT} \
	-subj "/C=BR/ST=SP/L=SãoPaulo/O=42sp/CN=macarval.42.fr"

COPY ./conf/nginx.conf /etc/nginx/conf.d

RUN chmod 755 /var/www/html && \
	chown -R www-data:www-data /var/www/html

ENTRYPOINT ["nginx", "-g", "daemon off;"]
