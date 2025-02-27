# Install and Configure Automatic Updates

Follow these steps to enable and configure automatic updates on your system:

## 1. Install Unattended Upgrades
Install the `unattended-upgrades` package, which handles automatic updates:
```bash
sudo apt install unattended-upgrades -y
```

## 2. Configure Unattended Upgrades
Run the following command to configure the package and enable automatic updates:
```bash
sudo dpkg-reconfigure --priority=low unattended-upgrades
```

## 3. Customize Update Settings (Optional)
To configure which updates should be automatically installed, edit the configuration file:
```bash
sudo nano /etc/apt/apt.conf.d/50unattended-upgrades
```
In this file, you can enable or disable automatic updates for specific package sources (e.g., security updates, stable updates, etc.). Here’s an example of how you might enable security updates:
```bash
Unattended-Upgrade::Allowed-Origins {
        "Ubuntu::Focal-security";
        // "Ubuntu::Focal-updates";
};
```

## 4. Enable Automatic Updates on Boot (Optional)
If you want your system to check for updates at boot, you can enable the `apt-daily-upgrade.timer`:
```bash
sudo systemctl enable apt-daily-upgrade.timer
```
