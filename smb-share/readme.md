# SMB Share configuration
1. Install Samba
    ```bash
    sudo apt install samba
    ```
2. Make share directory
    ```bash
    mkdir /home/<username>/sambashare/
    ```
3. Edit samba configuration
    ```bash
    sudo nano /etc/samba/smb.conf
    ```
4. Add directory as a share
    ```bash
    [share_name]
    path = /home/username/sambashare
    read only = no
    browsable = yes
    ```
5. Restart Samba
    ```bash
    sudo service smbd restart
    ```
6. Allow Samba traffic in UFW
    ```bash
    sudo ufw allow samba
    ```