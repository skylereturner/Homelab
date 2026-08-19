# DOCKER INSTALL  
sudo apt update  
sudo apt install -y docker.io util-linux-extra  
sudo systemctl enable docker –now  
sudo usermod -aG docker $USER  
newgrp docker  
docker run hello-world  
docker ps -a  

# Setting up Portainer - web UI for Docker  
## Create Volume  
docker volume create portainer_data  
## Create script to launch portainer  
docker run -d -p 8000:8000 -p 9443:9443 --name portainer \  
  --restart=always \  
  -v /var/run/docker.sock:/var/run/docker.sock \  
  -v portainer_data:/data \  
  portainer/portainer-ce:latest  
  # Access  
  https://your_vm_ip:9443  
  # Creation of stack on portainer  
  On portainer - Stack
  version: '3'  
services:  
  homepage:  
    image: ghcr.io/gethomepage/homepage:latest  
    container_name: homepage  
    ports:  
      - "3000:3000"  
    volumes:  
      - /home/YOURUSERNAME/dashboard-config:/app/config  
    restart: always  
# Link back to the proxmox server from the dashboard in Portainer
nano /home/YOURUSERAME/dashboard-config/bookmarks.yaml

- Proxmox Server:
    - icon: proxmox
      href: https://YOUR_PROXMOX_IP_HERE:8006



