FROM nginx:1.19.7

# links
RUN ln -sf /dev/stdout /var/log/nginx/access.log && \
    ln -sf /dev/stderr /var/log/nginx/error.log && \
    rm -rf /etc/nginx/conf.d/*

# install openssl
RUN apt-get update && apt-get install openssl && \
apt-get clean && rm -rf /var/lib/apt/lists/*

# create folder for certificates
RUN mkdir -p /etc/nginx/certificates

# generate certificates
RUN openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/nginx/certificates/key.pem -out /etc/nginx/certificates/cert.pem -subj "/C=PL/ST=Warsaw/L=Warsaw/O=Security/OU=Development/CN=example.com"