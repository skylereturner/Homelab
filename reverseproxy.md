in portainer:  
  
version: '3.8'  
services:  
  npm:  
    image: 'jc21/nginx-proxy-manager:latest'  
    container_name: nginx-proxy-manager  
    restart: unless-stopped  
    ports:  
      - '80:80'   # Public HTTP Traffic   
      - '443:443' # Public HTTPS Traffic  
      - '81:81'   # NPM Admin Web UI  
    volumes:  
      - ./npm/data:/data  
      - ./npm/letsencrypt:/etc/letsencrypt  
    networks:  
      - proxy-network  
  
networks:  
  proxy-network:  
    name: proxy-network  
    driver: bridge  
