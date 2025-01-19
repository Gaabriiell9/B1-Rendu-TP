# Compte Rendu TP3

## 🌞 Afficher la ligne du fichier qui concerne votre utilisateur

    cat /etc/passwd | grep fariasgomes

fariasgomes:x:1000:1000:fariasgomes,,,:v/home/fariasgomes:/bin/bash

## 🌞 Afficher la ligne du fichier qui concerne votre utilisateur ET celle de root en même temps

 

    cat /etc/passwd | grep -E "fariasgomes|root"

root:x:0:0:root:/root:/bin/bash
fariasgomes:x:1000:1000:fariasgomes,,,:/home/fariasgomes:/bin/bash


## 🌞 Afficher la liste des groupes d'utilisateurs de la machine

    cat /etc/group

root:x:0:  
daemon:x:1:  
bin:x:2:  
sys:x:3:  
adm:x:4:  
tty:x:5:  
disk:x:6:  
lp:x:7:  
mail:x:8:  
news:x:9:  
uucp:x:10:  
man:x:12:  
proxy:x:13:  

j'ai pris que les 13 premiers pour pas que ça soit trop sur le compte rendu.


## 🌞 Afficher la ligne du fichier qui concerne votre utilisateur ET celle de root en même temps

cat /etc/passwd | grep -E "root|$(whoami)" | cut -d":" -f1

root  
fariasgomes
# 2. Hash des passwords

## 🌞 Afficher la ligne qui contient le hash du mot de passe de votre utilisateur


    sudo cat /etc/shawdow | grep "fariasgomes"

[sudo] password for fariasgomes:   
fariasgomes:$y$j9T$lqyf6fLDNNtPW.dywmHTX/$lzUbJ8MmfR..Oh2LCc49wjtwugi33CEE7Una7A.4vl/:20033:0:99999:7:::

## 🌞 Faites en sorte que votre utilisateur puisse taper n'importe quelle commande sudo

    sudo usermod -aG sudo toto


# B. Practice
## 🌞 Créer un groupe d'utilisateurs


    getent group stronk_admins  
    sudo adduser imbob

## 🌞 Créer un utilisateur

    sudo usermod -aG stronk_admins imbob 


il devra appartenir aux groupes imbob et stronk_admins


    sudo usermod -aG stronk_admins imbob

## 🌞 Prouver que l'utilisateur imbob a un password défini

    sudo cat /etc/shadow | grep "imbob"

imbob:$y$j9T$lERJMFUoCSlWOgBU1Rb8E.  $ZlIaFxCsjUZ22q3tZFCnQAFylAQpNWZROZtVUB8i6GA:20040:0:99999:7:::

## 🌞 Prouver que l'utilisateur imbob appartient au groupe stronk_admins

    sudo cat /etc/group | grep "imbob"
users:x:100:fariasgomes,toto,marmotte,imbob  
stronk_admins:x:1003:imbob  
imbob:x:1004:  

## 🌞 Créer un deuxième utilisateur

il devra appartenir au groupe imnotbobsorry ET stronk_admins

    sudo adduser imnotbobsorry  
    sudo usermod -aG stronk_admins imnotbobsorry


## 🌞 Modifier la configuration de sudo pour que

    sudo nano /etc/sudoers

j'ai mit la ligne ```%stronk_admins ALL=(ALL:ALL) ALL```

    # Allow members of group sudo to execute any command
    %sudo   ALL=(ALL:ALL) ALL
    %stronk_admins ALL=(ALL:ALL) ALL

## 🌞 Créer le dossier /home/goodguy (avec une commande)

    sudo mkdir /home/goodguy

## 🌞 Changer le répertoire personnel de imbob

    sudo usermod -d /home/goodguy imbob

## 🌞 Créer le dossier /home/badguy

    sudo mkdir /home/badguy
    sudo usermod -d /home/badguy imnotbobsorry

Prouvez que le changement est effectif en affichant le contenu du fichier passwd

    cat /etc/passwd | grep "imnotbobsorry"
imnotbobsorry:x:1005:1005:,,,:/home/badguy:/bin/bash

## 🌞 Prouver que les permissions du dossier /home/gooduy sont incohérentes

    s -l ..  

total 28 
drwxr-xr-x  2 root          root          4096 Nov 13 15:03 badguy  
drwx------ 16 fariasgomes   fariasgomes   4096 Nov 13 14:09 fariasgomes  
drwxr-xr-x  2 root          root          4096 Nov 13 14:53 goodguy  
drwx------  2 imbob         imbob         4096 Nov 13 14:49 imbob  
drwx------  2 imnotbobsorry imnotbobsorry 4096 Nov 13 14:31 imnotbobsorry  
drwx------  2 marmotte      marmotte      4096 Nov 13 00:47 marmotte  
drwx------  2 toto          toto          4096 Nov 12 23:03 toto  

## 🌞 Modifier les permissions de /home/goodguy

    sudo chown imbob:imbob /home/goodguy


## 🌞 Modifier les permissions de /home/badguy

    sudo chown imnotbobsorry:imnotbobsorry /home/badguy 


## 🌞 Connectez-vous sur l'utilisateur imbob

    su - imbob

Password:   
imbob@tpos:~$  

    mbob@tpos:~$ pwd

/home/goodguy



# II. Processes

## 🌞 Affichez les processus bash

    ps aux | grep "bash"

fariasg+     908  0.0  0.3   7872  3952 pts/0    Ss+  14:09   0:00 bash  
fariasg+    1157  0.0  0.4   9236  4636 pts/1    Ss+  14:10   0:00 bash  
fariasg+    1428  0.0  0.4   8004  4836 pts/3    Ss   14:49   0:00 bash  
fariasg+    1875  0.0  0.1   6092  1956 pts/3    S+   15:52   0:00 grep bash  


## 🌞 Affichez tous les processus lancés par votre utilisateur
    ps -u 
USER         PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND  
fariasg+     908  0.0  0.3   7872  3952 pts/0    Ss+  14:09   0:00 bash   
fariasg+    1157  0.0  0.4   9236  4636 pts/1    Ss+  14:10   0:00 bash   
fariasg+    1428  0.0  0.4   8004  4836 pts/3    Ss   14:49   0:00 bash  
fariasg+    1882  150  0.4  11272  4112 pts/3    R+   15:54   0:00 ps -u  


## 🌞 Affichez le top 5 des processus qui utilisent le plus de RAM

    free -h
total        used        free      shared  buff/cache   available  
Mem:           973Mi       537Mi       116Mi        17Mi       417Mi       435Mi  
Swap:          948Mi       780Ki       948Mi  

## 🌞 Affichez le PID du processus du service SSH


    pgrep -x sshd
512

## 🌞 Affichez le nom du processus avec l'identifiant le plus petit

    ps -e | head -n 2
PID TTY          TIME CMD  
1   ?        00:00:00 systemd


# 2. Parent, enfant, et meurtre

## 🌞 Déterminer le PID de votre shell actuel

    ps -e | grep "bash"

OU

    echo $$

1428


## 🌞 Déterminer le PPID de votre shell actuel

    ps -o ppid= -p $$
811

## 🌞 Déterminer le nom de ce processus

    $ ps -p $(ps -o ppid= -p $$) -o comm=


## 🌞 Lancer un processus sleep 9999 en tâche de fond

    sleep 999  &

    ps -o pid,ppid,comm -p $!

PID    PPID COMMAND  
1923    1428 sleep


# III. Services

## 2. Analyser un service existant

## 🌞 S'assurer que le service ssh est démarré

```sudo systemctl status SSH```


## 🌞 Isolez la ligne qui indique le nom du programme lancé

```sudo systemctl start ssh```


```systemctl status ssh```
```bash
● ssh.service - OpenBSD Secure Shell server
     Loaded: loaded (/usr/lib/systemd/system/ssh.service; enabled; preset: disabled)
     Active: active (running) since Wed 2024-11-20 10:13:20 CET; 1h 2min ago
 Invocation: a3aba34d2cca4531979c99781dda9375
       Docs: man:sshd(8)
             man:sshd_config(5)
   Main PID: 691 (sshd)
      Tasks: 1 (limit: 2232)
     Memory: 6.8M (peak: 24M)
        CPU: 154ms
     CGroup: /system.slice/ssh.service
             └─691 "sshd: /usr/sbin/sshd -D [listener] 0 of 10-100 startups"

Warning: some journal files were not opened due to insufficient permissions.

```
Quelle commande ? Dans le systemctl status, le nom et le PID du programme lancé sont indiqués, isolez donc cette ligne


```sudo systemctl status ssh | grep -E '●|PID'```


## 🌞 Déterminer le port sur lequel écoute le service SSH
```sudo ss -lnpt```
```bash
State            Recv-Q           Send-Q                       Local Address:Port                        Peer Address:Port           Process                                              
LISTEN           0                1024                             127.0.0.1:40635                            0.0.0.0:*               users:(("code-4849ca9bdf",pid=1503,fd=9))           
LISTEN           0                4096                             127.0.0.1:631                              0.0.0.0:*               users:(("cupsd",pid=674,fd=7))                      
LISTEN           0                128                                0.0.0.0:22                               0.0.0.0:*               users:(("sshd",pid=691,fd=7))                       
LISTEN           0                4096                                 [::1]:631                                 [::]:*               users:(("cupsd",pid=674,fd=6))                      
LISTEN           0                128    
```


## 🌞 Consulter les logs du service SSH


```sudo journalctl -u ssh```

```bash
Nov 15 17:43:16 debian systemd[1]: Starting ssh.service - OpenBSD Secure Shell server...
Nov 15 17:43:16 debian sshd[591]: Server listening on 0.0.0.0 port 22.
Nov 15 17:43:16 debian sshd[591]: Server listening on :: port 22.
Nov 15 17:43:16 debian systemd[1]: Started ssh.service - OpenBSD Secure Shell server.
-- Boot aea5b060157846a89fa1677383641da2 --
Nov 18 15:35:44 debian systemd[1]: Starting ssh.service - OpenBSD Secure Shell server...
Nov 18 15:35:44 debian sshd[596]: Server listening on 0.0.0.0 port 22.
Nov 18 15:35:44 debian sshd[596]: Server listening on :: port 22.
Nov 18 15:35:44 debian systemd[1]: Started ssh.service - OpenBSD Secure Shell server.
-- Boot 0d65d8ae7d494f95b5f7f45246f0708a --
Nov 18 15:50:07 debian systemd[1]: Starting ssh.service - OpenBSD Secure Shell server...
Nov 18 15:50:07 debian sshd[613]: Server listening on 0.0.0.0 port 22.
Nov 18 15:50:07 debian sshd[613]: Server listening on :: port 22.
Nov 18 15:50:07 debian systemd[1]: Started ssh.service - OpenBSD Secure Shell server.
Nov 18 15:51:00 debian sshd[613]: Received signal 15; terminating.
Nov 18 15:51:00 debian systemd[1]: Stopping ssh.service - OpenBSD Secure Shell server...
Nov 18 15:51:00 debian systemd[1]: ssh.service: Deactivated successfully.
```


# 3. Modification du service

## A. Configuration du service SSH

## 🌞 Identifier le fichier de configuration du serveur SSH

```ls -l /etc/ssh/```
```bash
total 572
-rw-r--r-- 1 root root 541653 Oct 27 14:58 moduli
-rw-r--r-- 1 root root   1649 Oct 27 14:58 ssh_config
drwxr-xr-x 2 root root   4096 Nov 20 09:16 ssh_config.d
-rw-r--r-- 1 root root   3223 Jun 22 21:38 sshd_config
drwxr-xr-x 2 root root   4096 Jun 22 21:38 sshd_config.d
-rw------- 1 root root    505 Nov 15 17:41 ssh_host_ecdsa_key
-rw-r--r-- 1 root root    173 Nov 15 17:41 ssh_host_ecdsa_key.pub
-rw------- 1 root root    399 Nov 15 17:41 ssh_host_ed25519_key
-rw-r--r-- 1 root root     93 Nov 15 17:41 ssh_host_ed25519_key.pub
-rw------- 1 root root   2602 Nov 15 17:41 ssh_host_rsa_key
-rw-r--r-- 1 root root    565 Nov 15 17:41 ssh_host_rsa_key.pub
```

## 🌞 Modifier le fichier de conf

```sudo nano /etc/ssh/sshd_config```


```cat /etc/ssh/sshd_config | grep 'Port'```

#Port 2222
#GatewayPorts no


# 🌞 Redémarrer le service

sudo systemctl restart ssh
