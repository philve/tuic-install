## [TUIC](https://github.com/EAimTY/tuic)安装指南

1.下载程序
```
curl -Lo /root/tuic https://github.com/EAimTY/tuic/releases/download/0.8.4/tuic-server-0.8.4-x86_64-linux-musl && chmod +x /root/tuic
```

2.下载配置
```
curl -Lo /root/tuic_config.json https://raw.githubusercontent.com/chika0801/tuic-install/main/config_server.json
```

3.下载systemctl配置文件
```
curl -Lo /etc/systemd/system/tuic.service https://raw.githubusercontent.com/chika0801/tuic-install/main/tuic.service
```

4.上传证书和私钥

将证书文件改名为`fullchain.cer`，将私钥文件改名为`private.key`，使用WinSCP登录你的VPS，将它们上传到`/root/`目录。

[用acme申请 SSL 证书](https://github.com/chika0801/Xray-install#1%E7%94%A8acme%E7%94%B3%E8%AF%B7-ssl-%E8%AF%81%E4%B9%A6)

[什么是 SSL 证书](https://www.kaspersky.com.cn/resource-center/definitions/what-is-a-ssl-certificate)

5.启动程序
```
systemctl daemon-reload && systemctl enable tuic
```

```
systemctl start tuic
```

```
systemctl status tuic
```

- 程序 `/root/tuic`
- 配置 `/root/tuic_config.json`
- 证书 `/root/fullchain.cer`
- 私钥 `/root/private.key`
- systemctl配置文件 `/etc/systemd/system/tuic.service`
- 查看日志 `journalctl -u tuic --output cat -e`
- 实时日志 `journalctl -u tuic --output cat -f`

## v2rayN配置指南

下载Windows客户端程序[tuic-client-0.8.4-x86_64-windows-msvc.exe](https://github.com/EAimTY/tuic/releases/download/0.8.4/tuic-client-0.8.4-x86_64-windows-msvc.exe)，重命令为`tuic.exe`，复制到v2rayN文件夹。

下载客户端配置[config_client.json](https://github.com/chika0801/tuic-install/blob/main/config_client.json)，修改`chika.example.com`为证书中包含的域名，修改`1.1.1.1`为VPS的IP。

小技巧：只要证书在有效期内，证书中包含的域名可不用解析到VPS的IP。`即一份证书，在多个VPS上使用`。

![2](https://user-images.githubusercontent.com/88967758/195763590-f035f90f-f228-4022-b318-770791c63b92.jpg)

## 测速对比

Windows 11，Chrome -> Proxy SwitchyOmega(HTTP) -> v2rayN -> VPS

使用[speedtest.net](https://www.speedtest.net)网页版，多线程，以结束时显示的值为结果，测3次，取平均值。（香港测试点ID 22126，圣何塞测试点ID 49365，大阪测试点ID 21569，东京测试点ID 21569）

- 香港（CMI回程 1核 1G 15G SSD 500Mbps带宽）
  - Trojan-TCP-TLS 461Mbps
  - hysteria 486Mbps（客户端配置下行500Mbps）
  - TUIC 354Mbps

- 圣何塞（9929回程 2核 1G 20G SSD 1Gbps带宽）
  - Trojan-TCP-TLS 482Mbps
  - hysteria 595Mbps（客户端配置下行650Mbps）
  - TUIC 262Mbps

- 大阪（BBTEC回程 1核 512M 10G SSD 2.5Gbps带宽）
  - Trojan-TCP-TLS 505Mbps
  - hysteria 255Mbps（客户端配置下行300Mbps）
  - TUIC 570Mbps

- 东京（IIJ回程 2核 2.5G 50G SSD 1Gbps带宽）
  - Trojan-TCP-TLS 707Mbps
  - hysteria 671Mbps（客户端配置下行800Mbps）
  - TUIC 425Mbps

- 圣何塞（GTT回程 1核 2G 20G SSD 10Gbps带宽）
  - Trojan-TCP-TLS 113Mbps
  - hysteria 439Mbps（客户端配置下行500Mbps）
  - TUIC 210Mbps

使用[youtube.com](https://www.youtube.com/watch?v=I3o4WW4tD9M)播放4K视频，观看1分30秒，以稳定时的值为结果。

- 香港
  - Trojan-TCP-TLS 150000Kbps
  - hysteria 180000Kbps
  - TUIC 70000Kbps

- 圣何塞
  - Trojan-TCP-TLS 110000Kbps
  - hysteria 180000Kbps
  - TUIC 50000Kbps

- 大阪
  - Trojan-TCP-TLS 90000Kbps
  - hysteria 140000Kbps
  - TUIC 80000Kbps

- 东京
  - Trojan-TCP-TLS 90000Kbps
  - hysteria 160000Kbps
  - TUIC 50000Kbps

- 圣何塞
  - Trojan-TCP-TLS 10000Kbps
  - hysteria 90000Kbps
  - TUIC 40000Kbps
