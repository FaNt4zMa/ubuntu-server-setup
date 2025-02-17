# Immich logging to file
1. Start by using journald logging for immich container
    ```yml
    services:
        immich-server:
            logging:
                driver: "journald"
                options:
                    tag: "immich-server"
    ```

2. Make the directory where you want to store the log file
    ```bash
    mkdir /your/path/of/choice/logs
    ```
3. Create a service to forward journald logs to a log file
    ```bash
    sudo nano /etc/systemd/system/immich-log-forward.service
    ```
    For the service, paste the following. Make sure to replace {user} & {group}
    ```bash
    [Unit]
    Description=Forward Immich logs to a file
    After=network.target

    [Service]
    ExecStart=/bin/bash -c 'journalctl -t immich-server -f >> /your/path/of/choice/logs/immich.log 2>&1'
    Restart=always
    User={user}
    Group={group}

    [Install]
    WantedBy=multi-user.target
    ```
4. Reload systemctl daemon and enable the service
    ```
    sudo systemctl daemon-reload
    sudo systemctl enable --now immich-log-forward
    ```
    To check the service status:
    ```bash
    systemctl status immich-log-forward
    ```