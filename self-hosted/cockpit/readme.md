# Cockpit Install

Follow these steps to install and configure Cockpit on your system.

## 1. Install Cockpit  
To install Cockpit, run the following command:
```bash
sudo apt install cockpit
```
## 2. Enable and Start Cockpit  
To start Cockpit and enable it to run on boot, use these commands:
```bash
sudo systemctl start cockpit
sudo systemctl enable cockpit.socket
```
## 3. Allow Cockpit in UFW  
If you're using UFW, allow traffic on port 9090 (the default port for Cockpit):
```bash
sudo ufw allow 9090
```