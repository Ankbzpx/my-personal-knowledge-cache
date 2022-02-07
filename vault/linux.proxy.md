---
id: ItcRFwyzBzay5dNU8Y7Bl
title: Proxy
desc: ''
updated: 1643530147967
created: 1642930600402
---
In general, most application respect `HTTP_PROXY` or `ALL_PROXY` environment variable
```
export HTTP_PROXY=http://127.0.0.1:7890
export ALL_PROXY=http://127.0.0.1:7890
```

## Git
### HTTP/ HTTPS
```
git config --global http.proxy http://127.0.0.1:7890
...
git config --global --unset http.proxy
```

### SSH
Install `openbsd-netcat` then `nano .ssh/config`

```
Host github.com bitbucket.org
    ProxyCommand            nc -X 5 -x 127.0.0.1:7891 %h %p
```

## Pacman
```
export http_proxy="http://127.0.0.1:7890"
export https_proxy="http://127.0.0.1:7890"
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

## Wget

Add
```
use_proxy = on
http_proxy = http://127.0.0.1:7890/
https_proxy = http://127.0.0.1:7890/
```
to `/etc/wgetrc` or `~/.wgetrc`

## Snap
```
sudo snap set system proxy.http="http://127.0.0.1:7890"
sudo snap set system proxy.https="http://127.0.0.1:7890"

snap unset system proxy.http
snap unset system proxy.https
```