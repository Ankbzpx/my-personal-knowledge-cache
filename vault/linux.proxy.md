---
id: ItcRFwyzBzay5dNU8Y7Bl
title: Proxy
desc: ''
updated: 1642931007558
created: 1642930600402
---
## Git
```
git config --global http.proxy http://127.0.0.1:7890
...
git config --global --unset http.proxy
```

## Pacman
```
export http_proxy="http://127.0.0.1:7890"
sudo -E pacman -Syu
```

## Pip
```
pip install --proxy=http://127.0.0.1:7890 numpy
```

## Svn
Add
```
[global]
http-proxy-host = 127.0.0.1
http-proxy-port = 7890
http-proxy-compression = no
```
to `~/.subversion/servers`