# Webhook Server Installation & Configuration

This guide walks you through installing, configuring, and running a webhook server on Ubuntu.

## 1. Install `webhook`
Install the webhook package using apt:
```bash
sudo apt update
sudo apt install webhook
```

## 2. Configure Hooks
Create a directory for your webhook scripts and configuration:
```bash
mkdir -p /home/fantaz/scripts/webhook
cd /home/fantaz/scripts/webhook
touch hooks.json
nano hooks.json
```
**Example** `hooks.json:`
```json
[
  {
    "id": "redeploy-webhook",
    "execute-command": "/var/scripts/redeploy.sh",
    "command-working-directory": "/var/www/mysite"
  }
]
```
**Notes:**
- The `id` field defines the unique webhook identifier.
- `execute-command` is the script that will run when the webhook is triggered.
- `command-working-directory` is the directory in which the command runs.
- You can add custom trigger rules for security (e.g., secret tokens). See [Hook Rules Documentation](https://github.com/adnanh/webhook/blob/master/docs/Hook-Rules.md).


## 3. Test the Webhook Locally
Before enabling as a service, test the webhook server:
```bash
webhook -hooks /home/fantaz/scripts/webhook/hooks.json -port 9002 -verbose
```
- Access `http://localhost:9002/hooks/redeploy-webhook` to trigger your hook locally.
- Check the terminal logs for execution output.

## 4. Run Webhook as a Systemd Service
Create a systemd service file to run webhook on boot:
```bash
sudo nano /etc/systemd/system/webhook.service
```
**Example** `webhook.service`
```ini
[Unit]
Description=Webhook Server
After=network.target

[Service]
ExecStart=/usr/bin/webhook -hooks /home/fantaz/scripts/webhook/hooks.json -port 9002 -verbose
Restart=always
RestartSec=5
User=fantaz
Group=fantaz

[Install]
WantedBy=multi-user.target
```
Save and exit. Then enable and start the service:
```bash
sudo systemctl daemon-reload
sudo systemctl enable webhook.service
sudo systemctl start webhook.service
sudo systemctl status webhook.service
```

## 5. Triggering the Webhook
Once the service is running, you can trigger your webhook remotely using curl or any HTTP client:
```bash
curl -X POST http://your-server-ip:9002/hooks/redeploy-webhook
```
- Make sure any firewall allows access to port 9002 if triggering remotely.
- Consider securing the webhook with a secret token for production environments.

## 6. Additional Tips
- Logs can be viewed using `journalctl -u webhook.service -f`.
- Keep your scripts in a dedicated folder and ensure proper permissions.
- Test hooks locally before exposing them to the network.