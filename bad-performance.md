
### Increase socket buffer sizes

[XRDP Source code](https://github.com/neutrinolabs/xrdp)
Linux sends any data over the network using sockets. Xrdp, as well, is mostly written is C and its config has two lines of code,
which are 100 percent refers to two socket methods [send](https://man7.org/linux/man-pages/man2/send.2.html) and [recv](https://man7.org/linux/man-pages/man2/recv.2.html).

And thoose are written to config:
```ini
[Globals]
#tcp_send_buffer_bytes=32768
#tcp_recv_buffer_bytes=32768
tcp_nodelay=true
tcp_keepalive=true
```

It seems like xrdp uses this values as a default. For sending such amount of data among the internet this values seems to be too small,
even due to the fact that linux [doubles buffers sizes](https://man7.org/linux/man-pages/man7/tcp.7.html).

Set thoose values to something more valuable, like `4194304`.
Also, do not forget to increase linux global socket buffers limit itself to value, which equals doubled buffer size;

```bash
# for example, for 4194304
sudo sysctl -w net.core.wmem_max=8388608
```

But, setting this values to something really big, like a few Mb will not increase your performace.
Due to internet connection speed and `send-rate` buffers will not be able to be sended in time and will cause
undefined bahavior.


### Decrease crypt level

Send config `crypt_level=none` or `crypt_level=low`
This will descrease encryption level, but increase your performance.


### Remove desktop image

Yeah, i know how that sounds, but all remote desktops, i'm 100 percent sure, does not minds about sending only rerendered parts
of a desktop and maybe, removing wallpaper shoud also help to solve performance issue

This commands will remove desktop wallpaper and set it to `#FFFFFF` color as well. Change this value to set it to a different color

```bash
gsettings set org.gnome.desktop.background picture-uri ""
gsettings set org.gnome.desktop.background picture-uri-dark ""
gsettings set org.gnome.desktop.background primary-color '#FFFFFF'
```