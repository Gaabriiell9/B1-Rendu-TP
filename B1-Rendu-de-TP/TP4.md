# Compte rendu 4


### 🌞 Lister les périphériques de stockage branchés à la machine
    
```lsblk```
```bash
NAME                  MAJ:MIN RM  SIZE RO TYPE MOUNTPOINTS  
sr0                    11:0    1 1024M  0 rom    
vda                   254:0    0   32G  0 disk   
├─vda1                254:1    0  512M  0 part /boot/efi  
├─vda2                254:2    0  488M  0 part /boot  
└─vda3                254:3    0   31G  0 part   
  ├─debian--vg-root   253:0    0  6.1G  0 lvm  /  
  ├─debian--vg-var    253:1    0  2.4G  0 lvm  /var  
  ├─debian--vg-swap_1 253:2    0  976M  0 lvm  [SWAP]  
  ├─debian--vg-tmp    253:3    0  484M  0 lvm  /tmp  
  └─debian--vg-home   253:4    0 21.1G  0 lvm  /home
```
### 🌞 Lister les partitions en cours d'utilisation 


```df -h``` 
```bash
Filesystem                   Size  Used Avail Use% Mounted on
udev                         931M     0  931M   0% /dev
tmpfs                        198M  1.3M  197M   1% /run
/dev/mapper/debian--vg-root  6.0G  5.0G  713M  88% /
tmpfs                        989M     0  989M   0% /dev/shm
tmpfs                        5.0M  8.0K  5.0M   1% /run/lock
/dev/vda2                    456M  154M  278M  36% /boot
/dev/mapper/debian--vg-home   21G  195M   20G   1% /home
/dev/mapper/debian--vg-tmp   444M   22K  416M   1% /tmp
/dev/mapper/debian--vg-var   2.3G  1.1G  1.2G  48% /var
/dev/vda1                    512M  6.1M  506M   2% /boot/efi
tmpfs                        198M   84K  198M   1% /run/user/1000
```

# 4. Taille et inodes

```df -i```
### 🌞 Lister la quantité d'inodes disponibles sur chaque partition
```
Filesystem                   Inodes  IUsed   IFree IUse% Mounted on

udev                         238177    460  237717    1% /dev       
tmpfs                        253148    813  252335    1% /run       
/dev/mapper/debian--vg-root  403200 119661  283539   30% /  
tmpfs                        253148      1  253147    1% /dev/shm   
tmpfs                        253148      5  253143    1% /run/lock  
/dev/vda2                    124928    301  124627    1% /boot  
/dev/mapper/debian--vg-home 1381744     83 1381661    1% /home  
/dev/mapper/debian--vg-tmp   123952     32  123920    1% /tmp   
/dev/mapper/debian--vg-var   154432   6376  148056    5% /var   
/dev/vda1                         0      0       0     - /boot/efi  
tmpfs                         50629     68   50561    1% /run/user/1000 
```

### 🌞 Déterminer la taille (avec du) et l'inode (avec ls) de chacun des fichiers suivants :

```du -h /boot/vmlinuz```

```bash
/boot/vmlinuz
```
```which bash```

    /usr/bin/bash


# II. Partitioning


## 🌞 Ajouter ces deux disques comme des PV LVM
```
sudo pvcreate /dev/nvme0n1
sudo pvcreate /dev/nvme1n1
```

## 🌞 Créer un VG nommé cat
```
sudo vgcreate cat /dev/nvme1n1 /dev/nvme0n1
```
```bash
Volume group "cat" successfully created
```
## 🌞 Créer un LV nommé meoooow

```sudo lvcreate -L 3G cat -n meooow``` 


## 🌞 Formater la partition (le LV) en autre chose que ext4
```bash
btrfs-progs v6.6.3
See https://btrfs.readthedocs.io for more information.

ERROR: /dev/nvme0n1 appears to contain an existing filesystem (LVM2_member)
ERROR: use the -f option to force overwrite of /dev/nvme0n1
debian@debian:~/Work$ sudo mkfs.btrfs -f /dev/nvme0n1
btrfs-progs v6.6.3
See https://btrfs.readthedocs.io for more information.

Performing full device TRIM /dev/nvme0n1 (10.00GiB) ...
NOTE: several default settings have changed in version 5.15, please make sure
      this does not affect your deployments:
      - DUP for metadata (-m dup)
      - enabled no-holes (-O no-holes)
      - enabled free-space-tree (-R free-space-tree)

Label:              (null)
UUID:               ab81ae55-2e79-4faa-a305-26f37060cd0a
Node size:          16384
Sector size:        4096
Filesystem size:    10.00GiB
Block group profiles:
  Data:             single            8.00MiB
  Metadata:         DUP             256.00MiB
  System:           DUP               8.00MiB
SSD detected:       yes
Zoned device:       no
Incompat features:  extref, skinny-metadata, no-holes, free-space-tree
Runtime features:   free-space-tree
Checksum:           crc32c
Number of devices:  1
Devices:
   ID        SIZE  PATH        
    1    10.00GiB  /dev/nvme0n1
```

## 🌞 Monter la partition meoooow sur le point de montage que vous avez créé
```sudo mount /dev/nvme0n1 /mnt/meow```

```mount | grep /mnt/meow```
```bash
/dev/nvme0n1 on /mnt/meow type btrfs (rw,relatime,ssd,space_cache=v2,subvo lid=5,subvol=/)
```

# 2. Grow !

## 🌞 Agrandir la partition /home


```sudo lvextend -L +2G /dev/debian-vg/home```






# 3. Montage automatique

```sudo nano /etc/fstab```

j'ai rajouter : ```/dev/nvme0n1  /mnt/meow  btrfs  defaults  0  0```