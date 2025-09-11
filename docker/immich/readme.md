# Immich logging to file

Follow these steps to log Immich container output to a file using `journald`.

## 1. Enable Journald Logging for the Immich Container  
Start by using `journald` as the logging driver for the Immich container in your `docker-compose.yml`:
```yaml
services:
    immich-server:
        logging:
            driver: "journald"
            options:
                tag: "immich-server"
```
## 2. Create the Directory for Logs  
Create a directory where you want to store the log file:
```bash
mkdir /your/path/of/choice/logs
```
## 3. Create a Service to Forward Journald Logs to a File  
Create a new systemd service to forward the logs:
```bash
sudo nano /etc/systemd/system/immich-log-forward.service
```
Paste the following content into the service file, making sure to replace {user} and {group} with the appropriate values:
```ini
[Unit]
Description=Forward Immich logs to a file
After=network.target

[Service]
ExecStart=/bin/bash -c 'journalctl -t immich-server -f >> /your/path/of/choice/logs/immich.log 2>&1'
Restart=always
User={user}
Group={group}

[Install]
WantedBy=multi-user.target
```
## 4. Reload Systemd and Enable the Service  
Reload the systemd daemon and enable the log forwarding service:
```bash
sudo systemctl daemon-reload
sudo systemctl enable --now immich-log-forward
```
To check the status of the service:
```bash
systemctl status immich-log-forward
```