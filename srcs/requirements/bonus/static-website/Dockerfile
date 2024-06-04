# # Base image
# FROM		debian:oldstable

# ARG			CERTS_KEY
# ARG			CERTS_CRT

# # Update and upgrade system & install nodejs and npm
# RUN			apt update &&  apt upgrade -y
# RUN			apt install -y nginx openssl
# RUN			rm -rf /var/lib/apt/lists/*

# # Generate self-signed certificate
# RUN 		openssl req -x509 -sha256 -nodes -newkey rsa:4096 -days 365 \
# 			-subj "/C=BR/ST=Sao Paulo/L=Sao Paulo/O=42SP/OU=Inception/CN=macarval.42.fr" \
# 			-keyout ${CERTS_KEY} -out ${CERTS_CRT}

# # Copy the package nginx file
# COPY		./tools/index.html /usr/share/nginx/html
# COPY		./tools/imgs /usr/share/nginx/html/imgs
# COPY		./conf/nginx.conf /etc/nginx/conf.d

# # Permissions
# RUN			chmod 755 /var/www/html
# RUN			chown -R www-data:www-data /var/www/html

# # Expose port
# EXPOSE		3000

# # Run the application
# ENTRYPOINT	["nginx", "-g", "daemon off;"]

FROM debian:oldstable

RUN apk update && apk add nginx

COPY ./conf/nginx.conf /etc/nginx/http.d/default.conf

COPY		./tools/index.html /usr/share/nginx/html
COPY		./tools/imgs /usr/share/nginx/html/imgs

CMD ["nginx", "-g", "daemon off;"]
