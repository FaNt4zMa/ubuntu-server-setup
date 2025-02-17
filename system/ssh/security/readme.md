# Harden SSH security

Follow these steps to improve SSH security:

## 1. Ensure the User is in the Sudoers Group  
Run the following command, replacing `<username>` with your actual username:
```bash
usermod -aG sudo <username>
```
## 2. Edit the SSH Configuration File 
Open the SSH daemon configuration file:
```bash
sudo nano /etc/ssh/sshd_config
```
## 3. Modify the Following Settings  
Find and update these lines in the file:
```bash
Port <port>                 # Change SSH port (avoid common ports like 22)
AddressFamily inet          # Restrict to IPv4 only
PermitRootLogin no          # Disable root login
PasswordAuthentication no   # Disable password authentication (use SSH keys)
```
## 4. Save and Restart the SSH Service  
After making the changes, restart SSH:
```bash
sudo systemctl restart sshd
```
⚠ Important: Before closing your current SSH session, test the new settings by opening a new terminal and connecting with the updated port. If something goes wrong, you won't be locked out.
## 5. Allow the New SSH Port in UFW  
If using a firewall, allow the new SSH port:
```bash
sudo ufw allow <port>
```