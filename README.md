# Docker-Compose-Secret


Create a **docker-compose** with secrets **without** swarm


```Yml
version: "3.9"

services:

  db:
    image: mysql:5.7
    volumes:
      - ./db_data:/var/lib/mysql
    restart: always
    ports:
      - "3306:3306"
    secrets:
      - dbpass
      - ropass
      - usdb
      - dbdb
    environment:
      MYSQL_ROOT_PASSWORD_FILE: /run/secrets/ropass
      MYSQL_DATABASE_FILE: /run/secrets/dbdb
      MYSQL_USER_FILE: /run/secrets/usdb
      MYSQL_PASSWORD_FILE: /run/secrets/dbpass

  wordpress:
    depends_on:
      - db
    image: wordpress:latest
    volumes:
      - ./wordpress_data:/var/www/html
    ports:
      - "80:80"
      - "443:443"
    restart: always
    secrets:
      - dbpass
      - usdb
      - dbdb
    environment:
      WORDPRESS_DB_HOST: db:3306
      WORDPRESS_DB_USER_FILE: /run/secrets/usdb
      WORDPRESS_DB_PASSWORD_FILE: /run/secrets/dbpass
      WORDPRESS_DB_NAME_FILE: /run/secrets/dbdb

secrets:
  dbpass:
    file: ./.secret/db_pas
  ropass:
    file: ./.secret/ro_pas
  usdb:
    file: ./.secret/us_db
  dbdb:
    file: ./.secret/db_db

volumes:
    db_data: {}
    wordpress_data: {}
```
Run ```docker-compose up -d --build db wordpress```


Is no the best solution, but it's work.

Remenber add **_FILE** to environment variable.

Thank [ronaldb/docker-compose.yml](ronaldb/docker-compose.yml)
