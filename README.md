# arch-install

Boot the arch install media then
```
wget "https://raw.githubusercontent.com/jplikesbikes/arch-install/master/base-install"
chmod +x base-install
./base-install
```

## Resizing a partition
```
lvresize --resizefs -L +10G MyStorage/varvol
```

## Clean the systemd journals
Keeps the last week of logs
```
journalctl --vacuum-time=7d
```
