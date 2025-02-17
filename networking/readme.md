# Link speed check/configuration
1. Install ethtool
    ```bash
    sudo apt install ethtool
    ```
2. Check NIC details and properties
    ```bash
    ethtool <interface>
    ```
    To check for speed specifically, run
    ```bash
    sudo ethtool <interface> | grep Speed
    ```
3. If link speed is not correct, set using
    ```bash
    sudo ethtool -s [device_name] autoneg [on/off] speed [10/100/1000] duplex [half/full]
    sudo ethtool -s enp0s3 autoneg on speed 1000 duplex full
    ```