# Link speed check/configuration

Follow these steps to check and configure the link speed of your network interface.

## 1. Install ethtool  
To install ethtool (a utility for network interface configuration):
```bash
sudo apt install ethtool
```
## 2. Check NIC Details and Properties  
Check the details of your network interface (replace <interface> with the actual interface name, e.g., eth0, enp0s3):
```bash
ethtool <interface>
```
To specifically check the link speed, run:
```bash
sudo ethtool <interface> | grep Speed
```
## 3. Set Link Speed (If Incorrect)  
If the link speed is not as expected, you can set it manually using the following command. Replace [device_name] with your actual interface name and adjust autoneg, speed, and duplex as needed:
```bash
sudo ethtool -s [device_name] autoneg [on/off] speed [10/100/1000] duplex [half/full]
```
For example, to set the speed to 1000 Mbps (1 Gbps) and full duplex on interface enp0s3:
```bash
sudo ethtool -s enp0s3 autoneg on speed 1000 duplex full
```
### Note:
* `autoneg` **(auto-negotiation)** should generally be enabled (`on`) for most modern devices to automatically negotiate the best speed and duplex setting.
* Changes made with `ethtool` are not persistent across reboots. If you want the configuration to persist, you’ll need to add it to your network interface configuration file (e.g., `/etc/network/interfaces` or use `Netplan` depending on your Ubuntu version).