
## Kernel parameters
`sudo nano /etc/default/grub`

Then append on

`GRUB_CMDLINE_LINUX_DEFAULT="quiet splash"` line

## Regenerate boot menu
```
sudo grub-mkconfig -o /boot/grub/grub.cfg
``` 
