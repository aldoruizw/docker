# Portainer Setup with Docker

This repository contains Docker stacks managed via Portainer, running on a Raspberry Pi.

## Quick Start

### 1. Create a Docker volume

This volume will store Portainer's persistent data:

```bash
docker volume create portainer_data
