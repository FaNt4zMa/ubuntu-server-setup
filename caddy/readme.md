# Caddy Service Install/Configuration

Follow these steps to install and configure Caddy as a service on your system.

## 1. Download and Build Caddy  
Download and build Caddy from the [official website](https://caddyserver.com/download?package=github.com%2Fcaddy-dns%2Fduckdns&package=github.com%2Fcaddy-dns%2Fcloudflare).  
* Recommended plugins: DuckDNS, Cloudflare, transform-encoder.

## 2. Rename Binary to Caddy  
Once the download is complete, rename the binary to `caddy`:
```bash
sudo mv caddy_linux_amd64_custom ./caddy
```

## 3. Give Read, Write, Execute Permission to the Binary  
Allow the necessary permissions for the Caddy binary:
```bash
sudo chmod +x caddy
```

## 4. Copy Caddy Binary to $PATH  
Move the Caddy binary to the `/usr/bin/` directory:
```bash
sudo mv caddy /usr/bin/
```

## 5. Test Caddy  
Verify the installation by checking the version of Caddy:
```bash
caddy version
```

## 6. Create Caddy Group  
Create a system group for Caddy:
```bash
sudo groupadd --system caddy
```

## 7. Create a User Named Caddy with a Writable Home Directory  
Create a user for Caddy with a writable home directory:
```bash
sudo useradd --system \
--gid caddy \
--create-home \
--home-dir /var/lib/caddy \
--shell /usr/sbin/nologin \
--comment "Caddy web server" \
caddy
```
If using a config file, be sure it is readable by the Caddy user you just created.

## 8. Create the Caddy Service  
Create a systemd service for Caddy:
```bash
cd /etc/systemd/service
sudo nano caddy.service
```
Paste the following configuration into the file:
```bash
# caddy.service
#
# For using Caddy with a config file.
#
# Make sure the ExecStart and ExecReload commands are correct
# for your installation.
#
# See https://caddyserver.com/docs/install for instructions.
#
# WARNING: This service does not use the --resume flag, so if you
# use the API to make changes, they will be overwritten by the
# Caddyfile next time the service is restarted. If you intend to
# use Caddy's API to configure it, add the --resume flag to the
# `caddy run` command or use the caddy-api.service file instead.

[Unit]
Description=Caddy
Documentation=https://caddyserver.com/docs/
After=network.target network-online.target
Requires=network-online.target

[Service]
Type=notify
User=caddy
Group=caddy
ExecStart=/usr/bin/caddy run --environ --config /etc/caddy/Caddyfile
ExecReload=/usr/bin/caddy reload --config /etc/caddy/Caddyfile --force
TimeoutStopSec=5s
LimitNOFILE=1048576
PrivateTmp=true
ProtectSystem=full
AmbientCapabilities=CAP_NET_ADMIN CAP_NET_BIND_SERVICE
# Automatically restart caddy if it crashes except if the exit code was 1
RestartPreventExitStatus=1
Restart=on-failure
RestartSec=5s

[Install]
WantedBy=multi-user.target
```
Save & exit.

## 9. Create the Caddyfile and Configure It  
Create the Caddyfile and configure your reverse proxy settings:
```bash
cd /etc
sudo mkdir caddy
cd /etc/caddy
sudo nano Caddyfile
```
Here’s an example Caddyfile:
```
sub.domain.com {
    reverse_proxy 127.0.0.1:8096
    tls {
        dns cloudflare cloudflare-api
    }
}

subdomain.duckdns.org {
        reverse_proxy 192.168.2.98:8096
        tls {
                dns duckdns <token>
                resolvers 8.8.8.8 9.9.9.9 1.1.1.1
        }
}
```

## 10. Add Logs to Caddy  
To add logging to Caddy, use the `transform-encoder` plugin to format the logs into Apache/Nginx style.

Add the following to your Caddyfile, replacing `/your/path/of/choice` with your desired log directory:
```
{
        log {
                format transform "{common_log}" {
                        time_local
                }
                output file /your/path/of/choice/logs/access.log {
                        mode 644
                        roll_size 50MiB
                        roll_keep 20
                }
        }
}
```