# Portainer Installation and Configuration

Follow these steps to install and configure Portainer on your system. Always refer to the [Official Documentation](https://docs.portainer.io/start/install-ce/server/docker/linux) for the latest instructions.

## 1. Set Up Persistent Data

You can choose to use a Docker volume or a host path bind mount for storing Portainer's persistent data.

### Option 1: Docker Volume

Create a Docker volume to store Portainer’s persistent data:
```bash
docker volume create portainer_data
```
### Option 2: Host Path Bind Mount
Create a directory on your host system for persistent data:
```bash
mkdir -p /your/path/of/choice/portainer_data
```
## 2. Start Portainer
You can start Portainer either using the docker run command or with Docker Compose
### Option 1: Docker Run with Docker Volume
Run Portainer using the docker run command, specifying the Docker volume you created for persistent data:
```bash
docker run -d -p 8000:8000 -p 9443:9443 --name portainer --restart=always \
    -v /var/run/docker.sock:/var/run/docker.sock \
    -v portainer_data:/data portainer/portainer-ce:lts
```
### Option 2: Docker Compose with Bind Mount
1. Navigate to the directory where you want to store your docker-compose.yml file:
```bash
cd /your/path/of/choice/portainer_data
```
2. Create the docker-compose.yml file:
```bash
nano docker-compose.yml
```
3. Paste the following content into the file:
```yaml
services:
  portainer:
    image: portainer/portainer-ce:lts  # Pin version to avoid auto-updates by Watchtower
    container_name: portainer
    volumes:
      - /your/path/of/choice/portainer_data:/data
      - /var/run/docker.sock:/var/run/docker.sock
    ports:
      - 9443:9443
      - 8000:8000
    restart: always
```
4. Start Portainer using Docker Compose:
```bash
docker-compose up -d
```
5. Log in to Portainer:
```bash
https://<ip-address>:9443
```
**Note:** Don't forget to use `HTTPS`