# Web Server
In Portimer stack deployment:

version: '3'
services:
  webserver:
    image: nginx:alpine
    container_name: web_server
    ports:
      - "8082:80"
    volumes:
      - /home/sturner/my-website:/usr/share/nginx/html
    restart: always
    ## Create HTML file in Docker VM:
    nano /home/YOURUSERNAME/my-website/index.html
    Type code in this file
    ## Link back to Homepage UI:
    nano /home/sturner/dashboard-config/bookmarks.yaml
- Web Server:
    - icon: nginx
      href: http://192.168.100.36:8082

    
