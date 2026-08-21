# Make AI:

## Create Directory:
mkdir -p ~/jarvis-ai && cd ~/jarvis-ai
## Create Compose File:
nano docker-compose.yml

version: '3.8'

services:
  ollama:
    image: ollama/ollama:latest
    container_name: ollama
    volumes:
      - ./ollama_data:/root/.ollama
    ports:
      - "11434:11434"
    restart: always
    environment:
      - OLLAMA_NUM_PARALLEL=1

  open-webui:
    image: ghcr.io/open-webui/open-webui:main
    container_name: open-webui
    volumes:
      - ./open_webui_data:/app/backend/data
    ports:
      - "3000:8080"
    environment:
      - OLLAMA_BASE_URL=http://ollama:11434
    restart: always
    depends_on:
      - ollama

## Turn it on
docker compose up -d

## Check ps docker -a

