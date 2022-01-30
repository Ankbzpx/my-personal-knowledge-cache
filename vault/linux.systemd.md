---
id: TDUWGiakpfoj6NbBzsgkB
title: Systemd
desc: ''
updated: 1643511040382
created: 1643510790270
---

## Basic

- Enable a unit to start automatically at boot: `systemctl enable unit`
- Enable a unit to start automatically at boot and start it immediately: `systemctl enable --now unit`
- Disable a unit to no longer start at boot: `systemctl disable unit`


- Start a unit immediately: `systemctl start unit`
- Stop a unit immediately: `systemctl stop unit`
- Restart a unit: `systemctl restart unit`
- Reload a unit and its configuration: `systemctl reload unit`

## Custom unit
> /etc/systemd/system/clash.service

```
[Unit]
Description=Clash daemon, A rule-based proxy in Go.
After=network.target

[Service]
Type=simple
Restart=always
ExecStart=/usr/bin/clash -f /etc/clash/config.yaml

[Install]
WantedBy=multi-user.target
```
