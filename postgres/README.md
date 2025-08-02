
# 🐘 PostgreSQL 17 Docker Setup

Set up a lightweight and persistent PostgreSQL 17 instance on your Raspberry Pi using Docker.

## 🚀 Features

- PostgreSQL 17 with Alpine base (small footprint)
- Stores data persistently on your Pi
- Timezone correctly set to Montreal
- Custom port mapping for convenience

## 📦 Docker Setup

1. **Ensure the data directory exists and is owned by your user (e.g., `pi`)**  
This avoids permission issues when the container tries to write to the mounted volume:
```bash
mkdir -p /home/pi/container/postgres/data
sudo chown -R pi:pi /home/pi/container/postgres/data
```

2. **Create the container using the `postgres.yaml` compose file**  
You can do this with Portainer or directly using the command:
```bash
docker compose -f postgres.yaml up -d
```

3. **Access PostgreSQL**  
Connect to the database from your host machine or another container using:
```bash
psql -h <your_raspberry_pi_ip> -p 35432 -U postgres
```

Or from another Docker container in the same network:
```bash
psql -h postgres -U postgres
```

Or 🌐 Connect with CloudBeaver

You can use CloudBeaver [cloudbeaver.yaml](https://github.com/aldoruizw/docker/blob/main/cloudbeaver/cloudbeaver.yaml) — a web-based database management tool — to connect to your PostgreSQL instance.

Just deploy the CloudBeaver container from the provided setup and connect to your PostgreSQL using the following parameters:

- **Host**: `<your_raspberry_pi_ip>`
- **Port**: `35432`
- **Username**: `postgres`
- **Password**: `postgres`
- **Database**: any existing or new database

This provides a convenient graphical interface for managing and querying your PostgreSQL database from your browser.

## 📁 Volume Mount

| Host Path                            | Container Path                  | Description              |
|-------------------------------------|----------------------------------|--------------------------|
| `/home/pi/container/postgres/data` | `/var/lib/postgresql/data`      | Persistent data storage  |

## ⚙️ Configuration

| Variable           | Value              |
|--------------------|--------------------|
| `POSTGRES_PASSWORD` | `postgres`         |
| `TZ`               | `America/Montreal` |

## 🛠 Tips

- Port `35432` maps to the internal `5432`, so you can run multiple DB containers without conflict.

---

©️ Powered by [PostgreSQL](https://www.postgresql.org/) and Docker 🐳
