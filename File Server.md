 1. Create the empty storage and configuration folders cleanly
mkdir -p /home/sturner/filebrowser-config
mkdir -p /home/sturner/shared-files

# 2. Manually create the blank database binary file
touch /home/sturner/filebrowser-config/filebrowser.db

# 3. Change ownership from root to your personal sturner profile
sudo chown -R sturner:sturner /home/sturner/filebrowser-config /home/sturner/shared-files

# 4. Grant read and write permissions so Docker can modify the files
sudo chmod -R 775 /home/sturner/filebrowser-config /home/sturner/shared-files

# Inside of docker UI
version: '3'
services:
  filebrowser:
    image: filebrowser/filebrowser:latest
    container_name: file_server
    user: 1000:1000
    ports:
      - "8081:80"
    volumes:
      - /home/sturner/shared-files:/srv
      - /home/sturner/filebrowser-config/filebrowser.db:/database/filebrowser.db
    restart: always
