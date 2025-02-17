# Harden SSH security
1. Make sure user is part of sudoers
    ```bash
    usermod -aG sudo <username>
    ```
2. Edit sshd_config file
    ```bash
    sudo nano /etc/ssh/sshd_config
    ```
 3. Uncomment the following keys and change it’s values:
    ```bash
    Port 22                         -> <port>
    AddressFamily any               -> inet
    PermitRootLogin yes             -> no
    PasswordAuthentification yes    -> no
    ```
4. Save and restart SSH service
    ```bash
    sudo systemctl restart sshd
    ```
5. Allow new SSH port in UFW
    ```bash
    sudo ufw allow <port>
    ```