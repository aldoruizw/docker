# Mount Google Drive with rclone inside a Plex Docker container
 
Steps to install rclone inside the `linuxserver/plex` container and mount a Google Drive folder at `/Videos` (read-only, on-demand streaming).

## 1. Connect to the container as root
 
```bash
docker exec -it plex /bin/bash
```
 
## 2. Install rclone and FUSE
 
```bash
apt-get update && apt-get install -y curl unzip ca-certificates
curl https://rclone.org/install.sh | bash
apt-get install -y fuse3
```
 
## 3. Configure the rclone remote
 
```bash
rclone config
```
```
Current remotes:

Name                 Type
====                 ====

e) Edit existing remote
n) New remote
d) Delete remote
r) Rename remote
c) Copy remote
s) Set configuration password
q) Quit config
- e/n/d/r/c/s/q> n
Enter name for new remote.
```
- name>`GoogleDrive`
```
Option Storage.
Type of storage to configure.
Choose a number from below, or type in your own value.
24 / Google Drive - Tier 1
   \ (drive)
```
- Storage> `24`
```
Option client_id.
Google Application Client Id
Leave blank to use rclone's shared client_id, or if you are using a service account.
The shared client_id is being retired and will stop working during 2026, so creating your own is now strongly recommended.
See https://rclone.org/drive/#making-your-own-client-id for how to create your own.
Enter a value. Press Enter to leave empty.
```
- client_id> `google_drive_api_client_id`
```
Option client_secret.
Google Application Client Secret
Leave blank to use rclone's shared client_id, or if you are using a service account.
If you created your own client_id then enter its client secret here.
Enter a value. Press Enter to leave empty.
```
- client_secret> `google_drive_api_client_secret`
```
Option scope.
Comma separated list of scopes that rclone should use when requesting access from drive.
Choose a number from below, or type in your own value.
Press Enter to leave empty.
 1 / Full access all files, excluding Application Data Folder.
   \ (drive)
 2 / Read-only access to file metadata and file contents.
   \ (drive.readonly)
   / Access to files created by rclone only.
 3 | These are visible in the drive website.
   | File authorization is revoked when the user deauthorizes the app.
   \ (drive.file)
   / Allows read and write access to the Application Data folder.
 4 | This is not visible in the drive website.
   \ (drive.appfolder)
   / Allows read-only access to file metadata but
 5 | does not allow any access to read or download file content.
   \ (drive.metadata.readonly)
```
- scope> `2`
```
Option service_account_file.
Service Account Credentials JSON file path.
Leave blank normally.
Needed only if you want use SA instead of interactive login.
Leading `~` will be expanded in the file name as will environment variables such as `${RCLONE_CONFIG_DIR}`.
Enter a value. Press Enter to leave empty.
```
- service_account_file>
```
Edit advanced config?
y) Yes
n) No (default)
```
- y/n> `y`
- Press enter until you find the parameter `root_folder_id` (advanced config) to limit rclone to a single folder.
```
Option root_folder_id.
ID of the root folder.
Leave blank normally.
Fill in to access "Computers" folders (see docs), or for rclone to use
a non root folder as its starting point.
Enter a value. Press Enter to leave empty.
```
- root_folder_id> `google_drive_root_folder_id`
- Press enter until you get this option
```
Edit advanced config?
y) Yes
n) No (default)
```
- y/n> `n`
```
Use web browser to automatically authenticate rclone with remote?
 * Say Y if the machine running rclone has a web browser you can use
 * Say N if running rclone on a (remote) machine without web browser access
If not sure try Y. If Y failed, try N.

y) Yes (default)
n) No
```
- y/n> `n`
```
Option config_token.
For this to work, you will need rclone available on a machine that has
a web browser available.
For more help and alternate methods see: https://rclone.org/remote_setup/
Execute the following on the machine with the web browser (same rclone
version recommended):
        rclone authorize "drive" "eyJj_tmp_token_1"
Then paste the result.
Enter a value.
```
- config_token> `eyJj_tmp_token_2`
```
```
- Configure this as a Shared Drive (Team Drive)? > n
```
```
- Keep this "GoogleDrive" remote? > y
```
```
- q

The config is stored in `/root/.config/rclone/rclone.conf` (persisted by the volume above).
 
## 4. Create the videos folder
 
```bash
mkdir /Videos
```
 
## 5. Mount automatically when the container restarts
 
```bash
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
 
```bash
docker restart plex
docker exec plex ls /Videos
```
 
Or mount it manually right away:
 
```bash
rclone mount GoogleDrive: /Videos --read-only --allow-other --daemon --vfs-cache-mode full --vfs-cache-max-size 40G --vfs-cache-max-age 24h --vfs-read-ahead 256M --buffer-size 64M --dir-cache-time 12h --poll-interval 1m
```
 
Then add `/Videos` as a library in Plex.
 
## Notes
 
- Everything installed inside the container (rclone, FUSE, the symlink in `/custom-cont-init.d`) is lost if the container is **recreated** (`--force-recreate`, image update). A simple `docker restart` keeps it. After a recreation, repeat steps 2 and the `ln -sf` command from step 5. The script in `/config/scripts` and `rclone.conf` persist through the volumes.
- To avoid Plex downloading whole files, disable video preview thumbnails, intro/credits detection and extensive media analysis in the library settings.
- If the OAuth token expires, run `rclone config reconnect GoogleDrive:`.
