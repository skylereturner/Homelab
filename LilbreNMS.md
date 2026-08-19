# Monitoring Software (LibreNMS)
## Create files and Data Base
# 1. Create the persistent database and application folders
mkdir -p /home/sturner/librenms-config/db
mkdir -p /home/sturner/librenms-config/app

# 2. Grant full read/write permission to your sturner profile
sudo chown -R sturner:sturner /home/sturner/librenms-config
sudo chmod -R 775 /home/sturner/librenms-config

## On Docker UI app
version: '3.5'

services:
  db:
    image: mariadb:10.5
    container_name: librenms_db
    environment:
      - MYSQL_ROOT_PASSWORD=sturner_db_secure
      - MYSQL_DATABASE=librenms
      - MYSQL_USER=librenms
      - MYSQL_PASSWORD=librenms_vault_pass
    volumes:
      - /home/sturner/librenms-config/db:/var/lib/mysql
    restart: always

  redis:
    image: redis:alpine
    container_name: librenms_redis
    restart: always

  librenms:
    image: librenms/librenms:latest
    container_name: librenms
    ports:
      - "8080:80"
    volumes:
      - /home/sturner/librenms-config/app:/data
    environment:
      - TZ=America/Winnipeg
      - DB_HOST=db
      - DB_USER=librenms
      - DB_PASSWORD=librenms_vault_pass
      - DB_NAME=librenms
    depends_on:
      - db
      - redis
    restart: always
## Link it back to homepage:
nano /home/sturner/dashboard-config/bookmarks.yamml
- Network Monitor:
    - icon: librenms
      href: http://192.168.100.36:8080
