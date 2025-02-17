# UFW 

Follow these steps to configure UFW (Uncomplicated Firewall) on your system:

## 1. Enable ufw 
Run the following command to enable the firewall:
```bash
sudo ufw enable
```
## 2. Enable on boot  
To ensure UFW starts automatically on boot, run:
```bash        
sudo systemctl enable ufw
```
## 3. Basic Syntax to Add/Remove Rules  
Here are examples of how to add or remove rules:
```bash
# Allow a port (TCP by default)
sudo ufw allow <port>

# Allow a specific protocol (TCP/UDP)
sudo ufw allow <port>/<protocol>

# Deny a port
sudo ufw deny <port>

# Deny a specific protocol
sudo ufw deny <port>/<protocol>

# Delete a rule
sudo ufw delete allow <port>
sudo ufw delete allow <port>/<protocol>

# Allow traffic from a specific IP address
sudo ufw allow from <ip address>

# Allow a port range
sudo ufw allow <port-range>/protocol
```
### Example Commands:
```bash
# Allow port 53 (DNS)
sudo ufw allow 53
sudo ufw allow 53/tcp

# Deny port 53 (DNS)
sudo ufw deny 53
sudo ufw deny 53/udp

# Allow traffic from a specific IP
sudo ufw allow from 192.168.2.55

# Allow a range of ports
sudo ufw allow 8070:8100/tcp
```
## 4. Reload UFW  
After modifying the rules, reload UFW to apply the changes:
```bash
sudo ufw reload
```