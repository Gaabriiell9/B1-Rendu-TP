## 2. Let's go

### 🌞 Afficher la quantité d'espace disque disponible
```bash
df -h / | grep "/dev" |tr -s ' ' | cut -d ' ' -f4

26G
```

### 🌞 Afficher combien de fichiers il est possible de créer

```bash
date "+%d/%m/%y %H:%M:%S"

09/12/24 16:45:50
```

### 🌞 Afficher la version de l'OS précise
```
cat /etc/os-release | grep "NAME" | tr -s ' ' | cut -d= -f2 | head -n 1
```

### 🌞 Afficher la version du kernel en cours d'utilisation précise
```
uname -r 
5.14.0-503.15.1.el9_5.aarch64
```
### 🌞 Afficher le chemin vers la commande python3
```
which python3 
/usr/bin/python3
```

## 🌞 Afficher l'utilisateur actuellement connecté
```
env |grep "USER" | tr -s ' ' | cut -d'=' -f2
fariasgomes
```
## 🌞 Afficher le shell par défaut de votre utilisateur actuellement connecté

Shell par défaut :

cat /etc/passwd | grep "^$USER:" | cut -d':' -f7

## 🌞 Afficher le nombre de paquets installés

Sur un système RPM comme Rocky Linux :

rpm -qa | wc -l

## 🌞 Afficher le nombre de ports en écoute

Nombre de ports ouverts :

ss -tunlp | grep LISTEN | wc -l
