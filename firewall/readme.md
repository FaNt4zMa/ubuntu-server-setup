# UFW configuration
1. Enable ufw
    ```bash
    sudo ufw enable
    ```
2. Enable on boot
    ```bash        
    sudo systemctl enable ufw
    ```
## Basic syntax to add/remove rules (https://help.ubuntu.com/community/UFW):
```bash
sudo ufw allow <port>/<optional:protocol>
    sudo ufw allow 53
    sudo ufw allow 53/tcp

sudo ufw deny <port>/<optional:protocol>
    sudo ufw deny 53
    sudo ufw deny 53/udp

sudo ufw delete <rule-type> <port>/<optional:protocol>
    sudo ufw delete allow 53
    sudo ufw delete allow 53/tcp

sudo ufw allow from <ip address>
    sudo ufw allow from 192.168.2.55

ufw allow [port-range]/protocol
    sudo ufw allow 8070:8100/tcp
```
Reload UFW after modifying rules
```bash
sudo ufw reload
```