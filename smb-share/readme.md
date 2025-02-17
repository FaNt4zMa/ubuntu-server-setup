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
```bash
[share_name]
path = /home/username/sambashare
read only = no
browsable = yes
```
## 6. Restart Samba Service  
Restart Samba to apply the changes:
```bash
sudo service smbd restart
```
## 7. Allow Samba Traffic in UFW  
If you’re using UFW, allow Samba traffic:
```bash
sudo ufw allow samba
```