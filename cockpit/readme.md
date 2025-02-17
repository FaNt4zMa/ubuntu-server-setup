# Cockpit install
1. Install Cockpit
    ```bash
    sudo apt install cockpit
    ```

2. Enable/check status
    ```bash
    sudo systemctl start cockpit
    sudo systemctl enable cockpit.socket
    ```

3. Allow Cockpit in UFW
    ```bash
    sudo ufw allow 9090
    ```