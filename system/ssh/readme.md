# Add user to sudoers
When running a command with sudo remotely via SSH, it might prompt for a password, which can cause issues with automation scripts.

To allow a user to run `sudo` commands without a password prompt, follow these steps:

## 1. Edit the sudoers file  
Run the following command to safely edit the sudoers file:
```bash
sudo visudo
``` 
## 2. Add a sudoers entry  
Add the following line at the end of the file, replacing <username> with your actual username:
```bash
<username> ALL=(ALL) NOPASSWD: ALL
```
## 3. Save and exit

Now, the specified user can run sudo commands without being prompted for a password.