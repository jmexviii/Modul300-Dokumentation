# Festplatten formatieren
```Bash
root@vmLS3:~# apt-get update
root@vmLS3:~# apt-get install lshw
```
## Disks herauslesen
```Bash
root@vmLS3:~# lshw –class disk
```
## Partintionieren
```Bash
root@vmLS3:~# fdisk /dev/sdb 
Command (m for help): n
Command action
 e exdended
 p primary partition (1-4)
p
Partition number (1-4): 1
First cylinder (1-130, default 1) : 1
Last cylinder, +cylinder or +size{K,M,G} (1-130, default 130): 130
Command (m for help): w
```
## Formatieren
```BASH
root@vmLS3:~# mkfs.ext4 /dev/sd
```
## Mountverzeichnis erstellen
```BASH
root@vmLS3:~# mkdir /www
```
## /etc/fstab ergänzen, damit beim start gemountet
```BASH
/dev/sdb1 /www ext4 defaults 1 2
```
## neue Disk mit  mount –a einhängen. Mit mount ohne Parameter kontrollieren Sie ob die neue Platte im Verzeichnis /www eingehängt ist

```BASH
root@vmLS3:~# mount –a
root@vmLS3:~# mount 
..
fusectl on /sys/fs/fuse/connections type fusectl (rw)
/dev/sdb1 on /www type ext3 (rw)
```

