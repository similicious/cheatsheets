## Mount a samba share at boot
Edit `/etc/fstab` with root previliges and add the following line. Apply changes with a reboot or `sudo mount -a`.
```
//192.168.178.1/FRITZ.NAS/Media/Media /media/similicious/Media cifs _netdev,rw,user=<<user>>,password=<<password>>,noserverino,uid=1000,gid=1000 0 0
```

- `cifs` is the fuse driver, which is used to connect to the samba share
- `_netdev` marks the mount point as a network share, mounting only when network is available
- `rw` marks the mount point read- and -writable
- `noserverino` fixes issues with jellyfin / ffmpeg [source](https://askubuntu.com/a/1265165)
- `uid=1000` and `gid=1000` mount the share for the main user

