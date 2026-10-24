# Mount Google Drive with rclone inside a Plex Docker container
 
Steps to install rclone inside the `linuxserver/plex` container and mount a Google Drive folder at `/Videos` (read-only, on-demand streaming).

## 1. Connect to the container as root
 
```sh
docker exec -it plex sh
```
 
## 2. Install rclone and FUSE
 
```sh
apt-get update && apt-get install -y curl unzip ca-certificates
curl https://rclone.org/install.sh | bash
apt-get install -y fuse3
```
 
## 3. Configure the rclone remote
 
```sh
rclone config
```
 
- Name the remote `GoogleDrive`.
- Scope: `drive.readonly`.
- Set `root_folder_id` (advanced config) to limit rclone to a single folder.
The config is stored in `/root/.config/rclone/rclone.conf` (persisted by the volume above).
 
## 4. Create the videos folder
 
```sh
mkdir /Videos
```
 
## 5. Mount automatically when the container restarts
 
```sh
mkdir -p /config/scripts /custom-cont-init.d
cat > /config/scripts/rclone-mount.sh <<'EOF'
#!/bin/bash
(
  sleep 30
  rclone mount GoogleDrive: /Videos --read-only --allow-other --daemon --vfs-cache-mode full --vfs-cache-max-size 40G --vfs-cache-max-age 24h --vfs-read-ahead 256M --buffer-size 64M --dir-cache-time 12h --poll-interval 1m
) &
EOF
chmod +x /config/scripts/rclone-mount.sh
ln -sf /config/scripts/rclone-mount.sh /custom-cont-init.d/10-rclone.sh
```
 
## 6. Verify
 
Restart the container and check that `/Videos` contains your files:
 
```sh
docker restart plex
docker exec plex ls /Videos
```
 
Or mount it manually right away:
 
```sh
rclone mount GoogleDrive: /Videos --read-only --allow-other --daemon --vfs-cache-mode full --vfs-cache-max-size 40G --vfs-cache-max-age 24h --vfs-read-ahead 256M --buffer-size 64M --dir-cache-time 12h --poll-interval 1m
```
 
Then add `/Videos` as a library in Plex.
 
## Notes
 
- Everything installed inside the container (rclone, FUSE, the symlink in `/custom-cont-init.d`) is lost if the container is **recreated** (`--force-recreate`, image update). A simple `docker restart` keeps it. After a recreation, repeat steps 2 and the `ln -sf` command from step 5. The script in `/config/scripts` and `rclone.conf` persist through the volumes.
- To avoid Plex downloading whole files, disable video preview thumbnails, intro/credits detection and extensive media analysis in the library settings.
- If the OAuth token expires, run `rclone config reconnect GoogleDrive:`.
