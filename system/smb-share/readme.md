# SMB Share configuration

Follow these steps to configure a Samba share on Ubuntu:

## 1. Install Samba  
First, install Samba with the following command:
```bash
sudo apt install samba
```
## 2. Create the Share Directory  
Create the directory where the shared files will be stored. Replace <username> with your actual username:
```bash
mkdir /home/<username>/sambashare/
```
## 3. Set Directory Permissions (Optional but Recommended)  
Ensure proper access to the directory:
```bash
sudo chown <username>:<username> /home/<username>/sambashare
sudo chmod 775 /home/<username>/sambashare
```
## 4. Edit the Samba Configuration File  
Open the Samba configuration file for editing:
```bash
sudo nano /etc/samba/smb.conf
```
## 5. Add the Share Directory to the Samba Configuration  
At the end of the file, add the following section to create the share. Replace <username> with your actual username:
```ini
[share_name]
path = /home/username/sambashare
read only = no
browsable = yes
```
## 6. Set Up Samba User Accounts  
Since Samba doesn’t use the system account password, you need to set up a Samba password for your user account:
```bash
sudo smbpasswd -a <username>
```
## 7. Restart Samba Service  
Restart Samba to apply the changes:
```bash
sudo service smbd restart
```
## 8. Allow Samba Traffic in UFW  
If you’re using UFW, allow Samba traffic:
```bash
sudo ufw allow samba
```
## 9. Connecting to the Share 
### On Ubuntu:  
Open the default file manager and click Connect to Server then enter:   
```bash
smb://ip-address/share_name
```
### On macOS:  
In the Finder menu, click Go > Connect to Server then enter:
```bash
smb://ip-address/share_name
```
### On Windows:  
Open up File Manager and edit the file path to:
```bash
\\ip-address\share_name
```
**Note:** `ip-address` is the Samba server IP address and `share_name` is the name of the share. You’ll be prompted for your credentials. Enter them to connect!