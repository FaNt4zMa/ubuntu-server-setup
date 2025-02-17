# Ubuntu Installation & Configuration for Homelab

This guide covers installing and configuring Ubuntu for homelab use.

## BIOS Setup
Before installing Ubuntu, enable the following settings in your BIOS:
- **Virtualization** (Required for running virtual machines and containers).
- **Wake-on-LAN (WoL)** (Allows remote power-on of the machine).

## Ubuntu Installation
When installing Ubuntu, during the **storage configuration step**, disable **LVM group** to keep a simpler partition layout.

## First Boot Configuration
After the first boot, update the system and install some essential utilities:
```bash
sudo apt update
sudo apt upgrade -y
sudo apt autoremove -y
sudo apt install neofetch htop -y
```
## Assign a Static IP
Set a static IP for your machine in your router's DHCP settings.

## Laptop Tip: Disable Suspend on Lid Close.
To prevent the laptop from suspending when the lid is closed:
1. Open the logind.conf configuration file:
```bash
sudo nano /etc/systemd/logind.conf
```
2. Uncomment the following keys and change its values to ignore:
```bash
HandleSuspendKey=ignore
HandleLidSwitch=ignore
HandleLidSwitchDocked=ignore
```
3. Apply the changes:
```bash
sudo systemctl restart systemd-logind
```