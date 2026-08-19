# Monitoring Software (LibreNMS)
## Create files and Data Base
# 1. Create a clean stack code directory
mkdir -p /home/sturner/librenms-stack
cd /home/sturner/librenms-stack

# 2. Re-create the empty database and application storage folders
mkdir -p /home/sturner/librenms-config/db
mkdir -p /home/sturner/librenms-config/app

# 3. Grant full read/write permission levels to the host folders
sudo chmod -R 777 /home/sturner/librenms-config

## On Docker UI app
version: '3.5'

services:
  db:
    image: mariadb:10.5
    container_name: librenms_db
    command: [
      "mysqld",
      "--innodb-file-per-table=1",
      "--lower-case-table-names=1",
      "--character-set-server=utf8mb4",
      "--collation-server=utf8mb4_unicode_ci"
    ]
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
      - "8085:8000"
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
## Deploy:
docker compose up -d
docker compose down
## Link it back to homepage:
nano /home/sturner/dashboard-config/bookmarks.yamml
- Network Monitor:
    - icon: librenms
      href: http://192.168.100.36:8080

## To Create User:
docker compose exec --user librenms librenms lnms user:add --role=admin sturneradmin

