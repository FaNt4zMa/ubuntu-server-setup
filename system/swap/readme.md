# Swap Configuration

This guide covers how to disable, enable, resize, and configure swap files on Ubuntu.

## Checking Swap Status
Before making any changes, check if swap is currently enabled.
### View Active Swap
```bash
swapon --show
```
or
```bash
free -h
```
If you see active swap devices, proceed to disable them.
## Disabling Swap
Disabling the swap file can improve system performance when running memory-intensive applications.

### 1. Turn Off Swap Temporarily
```bash
sudo swapoff -a
```
**Note:** The `-a` option disables all active swap devices. However, this change is not persistent and will revert after a reboot.
### 2. Remove Swap Permanently (Optional)
If you want to permanently remove swap, edit `/etc/fstab` and remove any swap entries:
```bash
sudo nano /etc/fstab
```
Find and delete this line (if present):
```bash
/swapfile none swap sw 0 0
```
Then, delete the swap file:
```bash
sudo rm -f /swapfile
```
## Enabling or Creating a Swap File
If your system needs swap, follow these steps to create or enable it.
### 1. Create a Swap File
If a swap file does not exist, create one using `fallocate` (preferred) or `dd` (slower method):
```bash
# Using fallocate (faster)
sudo fallocate -l 16G /swapfile

# Using dd (slower)
sudo dd if=/dev/zero of=/swapfile bs=1M count=16384
```
### 2. Set the Correct Permissions
```bash
sudo chmod 600 /swapfile
sudo chown root:root /swapfile
```
### 3. Mark as Swap Space
```bash
sudo mkswap /swapfile
```
### 4. Enable Swap
```bash
sudo swapon /swapfile
```
Verify that swap is now active:
```bash
swapon --show
```
### 5. Make Swap Persistent After Reboot
To ensure swap is available after reboot, add it to `/etc/fstab`:
```bash
echo '/swapfile none swap sw 0 0' | sudo tee -a /etc/fstab
```
## Modifying Swap Size (Increase or Decrease)
If you need to change the swap size, follow these steps:
### 1. Disable the Current Swap
```bash
sudo swapoff /swapfile
```
### 2. Resize the Swap File
To **increase** swap to 20GB (example), use:
```bash
sudo fallocate -l 20G /swapfile
```
or (if `fallocate` fails):
```bash
sudo dd if=/dev/zero of=/swapfile bs=1M count=20480
```
### 3. Reset Permissions and Reinitialize Swap
```bash
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile
```
### 4. Verify the Changes
```bash
free -h
```
## Viewing and Managing Swap
Check swap usage with:
```bash
free -h
swapon --show
```
## Important Notes
* **Disabling swap** can improve performance, but it may lead to **Out of Memory (OOM) errors** if physical RAM is exhausted.
* **Resizing swap** requires disabling and recreating it.
* **Persistent swap requires** `/etc/fstab` **changes**, or it will not survive reboots.