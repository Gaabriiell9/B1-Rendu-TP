# Compte Rendu

## 🌞 Trouver le chemin vers le répertoire personnel de votre utilisateur
    sudo adduser toto

[sudo] password for fariasgomes:   
Adding user `toto' ...  
Adding new group `toto' (1001) ...  
Adding new user `toto' (1001) with group `toto (1001)' ...  
Creating home directory `/home/toto' ...  
Copying files from `/etc/skel' ...  
New password:   
Retype new password:   
passwd: password updated successfully  
Changing the user information for toto  
Enter the new value, or press ENTER for the default  
	Full Name []: toto  
	Room Number []: 10  
	Work Phone []:   
	Home Phone []:   
	Other []:   
Is the information correct? [Y/n] y  
Adding new user `toto' to supplemental / extra groups `users' ...  
Adding user `toto' to group `users' ...  

    sudo su - toto

toto@tpos:~$ 

    cat /etc/passwd

root:x:0:0:root:/root:/bin/bash  
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin  
bin:x:2:2:bin:/bin:/usr/sbin/nologin  
sys:x:3:3:sys:/dev:/usr/sbin/nologin
sync:x:4:65534:sync:/bin:/bin/sync  
games:x:5:60:games:/usr/games:/usr/sbin/nologin  
man:x:6:12:man:/var/cache/man:/usr/sbin/nologin  
lp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin  
mail:x:8:8:mail:/var/mail:/usr/sbin/nologin  
news:x:9:9:news:/var/spool/news:/usr/sbin/nologin  
uucp:x:10:10:uucp:/var/spool/uucp:/usr/sbin/nologin  
proxy:x:13:13:proxy:/bin:/usr/sbin/nologin  
www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin  
backup:x:34:34:backup:/var/backups:/usr/sbin/nologin  
list:x:38:38:Mailing List Manager:/var/list:/usr/sbin/nologin  
irc:x:39:39:ircd:/run/ircd:/usr/sbin/nologin  
_apt:x:42:65534::/nonexistent:/usr/sbin/nologin  
nobody:x:65534:65534:nobody:/nonexistent:/usr/sbin/nologin  
systemd-network:x:998:998:systemd Network Management:/:/usr/sbin/nologin 
systemd-timesync:x:997:997:systemd Time Synchronization:/:/usr/sbin/nologin  
messagebus:x:100:107::/nonexistent:/usr/sbin/nologin  
sshd:x:101:65534::/run/sshd:/usr/sbin/nologin  
dnsmasq:x:102:65534:dnsmasq,,,:/var/lib/misc:/usr/sbin/nologin  
avahi:x:103:110:Avahi mDNS daemon,,,:/run/avahi-daemon:/usr/sbin/nologin 
speech-dispatcher:x:104:29:Speech Dispatcher,,,:/run/speech-dispatcher:/bin/false  
pulse:x:105:112:PulseAudio daemon,,,:/run/pulse:/usr/sbin/nologin  
saned:x:106:115::/var/lib/saned:/usr/sbin/nologin  
lightdm:x:107:116:Light Display Manager:/var/lib/lightdm:/bin/false  
polkitd:x:996:996:polkit:/nonexistent:/usr/sbin/nologin  
rtkit:x:108:117:RealtimeKit,,,:/proc:/usr/sbin/nologin  
colord:x:109:118:colord colour management daemon,,,:/var/lib/colord:/usr/sbin/nologin  
fariasgomes:x:1000:1000:fariasgomes,,,:/home/fariasgomes:/bin/bash
toto:x:1001:1001:toto,10,,:/home/toto:/bin/bash

## 🌞 Vérifier les permissions du répertoire personnel de votre utilisateurs

    ls -l

total 8  
drwx------ 16 fariasgomes fariasgomes 4096 Nov 12 11:37 fariasgomes  
drwx------  2 toto        toto        4096 Nov 12 11:42 toto  

## 🌞 Trouver le chemin du fichier de configuration du serveur SSH

    sudo -u USER COMMAND

sudo: unknown user USER  
sudo: error initializing audit plugin sudoers_audit  

# 2. Users


## 🌞 Créer un nouvel utilisateur

    sudo useradd marmotte
    sudo passwd marmotte  

New password:   
Retype new password:   
passwd: password updated successfully  

## 🌞 Prouver que cet utilisateur a été créé

    cat /etc/passwd | grep marmotte

marmotte:x:1002:1002:,,,:/home/marmotte:/bin/bash

## 🌞 Déterminer le hash du password de l'utilisateur marmotte

    sudo cat /etc/shadow file | grep marmotte

marmotte:$y$j9T$z4PnP4TGVhS59QdGNSsgb.$hb6By3vTlbJ78ji4u1S8Fo8wL0LI.JW4YpKuQmloJF2:20039:0:99999:7:::

# Connexion sur le nouvel utilisateur 

## 🌞 Tapez une commande pour vous déconnecter : fermer votre session utilisateur

    exit


# III. Programmes et paquets

## 🌞 Lancer un processus sleep


    sleep 1000

    ps aux | grep sleep

fariasg+    4739  0.0  0.0   5204   788 pts/6    S+   23:45   0:00 sleep 1000
fariasg+    4768  0.0  0.1   6092  1940 pts/7    S+   23:47   0:00 grep sleep
# B. Tâche de fond
## 🌞 Lancer un nouveau processus sleep, mais en tâche de fond

    sleep 1000 &

[6] 4780


## 🌞 Visualisez la commande en tâche de fond

    jobs

[3]+  Stopped                 sleep 1000
[5]   Running                 sleep 1000 &
# C. Find paths
## 🌞 Trouver le chemin où est stocké le programme sleep

    sudo find / -name "sleep"

/usr/bin/sleep  
/usr/lib/klibc/bin/sleep

    sudo find / -name ".bashrc"

/etc/skel/.bashrc  
/home/toto/.bashrc  
/home/fariasgomes/gameshell/gameshell.1/World/.bashrc  
/home/fariasgomes/gameshell/gameshell/World/.bashrc  
/home/fariasgomes/.bashrc  
/home/marmotte/.bashrc  
/root/.bashrc  

# 🌞 Vérifier que

les commandes sleep, ssh, et ping sont bien des programmes stockés dans l'un des dossiers listés dans votre PATH

    which sleep ssh ping

/usr/bin/sleep
/usr/bin/ssh
/usr/bin/ping

# 2. Paquets

## 🌞 Installer le paquet firefox

    sudo apt install firefox

## 🌞 Utiliser une commande pour lancer Firefox

    sudo ln -s /usr/bin/firefox   
    export DISPLAY=:0

# 🌞 Mais aussi déterminer...
il existe un dossier qui contient la liste des URLs consultées quand vous demandez un téléchargement de paquets

    cat /etc/apt/sources.list

# IV. Poupée russe

## 🌞 Récupérer le fichier meow

    curl -L -o meow https://gitlab.com/it4lik/b1-os/-/blob/main/tp/2/meow

## 🌞 Trouver le dossier dawa/

    file meow

meow: XZ compressed data, checksum CRC64
 

    mv meow meow.zip

meow.zip


    unzip meow.zip

Archive:  meow.zip  
  inflating: meow   
