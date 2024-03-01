**1. To issue Certificate**

- Write nginx http configuration file `ssl.default.conf` as

    ```
    server {
        listen 80 default_server;
        server_name _;

        location / {
            proxy_pass http://app:8800;  
            #app --> name of the server container
        }
        location ~ /.well-known/acme-challenge/ {
            root /var/www/certbot;
        }
    }
    ```

- Then update `ssl.docker-compose.yml` file as below
    ```
    version: "3.8"

    services:
    app:
        container_name: app
        build:
        context: .
        ports:
        - "8800"
        environment:
        #Your environment variables

    nginx:
        container_name: nginx
        restart: unless-stopped
        image: nginx:latest
        ports:
        - "80:80"
        - "443:443"
        volumes:
        - ./nginx.conf:/etc/nginx/nginx.conf
        - ./ssl.default.conf:/etc/nginx/conf.d/ssl.default.conf
        - ./certbot/conf:/etc/letsencrypt
        - ./certbot/www:/var/www/certbot

        depends_on:
        - app

    certbot:
        container_name: certbot
        image: certbot/certbot:latest
        volumes:
            - ./certbot/conf/:/etc/letsencrypt
            - ./certbot/www/:/var/www/certbot
        command: certonly --webroot -w /var/www/certbot --force-renewal --email <example@email.com> -d <example.com> --agree-tos

        #add --staging flag at the end of command if you want to issue certificate for testing purpose
    ```
    Replace with your email and domain at `<example@email.com>` and `<example.com>`

- Run the above docker file on the cloud using following command
  `sudo docker compose -f ssl.docker-compose.yml up`


**2. Verify and Run the ssl certificate**

- Write `default.conf` file for nginx as follows
    ```
    server {

        listen 80 default_server;
        server_name <your-domain.com>;

        location ~ /.well-known/acme-challenge/ {
            root /var/www/certbot;
        }
        return 301 https://$host$request_uri;
    }

    server {
        listen 443 ssl http2;

        ssl_certificate     /etc/letsencrypt/live/<your-domain.com>/fullchain.pem;
        ssl_certificate_key /etc/letsencrypt/live/<your-domain.com>/privkey.pem;

        server_name <your-domain.com>;

        location / {
            proxy_pass http://app:8800;
        }

        location ~ /.well-known/acme-challenge/ {
            root /var/www/certbot;
        }
    }
    ```

- Write `docker-compose.yml` as below
  ```
    version: "3.8"

    services:
    app:
        container_name: app
        build:
        context: .
        ports:
        - "8800"
        environment:

        depends_on:
        - mongodb

    nginx:
        container_name: nginx
        restart: unless-stopped
        image: nginx:latest
        ports:
        - "80:80"
        - "443:443"
        volumes:
        - ./nginx.conf:/etc/nginx/nginx.conf
        - ./default.conf:/etc/nginx/conf.d/default.conf
        - ./certbot/conf/:/etc/letsencrypt
        - ./certbot/www/:/var/www/certbot
        depends_on:
        - app

    certbot:
        container_name: certbot
        image: certbot/certbot:latest
        volumes:
            - ./certbot/www/:/var/www/certbot
            - ./certbot/conf/:/etc/letsencrypt
        command: certonly --webroot -w /var/www/certbot --force-renewal --email <your-email.com> -d <your-domain.com> --agree-tos

  ```

- Run the above docker-compose.yml on the cloud

**3. AutoRenew the certificate**


   

