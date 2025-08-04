
# 🪟 Windows 11 ARM on Raspberry Pi with Docker

Run a lightweight version of Windows 11 inside a container on your Raspberry Pi 5 using [dockurr/windows-arm](https://github.com/dockurr/windows).

## 🚀 Features

- Runs Windows 11 ARM with Tiny11-Lite
- Remote access via RDP (Remote Desktop Protocol)
- VNC access through web browser
- Persist storage and shared downloads folder
- Customizable RAM, disk, region, and keyboard layout

## 📦 Docker Setup

1. **Create the container using the `windows.yaml` file**

3. **Access the Windows VM**

### 🖥 Remote Desktop (RDP)
Open **Microsoft Remote Desktop** and connect to:
```
<raspberry-pi-ip>:33389
```

### 🌐 Browser (QEMU noVNC)
Open your browser and go to:
```
http://<raspberry-pi-ip>:38006
```

This opens the Windows desktop via noVNC.

> 🛡 **Note**: The web-based viewer is useful for first-time setup or quick access.

## 📁 File Mounts

| Host Path               | Container Path | Description             |
|-------------------------|----------------|-------------------------|
| `/home/pi/container/windows` | `/storage`      | Persistent VM disk and config |
| `/media/sda1/Downloads` | `/data`        | Shared folder from host |

## ⚙️ Configuration Tips

- Customize RAM by editing `RAM_SIZE` (e.g., `"2G"`)
- You can expose more ports if needed
- Adjust other parameters in the `environment:` section of the compose file

---

©️ Powered by [dockurr/windows](https://github.com/dockurr/windows) and Docker 🐳
