# 🔧🌊 Portainer Setup with Docker

To get started with Portainer using Docker:

1. **🧱 Create a Docker volume to store Portainer data**
   Run the following command:
   ```bash
   docker volume create portainer_data
   ```

2. **📦 Create and run the Portainer container**
   - Create a `docker-compose.yaml` file (you can use the script `portainer.yml`).
   - Then start the container with:
     ```bash
     docker-compose up -d
     ```

3. **🌐 Access the Portainer Web UI**
   - Open your browser and go to:
     ```
     https://<your-raspberry-pi-ip>:39443
     ```
   - On first access, you’ll be prompted to set up an admin user.

✅ You’re now ready to manage your containers with a powerful, user-friendly interface!

✅ Reference: [Install Portainer CE with Docker on Linux](https://docs.portainer.io/start/install-ce/server/docker/linux)

# 🔁 Update Portainer to the Latest Version

To update Portainer to the latest version, follow these steps:

1. **Pull the latest image manually**
   Run the following command to download the newest version of the Portainer image:
   ```bash
   docker pull portainer/portainer-ce:latest
   ```

3. **Restart your container using Docker Compose**
   Shut down the current container and start it again with the updated image:
   ```bash
   docker-compose down
   docker-compose up -d
   ```

✅ **Note:** Just having `:latest` in the `image:` line of your `docker-compose.yaml` is *not enough* to ensure updates.
You must run `docker pull` to actually fetch the new image.
