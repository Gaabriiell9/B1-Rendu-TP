# Compte Rendu


## 🌞 Lister tous les processus en cours d'exécution sur votre machine

```ps -e -o pid,comm```

 PID COMM
    1 /sbin/launchd
  459 /usr/libexec/logd
  461 /usr/libexec/UserEventAgent
  463 /System/Library/Frameworks/CoreServices.framework/Versions/A/Frameworks/FSEvents.framework/Versions/A/Support/fseventsd
  464 /System/Library/PrivateFrameworks/MediaRemote.framework/Support/mediaremoted
  467 /usr/sbin/systemstats
  470 /usr/libexec/configd
  472 /System/Library/CoreServices/powerd.bundle/powerd
  473 /usr/libexec/IOMFB_bics_daemon
  477 /usr/libexec/remoted
  482 /usr/libexec/watchdogd
  488 /usr/libexec/kernelmanagerd
  489 /usr/libexec/diskarbitrationd
  493 /usr/sbin/syslogd

## 🌞 Trouver les 3 processus qui ont le plus petit identifiant

```ps -e -o pid,comm | head -n 4```


 PID COMM
    1 /sbin/launchd
  459 /usr/libexec/logd
  461 /usr/libexec/UserEventAgent

## 🌞 Lister tous les services de la machine.

avec une commande, lister tous les services qui sont en cours d'exécution :

```launchctl list | grep -v "0" ```

PID	Status	Label
42561	-15	com.apple.Finder
-	-9	com.apple.knowledgeconstructiond
-	-9	com.apple.inputanalyticsd
41124	-9	com.apple.devicecheckd
39642	-9	com.apple.iconservices.iconservicesagent
-	-9	com.apple.diagnosticextensionsd
39427	-9	com.apple.intelligenceplatformd
39451	-9	com.apple.LinkedNotesUIService
41346	-9	com.apple.ndoagent
39478	-9	com.apple.localizationswitcherd
42152	-9	com.apple.installerauthagent
44683	-9	com.apple.ManagedSettingsAgent
39649	-9	com.apple.photolibraryd
-	-9	com.apple.stickersd

Avec une autre commande, lister tous les services qui existent mais ne sont pas lancés :

```launchctl list | awk '$1 == "-" {print $0}'```

-	0	com.apple.SafariHistoryServiceAgent
-	0	com.apple.quicklook
-	0	com.apple.parentalcontrols.check
-	0	com.apple.amp.mediasharingd
-	-9	com.apple.knowledgeconstructiond
-	-9	com.apple.inputanalyticsd
-	0	com.apple.familycontrols.useragent
-	0	com.apple.AssetCache.agent
-	0	com.apple.UserPictureSyncAgent
-	0	com.apple.syncservices.uihandler
-	-9	com.apple.diagnosticextensionsd
-	0	com.apple.cmio.LaunchCMIOUserExtensionsAgent
-	0	com.apple.bookassetd
-	0	com.apple.ManagedClientAgent.agent
-	0	com.apple.screensharing.agent
-	0	com.apple.AddressBook.SourceSync
-	0	com.apple.languageassetd


2. Mémoire et CPU

## 🌞 RAM

```sysctl hw.memsize  ```                           

                        8589934592 Octet


```vm_stat | grep "Pages free\|Pages inactive" | awk '{sum += $3} END {print (sum * 16384 / 1024 / 1024) " Mo"}'```
	
					      1789,97 Mo

## 🌞 CPU

```ps aux | awk 'BEGIN {sum=0} {sum += $3} END {print "CPU Usage: " sum "%"}'```

CPU Usage: 33,1%

3.Stockage


## 🌞 Périphériques


lister les périphériques de stockage

```df -h  ```

Filesystem        Size    Used   Avail Capacity iused ifree %iused  Mounted on
/dev/disk3s3s1   228Gi   9,9Gi    16Gi    39%    407k  169M    0%   /
devfs            201Ki   201Ki     0Bi   100%     695     0  100%   /dev
/dev/disk3s6     228Gi   2,0Gi    16Gi    12%       2  169M    0%   /System/Volumes/VM
/dev/disk3s4     228Gi   6,1Gi    16Gi    28%    1,2k  169M    0%   /System/Volumes/Preboot
/dev/disk3s2     228Gi    43Mi    16Gi     1%      52  169M    0%   /System/Volumes/Update
/dev/disk1s2     500Mi   6,0Mi   482Mi     2%       1  4,9M    0%   /System/Volumes/xarts
/dev/disk1s1     500Mi   5,6Mi   482Mi     2%      34  4,9M    0%   /System/Volumes/iSCPreboot
/dev/disk1s3     500Mi   1,8Mi   482Mi     1%      74  4,9M    0%   /System/Volumes/Hardware
/dev/disk3s1     228Gi   193Gi    16Gi    93%    595k  169M    0%   /System/Volumes/Data
map auto_home      0Bi     0Bi     0Bi   100%       0     0     -   /System/Volumes/Data/home


je veux voir les disques durs branchés à votre PC, pas les partitions

```diskutil list | grep -E '^/dev/disk' | grep -v 'part' | awk '{print $1, $3, $4}'```



/dev/disk0 physical: 
/dev/disk3  



## 🌞 Partitions

```diskutil list | grep -E '^[[:space:]]*[0-9]+:'```

 0:      GUID_partition_scheme                        *251.0 GB   disk0
   1:             Apple_APFS_ISC Container disk1         524.3 MB   disk0s1
   2:                 Apple_APFS Container disk3         245.1 GB   disk0s2
   3:        Apple_APFS_Recovery Container disk2         5.4 GB     disk0s3
   0:      APFS Container Scheme -                      +245.1 GB   disk3
   1:                APFS Volume Macintosh HD - Data     207.3 GB   disk3s1
   2:                APFS Volume Macintosh HD            10.7 GB    disk3s3
   3:              APFS Snapshot com.apple.os.update-... 10.7 GB    disk3s3s1
   4:                APFS Volume Preboot                 6.6 GB     disk3s4
   5:                APFS Volume Recovery                966.7 MB   disk3s5
   6:                APFS Volume VM                      2.1 GB     disk3s6


## 🌞 Espace disque

afficher l'espace disque restant sur votre partition principale

```df -h ```

Filesystem        Size    Used   Avail Capacity iused ifree %iused  Mounted on
/dev/disk3s3s1   228Gi   9,9Gi    16Gi    39%    407k  169M    0%   /


4. Réseau


## 🌞 Cartes réseau

```ifconfig | awk '/^[a-z]/ {iface=$1} /inet / {print iface ": " $2}'```

lo0:: 127.0.0.1
en0:: 10.3.204.51


## 🌞 Connexions réseau


```netstat -tulnp | grep ESTABLISHED```

netstat: option requires an argument -- p
Usage:	netstat [-AaLlnW] [-f address_family | -p protocol]
	netstat [-gilns] [-f address_family]
	netstat -i | -I interface [-w wait] [-abdgRtS]
	netstat -s [-s] [-f address_family | -p protocol] [-w wait]
	netstat -i | -I interface -s [-f address_family | -p protocol]
	netstat -m [-m]
	netstat -r [-Aaln] [-f address_family]
	netstat -rs [-s]



5. Utilisateurs


## 🌞 Lister les utilisateurs de la machine

```dscl . list /Users```

_accessoryupdater
_amavisd
_analyticsd
_aonsensed
_appinstalld
_appleevents
_applepay
_appowner
_appserver
_appstore
_ard
_asset




```dscacheutil -q group -a name admin```

name: admin
password: *
gid: 80
users: root fariasgomesgabriel 


## 🌞 Heure de login

```last -1 $(whoami)```

fariasgomesgabriel ttys000                         Lun  4 nov 16:00   still logged in

## 🌞 Lister les processus en cours d'exécution

```ps -eo user,pid,comm```
USER               PID COMM
root                 1 /sbin/launchd
root               459 /usr/libexec/logd
root               461 /usr/libexec/UserEventAgent
root               463 /System/Library/Frameworks/CoreServices.framework/Versions/A/Frameworks/FSEvents.framework/Versions/A/Support/fseventsd
root               464 /System/Library/PrivateFrameworks/MediaRemote.framework/Support/mediaremoted
root               467 /usr/sbin/systemstats
root               470 /usr/libexec/configd
root               472 /System/Library/CoreServices/powerd.bundle/powerd
root               473 /usr/libexec/IOMFB_bics_daemon
root               477 /usr/libexec/remoted
root               482 /usr/libexec/watchdogd
root               488 /usr/libexec/kernelmanagerd
root               489 /usr/libexec/diskarbitrationd


## 🌞 Sur un fichier random qui se trouve dans votre dossier Téléchargements/

```ls -l Documents/Compte\ Rendu.odt ```
-rw-r--r--@ 1 fariasgomesgabriel  staff  31165  4 nov 17:46 Documents/Compte Rendu.odt


6. Random

## 🌞 Uptime

```uptime   ```                                                        

18:17  up 6 days, 15:34, 2 users, load averages: 1,91 1,84 1,92

## 🌞 Device

```sysctl -n machdep.cpu.brand_string```
Apple M2

## 🌞 Version

```sw_vers```

ProductName:		macOS
ProductVersion:		15.0.1
BuildVersion:		24A348

## 🌞 Mise à jour

```ls -lt /var/db/receipts/ | grep -i "update"```

-rw-r--r--  1 root  wheel      281 29 oct 02:55 com.razerzone.RzUpdater.pkg.plist
-rw-r--r--  1 root  wheel    38168 29 oct 02:55 com.razerzone.RzUpdater.pkg.bom
-rw-r--r--  1 root  wheel      348 15 oct 21:45 com.microsoft.package.Microsoft_AutoUpdate.app.plist
-rw-r--r--  1 root  wheel   129856 15 oct 21:45 com.microsoft.package.Microsoft_AutoUpdate.app.bom



7. Ptit amusement


## 🌞 Lister les connexions actives


```netstat -atn```

Active Internet connections (including servers)
Proto Recv-Q Send-Q  Local Address          Foreign Address        (state)    
tcp4       0      0  10.3.204.51.50436      185.60.219.174.443     ESTABLISHED
tcp4       0      0  10.3.204.51.50432      34.117.188.166.443     ESTABLISHED
tcp4       0      0  10.3.204.51.50431      34.110.138.217.443     ESTABLISHED
tcp4       0      0  10.3.204.51.50430      172.65.251.78.443      ESTABLISHED
tcp4       0      0  10.3.204.51.50428      35.186.224.45.443      ESTABLISHED
tcp4       0      0  10.3.204.51.50427      35.186.224.24.443      ESTABLISHED
tcp4       0      0  10.3.204.51.50426      104.18.32.47.443       ESTABLISHED
tcp4       0      0  10.3.204.51.50424      162.159.134.234.443    ESTABLISHED
tcp4       0      0  10.3.204.51.50414      104.18.32.47.443       ESTABLISHED
tcp4       0      0  10.3.204.51.50411      34.107.243.93.443      ESTABLISHED
tcp4       0      0  127.0.0.1.6463         *.*                    LISTEN     
tcp6       0      0  *.49657                *.*                    LISTEN     
tcp4       0      0  *.49657                *.*                    LISTEN     
tcp4       0      0  10.3.204.51.49341      17.57.156.25.993       ESTABLISHED
tcp4       0      0  10.3.204.51.49340      17.57.156.25.993       ESTABLISHED
tcp6       0      0  fe80::3ec3:369f:.1025  fe80::ca0:b619:8.1026  ESTABLISHED
tcp6       0      0  fe80::3ec3:369f:.1024  fe80::ca0:b619:8.1024  ESTABLISHED
tcp4       0      0  127.0.0.1.9000         *.*                    LISTEN     
tcp6       0      0  *.5000                 *.*                    LISTEN     
tcp4       0      0  *.5000                 *.*                    LISTEN     
tcp6       0      0  *.7000                 *.*                    LISTEN     
tcp4       0      0  *.7000                 *.*                    LISTEN     
tcp4       0      0  *.22                   *.*                    LISTEN     
tcp6       0      0  *.22                   *.*                    LISTEN     
tcp4       0      0  10.3.204.51.53784      17.253.37.216.80       ESTABLISHED
tcp4       0      0  10.3.204.51.53783      17.253.37.214.80       TIME_WAIT  
tcp4       0      0  10.3.204.51.53464      17.188.185.137.5223    ESTABLISHED

Mon IP : 192.168.1.2  				 IP du serveur : 203.0.113.5
			Nom du programme : Firefox.#

## 🌞 En apprendre + sur le processus en cours d'exécution


```ps -p 4215 -o pid,user,%mem,command```

  PID USER             %MEM COMMAND
 4215 fariasgomesgabriel  4,9 /Applications/Firefox.app/Contents/MacOS/firefox


## 🌞 En apprendre + sur le programme

```ls -l /Applications/Firefox.app/Contents/MacOS/firefox```

-rwxrwxr-x@ 1 fariasgomesgabriel  admin  156352 29 oct 20:49 /Applications/Firefox.app/Contents/MacOS/firefox

## 🌞 En apprendre + sur l'adresse IP

```curl http://ip-api.com/json/203.0.113.5    ``` 

{"status":"success","country":"United States","countryCode":"US","region":"NY","regionName":"New York","city":"New York","zip":"000000","lat":40.7127,"lon":-74.0059,"timezone":"America/New_York","isp":"TEST-NET-3","org":"TEST-NET-3 RFC5737","as":"","query":"203.0.113.5"}%    

## 🌞 Délai

```ping 203.0.113.5   ```        

PING 203.0.113.5 (203.0.113.5): 56 data bytes

Request timeout for icmp_seq 0  
Request timeout for icmp_seq 1  
Request timeout for icmp_seq 2