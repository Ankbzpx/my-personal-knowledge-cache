---
id: yD5cqh9OP4cNXK3BC98f6
title: Grub
desc: ''
updated: 1644200486846
created: 1642930537952
---

## Kernel parameters
`sudo nano /etc/default/grub`

Then append on

`GRUB_CMDLINE_LINUX_DEFAULT="quiet splash"` line

## Regenerate boot menu
```
sudo grub-mkconfig -o /boot/grub/grub.cfg
``` 
