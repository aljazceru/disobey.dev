---
title: "wordpress docker setup"
date: 2025-05-05
slug: "wordpress-docker-setup"
draft: false
---



I've recently had the need to set up wordpress quickly for some testing while developing a new woocommerce extension and I've stupidly needed way more time than I hoped to have a working set up in docker so here's the setup.

docker-compose.yml

```yaml
services:
  wordpress:
    image: wordpress:latest
    container_name: wordpress
    ports:
      - "12999:80"  # Only expose HTTP to the host
    environment:
      WORDPRESS_DB_HOST: db:3306
      WORDPRESS_DB_NAME: wordpress
      WORDPRESS_DB_USER: wordpress
      WORDPRESS_DB_PASSWORD: wordpresspass
    volumes:
      - ./wp-content:/var/www/html/wp-content/
    networks:
      - wp-network
    depends_on:
      - db

  db:
    image: mysql:5.7
    container_name: mysql
    environment:
      MYSQL_DATABASE: wordpress
      MYSQL_USER: wordpress
      MYSQL_PASSWORD: wordpresspass
      MYSQL_ROOT_PASSWORD: rootpass
    volumes:
      - db_data:/var/lib/mysql
    networks:
      - wp-network

volumes:
  db_data:

networks:
  wp-network:
    driver: bridge
```
nginx reverse proxy configuration. Key part is that you need to set https headers for the proxy otherwise wordpress will serve the content back in mixed mode and your browser will complain

```yaml 
server {
    server_name <server_name>;

    location / {
        proxy_pass http://localhost:12999;
        proxy_set_header X-Forwarded-Proto https;

        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }


    listen 443 ssl; # managed by Certbot
    ssl_certificate /etc/letsencrypt/live/<server_name>/fullchain.pem; 
    ssl_certificate_key /etc/letsencrypt/live/<server_name>/privkey.pem; 
    include /etc/letsencrypt/options-ssl-nginx.conf; 
    ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem;

}
```

