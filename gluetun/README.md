# 🔐 Gluetun + Rclone + Firefox Stack

This setup provides a secure environment where Firefox traffic is routed through a VPN using Gluetun, with Rclone mounted for cloud file access.

## 📦 Components

- **Gluetun**: VPN client container (WireGuard or OpenVPN)
- **Rclone**: Mount and manage cloud storage inside container
- **Firefox**: Web browser with all traffic tunneled via VPN

## 🛠️ Setup Instructions

1. Add your WireGuard configuration file:

   Place your `.conf` file (e.g. `your-vpn.conf`) inside the Gluetun config folder:
   ```
   /home/pi/container/gluetun/wireguard/your-vpn.conf
   ```
   ✅ Important: In your `.conf` file, you must use a **public IP address** instead of a DNS name for the VPN endpoint because Gluetun cannot resolve DNS names at startup.

   ❗️ This means that **each time your VPN server's public IP changes, you will need to update the IP address in your `.conf` file and restart the containers** to apply the change.

3. Use the `gluetun.yaml` Docker Compose file to create the containers.

4. Accessing Firefox

   Once the containers are running, access Firefox in your browser at:
   ```bash
   http://<your-raspberry-pi-ip>:33000
   ```
   > 🔒 Firefox traffic is routed through the Gluetun VPN for secure browsing.

6. Verify Your Public IP Address in rclone container

   To confirm your traffic is going through the VPN, you can check the public IP from inside the Rclone container:
   ```sh
   docker exec -it rclone wget -qO- https://ipv4.icanhazip.com
   ```
