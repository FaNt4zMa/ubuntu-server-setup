# ubuntu-install-steps
How to install/config ubuntu for homelab use.

Enable virtualization in BIOS settings.
Enable WoL.

When installing Ubuntu, during storage configuration, disable LVM group.

On first boot, do:
```bash
sudo apt update
sudo apt upgrade
sudo apt install neofetch htop -y
```
Assign a static IP in router settings.

Laptop Tip: Disable Suspend on Lid Close.
```bash
sudo nano /etc/systemd/logind.conf
```
Uncomment the following keys and change it’s values to ignore:
```bash
HandleSuspendKey=ignore
HandleLidSwitch=ignore
HandleLidSwitchDocked=ignore
```
Then:
```bash
sudo systemctl restart systemd-logind
```