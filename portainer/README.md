# Portainer Setup with Docker

To get started with Portainer using Docker:

1. **Create a Docker volume that will contain the data managed by the Portainer server**  
   Run:  
   `docker volume create portainer_data`

2. **Create a docker-compose.yaml file with the script portainer.yml**  

3. **Create the container**  
   Start it using Docker Compose:  
   `docker-compose up -d`

4. **Access Portainer**  
   Open your browser and go to:  
   `https://localhost:9443`
