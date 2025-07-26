# 📁 FileBrowser Docker Setup

Easily manage and share files on your Raspberry Pi through a web-based interface using [FileBrowser](https://filebrowser.org/).

## 🚀 Features

- Simple, user-friendly web UI  
- Browse, upload, and edit files  
- Authentication support  
- Fast and lightweight  

## 📦 Docker Setup

1. **Create the container using the `filebrowser.yml` compose file**

2. **Get the admin password**  
You can get the password by checking the container logs using Portainer or by running the command:
```bash
docker logs filebrowser
```
You should see output like this with the randomly generated admin password:  
```log
2025/07/26 19:06:43 Warning: filebrowser.db can't be found. Initializing in /database/
2025/07/26 19:06:43 Using database: /database/filebrowser.db
2025/07/26 19:06:43 Using config file: /config/settings.json
2025/07/26 19:06:43 Randomly generated password for user 'admin': x9rMR7iz-QEQEqSS
2025/07/26 19:06:43 Listening on [::]:80
```

3. **Access FileBrowser**  
Open your browser and go to:  
```
http://<your-raspberry-pi-ip>:30080
```  
Default login credentials:  
- **Username**: `admin`  
- **Password**: `x9rMR7iz-QEQEqSS (the generated password from logs)`  

_Remember to change your password after the first login!_

## 📁 File Mounts

| Host Path     | Container Path | Description           |
|---------------|----------------|-----------------------|
| `/home/pi`    | `/srv/home`    | Access Pi home folder |
| `/media/sda1` | `/srv/usb`     | Access external disk  |

## 🛠 Tips

- If port `30080` is restricted or in use, select another available port  
- Add more folders by modifying the `volumes` section in the compose file  
- Use a config volume (`/config`) to persist your settings  

---

©️ Powered by [FileBrowser](https://github.com/filebrowser/filebrowser) and Docker 🐳
