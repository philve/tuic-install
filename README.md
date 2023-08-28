# [TUIC](https://github.com/EAimTY/tuic) Installation Guide

## Server

### Installation

1. Download program (linux-amd64)

```
curl -Lo /root/tuic https://github.com/EAimTY/tuic/releases/download/tuic-server-1.0.0/tuic-server-1.0.0-x86_64-unknown-linux-gnu && chmod +x /root/tuic && mv -f /root/tuic /usr/local/bin
```

2. Download the configuration

```
curl -Lo /root/tuic_config.json https://raw.githubusercontent.com/chika0801/tuic-install/main/config_server.json
```

3. Download the systemctl configuration

```
curl -Lo /etc/systemd/system/tuic.service https://raw.githubusercontent.com/chika0801/tuic-install/main/tuic.service && systemctl daemon-reload
```

4. Upload the certificate and private key

- Rename the certificate file to fullchain.cer and the private key file to private.key and upload them to the /root directory

5. Start the program

```
systemctl enable --now tuic
```

| project | |
| :--- | :--- |
| procedure | **/usr/local/bin/tuic** |
| disposition | **/root/tuic_config.json** |
| Reboot | `systemctl restart tuic` |
| state | `systemctl status tuic` |
| Review the logs | `journalctl -u tuic -o cat -e` |
| Real-time logs | `journalctl -u tuic -o cat -f` |

### unload

```
systemctl disable --now tuic && rm -f /usr/local/bin/tuic /root/tuic_config.json /etc/systemd/system/tuic.service
```

## client

### HTTP SOCKS2 proxy is provided by v5rayN and routing rules are provided by v2rayN

1. Download the Windows client program [tuic-client-1.0.0-x86_64-pc-windows-gnu.exe](https://github.com/EAimTY/tuic/releases/download/tuic-client-1.0.0/tuic-client-1.0.0-x86_64-pc-windows-gnu.exe)，rename it to tuic.exe, and copy it to the v2rayN\bin\tuic folder.

2. Download the client configuration [config_client.json](https://github.com/chika0801/tuic-install/blob/main/config_client.json)，change the chika.example.com to the domain name contained in the certificate, and change 10.0.0.1 to the IP address of the VPS.

3. v2rayN: Server - > Add custom configuration server - > Browse - > Select client configuration - > Core type tuic - > Socks port 50000

![TUIC](https://github.com/chika0801/tuic-install/assets/88967758/00bcbfd2-e24d-4187-aeb9-e2afefab219d)

### Tun mode (transparent proxy) is provided by sing-box, and routing rules are provided by sing-box

1. sing-box: Refer to [the Windows usage method](https://github.com/chika0801/sing-box-examples/blob/main/README.md)， to modify [the client configuration](https://github.com/chika0801/sing-box-examples/blob/main/Tun/config_client_windows.json) as follows.

Original content
```jsonc
        {
            "tag": "proxy",
            // 粘贴你的客户端配置，需要保留 "tag": "proxy",
        },
```

to be replaced with
```jsonc
        {
            "type": "socks",
            "tag": "proxy",
            "server": "127.0.0.1",
            "server_port": 50000
        },
```

2. v2rayN: Server - > Add a custom configuration server - > Browse - > Select Client Configuration - > Core type tuic - > Socks port 0.
