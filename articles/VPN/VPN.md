## VPN

First think first: **VPN is not designed to be anonymisation technology**. It has multiple issues. Use 

### NordVPN
*Chipest and probably shittiest.*

1. CLI client for Linux is shit. It breaks internet connection after wakeup from hibernate.
2. Better is to use [OpenVPN file](https://nordvpn.com/ovpn/) instead
3. But it **LEAKS** IPV6! So you need to [disable it](https://www.techrepublic.com/article/how-to-disable-ipv6-through-grub-in-linux/).
   ```bash
   sudo nano /etc/default/grub
   ```
   Make those lines looks like this
   ```bash
   GRUB_CMDLINE_LINUX_DEFAULT="ipv6.disable=1 quiet splash"
   GRUB_CMDLINE_LINUX="ipv6.disable=1"
   ```
   Run `sudo update-grub` and **reboot**.
