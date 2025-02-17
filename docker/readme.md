# Docker setup
1. Set up Docker's apt repository
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
2. Install the Docker packages
    ```bash
    sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
    ```
3. Verify that the installation is successful by running the hello-world image
    ```bash
    sudo docker run hello-world
    ```
## Post Install Step
1. Create the docker group
    ```bash
    sudo groupadd docker
    ```
2. Add your user to the docker group
    ```bash
    sudo usermod -aG docker $USER
    ```
3. Log out and log back in so that your group membership is re-evaluated.
You can also run the following command to activate the changes to groups:
    ```bash
    newgrp docker
    ```
4. Verify that you can run docker commands without sudo.
    ```bash
    docker run hello-world
    ```
If you initially ran Docker CLI commands using sudo before adding your user to the docker group, you may see the following error:
```bash
WARNING: Error loading config file: /home/user/.docker/config.json -
stat /home/user/.docker/config.json: permission denied
```
This error indicates that the permission settings for the ~/.docker/ directory are incorrect, due to having used the sudo command earlier.

To fix this problem, either remove the ~/.docker/ directory (it's recreated automatically, but any custom settings are lost), or change its ownership and permissions using the following commands:
```bash
sudo chown "$USER":"$USER" /home/"$USER"/.docker -R
sudo chmod g+rwx "$HOME/.docker" -R
```
### Logging driver config
The default logging driver, json-file, writes log data to JSON-formatted files on the host filesystem. Over time, these log files expand in size, leading to potential exhaustion of disk resources.

To avoid issues with overusing disk for log data, consider one of the following options:
* Configure the json-file logging driver to turn on log rotation.
* Use an alternative logging driver such as the "local" logging driver that performs log rotation by default.
* Use a logging driver that sends logs to a remote logging aggregator.

Difference Between `json-file` and `local` Log Driver:
* `json-file`:
Logs are stored as JSON formatted text, which makes it easier to parse and analyze programmatically. This is particularly useful if you need to process logs using tools like jq or other JSON parsing utilities.
It's very human-readable, but the log size can get big if there is a lot of detailed information, since each log entry is stored in JSON format.

* `local`:
This log driver is more space-efficient than json-file because it compresses log files automatically.
It stores logs in a binary format, which is not human-readable, but it's more optimized for storage.
It has a built-in retention mechanism that keeps logs to a default size of 100MB, with the option to adjust the maximum size and the number of files kept (5 files by default).
The local driver might be a good choice if you want automatic compression and you're okay with not having logs in a human-readable format (or if you don’t need to access the logs frequently).

To setup log rotation, run:
```bash
sudo nano /etc/docker/daemon.json
```
Add the following:
```json
{
  "log-driver": "local",
  "log-opts": {
    "max-size": "100m",
    "max-file": "5"
  }
}
```