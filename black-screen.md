### Desktop starts up, but all you see is a black screen

Write thoose line to `/etc/xrdp/startwm.sh` before line `if test ...`

```bash
unset DBUS_SESSION_BUS_ADDRESS
unset XDG_RUNTIME_DIR
```