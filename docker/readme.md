# Docker setup

Follow these steps to set up Docker on your system.

## 1. Set Up Docker's Apt Repository  
Run the following commands to add Docker's official GPG key and apt repository:
```bash
# Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
echo \
"deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
$(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}") stable" | \
sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
```
## 2. Install the Docker Packages  
To install Docker, run:
```bash
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```
## 3. Verify Installation  
Verify the installation by running the hello-world image:
```bash
sudo docker run hello-world
```
## Post Install Step

Follow these steps to configure Docker to run without `sudo`:

### 1. Create the Docker Group
```bash
sudo groupadd docker
```
### 2. Add Your User to the Docker Group  
Add your user to the Docker group:
```bash
sudo usermod -aG docker $USER
```
### 3. Log Out and Log Back In  
Log out and back in to re-evaluate your group membership. Alternatively, run:
```bash
newgrp docker
```
### 4. Verify Docker Runs Without `sudo`  
Test if you can run Docker commands without `sudo`:
```bash
docker run hello-world
```
### Handling Permission Issues  
If you see the following error after running Docker with `sudo` before adding your user to the Docker group:
```bash
WARNING: Error loading config file: /home/user/.docker/config.json -
stat /home/user/.docker/config.json: permission denied
```
You can fix it by either removing the `~/.docker/` directory or changing its ownership and permissions:
```bash
sudo chown "$USER":"$USER" /home/"$USER"/.docker -R
sudo chmod g+rwx "$HOME/.docker" -R
```
## Logging Driver Configuration

By default, Docker uses the `json-file` logging driver, which can lead to large log files over time. To prevent this, consider one of the following options:

* Configure the `json-file` logging driver for log rotation.
* Use the `local` logging driver, which automatically rotates logs.
* Use a logging driver that sends logs to a remote logging aggregator.

### Difference Between `json-file` and `local` Log Drivers
#### `json-file`:
- Logs are stored in a JSON format.
- Easy to parse and analyze but may get large quickly.
- Human-readable but can increase disk usage if logs are detailed.

#### `local`:
- More space-efficient, compresses log files automatically.
- Stores logs in a binary format (not human-readable).
- Includes log rotation with a default size of 100MB and 5 files retained.

### Setting Up Log Rotation  
To configure log rotation, run:
```bash
sudo nano /etc/docker/daemon.json
```
Add the following configuration:
```json
{
  "log-driver": "local",
  "log-opts": {
    "max-size": "100m",
    "max-file": "5"
  }
}
```