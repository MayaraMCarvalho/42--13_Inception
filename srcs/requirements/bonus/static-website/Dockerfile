# Base image
FROM		debian:oldstable

ARG			CERTS_KEY
ARG			CERTS_CRT

# Update and upgrade system & install nodejs and npm
RUN			apt update &&  apt upgrade -y
RUN			apt install -y nginx openssl
RUN			rm -rf /var/lib/apt/lists/*

RUN			openssl req -batch -x509 -sha256 -nodes -newkey rsa:2048 -days 365 \
			-keyout /etc/ssl/private/${CERTS_KEY} \
			-out /etc/ssl/certs/${CERTS_CRT} \
			-subj "/C=BR/ST=SP/L=SãoPaulo/O=42sp/CN=macarval.42.fr"

# Copy the package nginx file
COPY		./tools/index.html /usr/share/nginx/html
COPY		./tools/imgs /usr/share/nginx/html/imgs
COPY		./conf/nginx.conf /etc/nginx/conf.d

# Permissions
RUN		chmod 755 /var/www/html
RUN		chown -R www-data:www-data /var/www/html

# Run the application
ENTRYPOINT ["nginx", "-g", "daemon off;"]
