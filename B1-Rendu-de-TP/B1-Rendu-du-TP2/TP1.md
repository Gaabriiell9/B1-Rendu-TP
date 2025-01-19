# I. Service SSH

## 1. Analyse du service

### 🌞 S'assurer que le service sshd est démarré

````bash
systemctl status sshd 
● sshd.service - OpenSSH server daemon
     Loaded: loaded (/usr/lib/systemd/system/sshd.service; enabled; preset: enabled)
     Active: active (running) since Mon 2024-12-02 16:00:48 CET; 6min ago
       Docs: man:sshd(8)
             man:sshd_config(5)
   Main PID: 695 (sshd)
      Tasks: 1 (limit: 10236)
     Memory: 5.0M
        CPU: 91ms
     CGroup: /system.slice/sshd.service
             └─695 "sshd: /usr/sbin/sshd -D [listener] 0 of 10-100 startups"

Dec 02 16:00:48 gabz.b1 systemd[1]: Starting OpenSSH server daemon...
Dec 02 16:00:48 gabz.b1 sshd[695]: Server listening on 0.0.0.0 port 22.
Dec 02 16:00:48 gabz.b1 sshd[695]: Server listening on :: port 22.
Dec 02 16:00:48 gabz.b1 systemd[1]: Started OpenSSH server daemon.
Dec 02 16:05:22 gabz.b1 sshd[1281]: Accepted password for fariasgomes from 10.1.1.100 port 64353 ssh2
Dec 02 16:05:22 gabz.b1 sshd[1281]: pam_unix(sshd:session): session opened for user fariasgomes(uid=1000) by fariasgomes(uid=0)
````

### 🌞 Analyser les processus liés au service SSH

ps aux | grep sshd

```bash
root         695  0.0  0.4  16456  8192 ?        Ss   16:00   0:00 sshd: /usr/sbin/sshd -D [listener] 0 of 10-100 startups
root        1281  0.0  0.6  21896 10240 ?        Ss   16:05   0:00 sshd: fariasgomes [priv]
fariasg+    1285  0.0  0.3  22080  6520 ?        S    16:05   0:00 sshd: fariasgomes@pts/0
fariasg+    1314  0.0  0.4  22048  8192 pts/0    T    16:07   0:00 systemctl status sshd
fariasg+    1327  0.0  0.1   6116  1920 pts/0    S+   16:14   0:00 grep --color=auto sshd
```


### 🌞 Déterminer le port sur lequel écoute le service SSH

 ss -t | grep ssh
 ```bash
ESTAB 0      0         10.1.1.101:ssh    10.1.1.100:64353   
```

### 🌞 Consulter les logs du service SSH

journalctl -u sshd
```
Dec 02 16:00:48 gabz.b1 systemd[1]: Starting OpenSSH server daemon...
Dec 02 16:00:48 gabz.b1 sshd[695]: Server listening on 0.0.0.0 port 22.
Dec 02 16:00:48 gabz.b1 sshd[695]: Server listening on :: port 22.
Dec 02 16:00:48 gabz.b1 systemd[1]: Started OpenSSH server daemon.
Dec 02 16:05:22 gabz.b1 sshd[1281]: Accepted password for fariasgomes from 10.1.1.100 port 64353 ssh2
Dec 02 16:05:22 gabz.b1 sshd[1281]: pam_unix(sshd:session): session opened for user fariasgomes(uid=1000) by fariasgomes(uid=0)
[fariasgomes@gabz ~]$ 
```

sudo tail -n 10 /var/log/secure

```
Dec  2 16:01:33 gabz sudo[1274]: fariasgomes : TTY=tty1 ; PWD=/home/fariasgomes ; USER=root ; COMMAND=/bin/nano /etc/sysconfig/network-scripts/ifcfg-enp0s1
Dec  2 16:01:33 gabz sudo[1274]: pam_unix(sudo:session): session opened for user root(uid=0) by fariasgomes(uid=1000)
Dec  2 16:02:00 gabz sudo[1274]: pam_unix(sudo:session): session closed for user root
Dec  2 16:05:22 gabz sshd[1281]: Accepted password for fariasgomes from 10.1.1.100 port 64353 ssh2
Dec  2 16:05:22 gabz sshd[1281]: pam_unix(sshd:session): session opened for user fariasgomes(uid=1000) by fariasgomes(uid=0)
Dec  2 16:09:29 gabz sudo[1318]: fariasgomes : TTY=pts/0 ; PWD=/home/fariasgomes ; USER=root ; COMMAND=/bin/systemctl list-units -t service -a
Dec  2 16:09:29 gabz sudo[1318]: pam_unix(sudo:session): session opened for user root(uid=0) by fariasgomes(uid=1000)
Dec  2 16:22:23 gabz sudo[1345]: fariasgomes : TTY=pts/0 ; PWD=/home/fariasgomes ; USER=root ; COMMAND=/bin/tail -n 10 /var/log/secure
Dec  2 16:22:23 gabz sudo[1345]: pam_unix(sudo:session): session opened for user root(uid=0) by fariasgomes(uid=1000)
Dec  2 16:22:23 gabz sudo[1345]: pam_unix(sudo:session): session closed for user root
```

# 2. Modification du service

### 🌞 Identifier le fichier de configuration du serveur SSH
```
 which ssh
/usr/bin/ssh
```


### 🌞 Modifier le fichier de conf
```bash
[fariasgomes@gabz ~]$ echo $RANDOM
4319
[fariasgomes@gabz ~]$ sudo nano /etc/ssh/sshd_config

[fariasgomes@gabz ~]$ sudo cat /etc/ssh/sshd_config | grep Port
```

```bash
[fariasgomes@gabz ~]$ sudo firewall-cmd --add-port=4319/tcp --permanent
success

[fariasgomes@gabz ~]$ sudo firewall-cmd --reload
success

[fariasgomes@gabz ~]$ sudo firewall-cmd --list-all | grep 4319
  ports: 4319/tcp
```


### 🌞 Redémarrer le service

sudo systemctl restart sshd


### 🌞 Effectuer une connexion SSH sur le nouveau port
```
fariasgomesgabriel@MacBook-Pro-de-Gabriel ~ % ssh -p 4319 fariasgomes@10.1.1.101
fariasgomes@10.1.1.101's password: 
Last login: Mon Dec  2 17:07:02 2024 from 10.1.1.100
[fariasgomes@gabz ~]$ 
```

# II. Service HTTP

### 🌞 Installer le serveur NGINX
```
sudo dnf search nginx
sudo dnf install nginx
```
 
### 🌞 Démarrer le service NGINX
```
sudo systemctl start nginx
sudo systemctl enable nginx
```

### 🌞 Déterminer sur quel port tourne NGINX

cat /etc/nginx/nginx.conf | grep listen
```
        listen       80;
        listen       [::]:80;
#        listen       443 ssl http2;
#        listen       [::]:443 ssl http2;
[fariasgomes@gabz ~]$ 
```

### 🌞 Déterminer les processus liés au service NGINX
ps aux | grep nginx
```
root        9685  0.0  0.0  10164  1384 ?        Ss   17:19   0:00 nginx: master process /usr/sbin/nginx
nginx       9686  0.0  0.2  14676  4716 ?        S    17:19   0:00 nginx: worker process
nginx       9687  0.0  0.3  14676  5228 ?        S    17:19   0:00 nginx: worker process
fariasg+    9724  0.0  0.1   6116  1920 pts/0    S+   17:23   0:00 grep --color=auto nginx
```

### 🌞 Déterminer le nom de l'utilisateur qui lance NGINX
cat /etc/passwd | grep nginx
```
nginx:x:995:991:Nginx web server:/var/lib/nginx:/sbin/nologin
```

### 🌞 Test !
curl http://10.1.1.101:80 | head -n 7

```html
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
<!doctype html>    0    0     0      0      0 --:--:-- --:--:-- --:--:--     0
<html>
  <head>
    <meta charset='utf-8'>
    <meta name='viewport' content='width=device-width, initial-scale=1'>
    <title>HTTP Server Test Page powered by: Rocky Linux</title>
    <style type="text/css">
100  7620  100  7620    0     0  3720k      0 --:--:-- --:--:-- --:--:-- 3720k
curl: (23) Failed writing body
[fariasgomes@gabz ~]$ 
```

# 2. Analyser la conf de NGINX

### 🌞 Déterminer le path du fichier de configuration de NGINX

ls -al /etc/nginx/
```
total 84
drwxr-xr-x.  4 root root 4096 Dec  2 17:19 .
drwxr-xr-x. 88 root root 8192 Dec  2 17:19 ..
drwxr-xr-x.  2 root root    6 Nov  8 17:44 conf.d
drwxr-xr-x.  2 root root    6 Nov  8 17:44 default.d
-rw-r--r--.  1 root root 1077 Nov  8 17:44 fastcgi.conf
-rw-r--r--.  1 root root 1077 Nov  8 17:44 fastcgi.conf.default
-rw-r--r--.  1 root root 1007 Nov  8 17:44 fastcgi_params
-rw-r--r--.  1 root root 1007 Nov  8 17:44 fastcgi_params.default
-rw-r--r--.  1 root root 2837 Nov  8 17:44 koi-utf
-rw-r--r--.  1 root root 2223 Nov  8 17:44 koi-win
-rw-r--r--.  1 root root 5231 Nov  8 17:44 mime.types
-rw-r--r--.  1 root root 5231 Nov  8 17:44 mime.types.default
-rw-r--r--.  1 root root 2334 Nov  8 17:43 nginx.conf
-rw-r--r--.  1 root root 2656 Nov  8 17:44 nginx.conf.default
-rw-r--r--.  1 root root  636 Nov  8 17:44 scgi_params
-rw-r--r--.  1 root root  636 Nov  8 17:44 scgi_params.default
-rw-r--r--.  1 root root  664 Nov  8 17:44 uwsgi_params
-rw-r--r--.  1 root root  664 Nov  8 17:44 uwsgi_params.default
-rw-r--r--.  1 root root 3610 Nov  8 17:44 win-utf
```


### 🌞 Trouver dans le fichier de conf

cat /etc/nginx/nginx.conf | grep "server " -A 10


 cat /etc/nginx/nginx.conf | grep "include"
``` 
include /usr/share/nginx/modules/*.conf;
    include             /etc/nginx/mime.types;
    # See http://nginx.org/en/docs/ngx_core_module.html#include
    include /etc/nginx/conf.d/*.conf;
        include /etc/nginx/default.d/*.conf;
#        include /etc/nginx/default.d/*.conf; 
```

<>
cat /etc/nginx/nginx.conf | grep "user"
```
user nginx;
    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '"$http_user_agent" "$http_x_forwarded_for"';
```



# 3. Déployer un nouveau site web


### 🌞 Créer un site web

```sudo mkdir /var/www/tp1_parc```

```touch index.html```



### 🌞 Gérer les permissions


sudo chmod 777

### 🌞 Adapter la conf NGINX
```sudo nano /etc/nginx/nginx.conf```

supprimer le server {}

puis ```sudo nginx -t```
```
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
```
```sudo nano /etc/nginx/conf.d/tp1_parc.conf```
```server {
    listen       63392;
    server_name  localhost;

    root         /var/www/tp1_parc;
    index        index.html;

    location / {
        try_files $uri $uri/ =404;
    }
}
```
```sudo firewall-cmd --zone=public --add-port=63392/tcp --permanent```

http://10.1.1.101:63392


# III. Monitoring et alerting


### 🌞 Installer Netdata



## 2. Un peu d'analyse de service

```sudo systemctl start netdata```

sudo lsof -i -P -n | grep netdata 