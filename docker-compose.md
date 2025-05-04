    services:
    
      portainer:
        image: portainer/portainer-ce:latest
        restart: unless-stopped
        ports:
          - 9443:9443
        volumes:
          - data:/data
          - /var/run/docker.sock:/var/run/docker.sock
    
      wordpress:
        image: wordpress
        restart: always
        ports:
          - 8080:80
        environment:
          WORDPRESS_DB_HOST: db
          WORDPRESS_DB_USER: wordpress
          WORDPRESS_DB_PASSWORD: kilpakonna234
          WORDPRESS_DB_NAME: wordpress
        volumes:
          - wordpress:/var/www/html
    
      db:
        image: mysql:8.0
        restart: always
        environment:
          MYSQL_DATABASE: wordpress
          MYSQL_USER: wordpress
          MYSQL_PASSWORD: kilpakonna234
          MYSQL_RANDOM_ROOT_PASSWORD: '1'
        volumes:
          - db:/var/lib/mysql
    
    volumes:
     data:
     wordpress:
     db:
