## Voici le script
````bash
!/bin/bash


log() {
    echo "$(date '+%H:%M:%S') [INFO] $1"
}
log_warn() {
    echo "$(date '+%H:%M:%S') [WARN] $1"
}
# Démarrage du script
log "Le script d'autoconfiguration a démarré."

# Vérifie si le script est exécuté en tant que root
if [ "$EUID" -ne 0 ]; then
    echo " Ca doit etre lancé en root frérot. Réassaye."
    exit 1
fi
log "Le script a bien été lancé en root."

# Vérifie et désactive SELinux si nécessaire
if ! sestatus | grep -q "Current mode: permissive"; then
    log_warn "SELinux est toujours activé !"
    log "Désactivation de SELinux temporaire"
    setenforce 0
    log "Désactivation de SELinux définitive"
    sed -i 's/^SELINUX=enforcing/SELINUX=permissive/' /etc/selinux/config
fi

# Vérifie si le firewall est activé
if ! systemctl is-active --quiet firewalld; then
    echo "[ERROR] Le service firewalld n'est pas actif. Quitte."
    exit 1
fi
log "Service de firewalling firewalld est activé."

# Vérifie et modifie le port SSH si nécessaire
ssh_port=$(ss -tuln | grep ssh | awk -F: '{print $2}')
if [ "$ssh_port" == "22" ]; then
    new_port=$((1025 + RANDOM % 64510))
    log_warn "Le service SSH tourne toujours sur le port 22/TCP"
    log "Modification du fichier de configuration SSH pour écouter sur le port $new_port/TCP"
    sed -i "s/^#\?Port 22/Port $new_port/" /etc/ssh/sshd_config
    systemctl restart sshd
    log "Redémarrage du service SSH"
    firewall-cmd --permanent --add-port=$new_port/tcp
    firewall-cmd --permanent --remove-port=22/tcp
    firewall-cmd --reload
    log "Ouverture du port $new_port/TCP dans firewalld"
else
    log "Le service SSH ne tourne pas sur le port 22/TCP"
fi

current_hostname=$(hostnamectl --static)
if [ "$current_hostname" == "localhost" ] && [ -n "$1" ]; then
    log_warn "La machine s'appelle toujours localhost !"
    log "Changement du nom pour $1"
    hostnamectl set-hostname "$1"
else
    log "Le nom de la machine est déjà configuré : $current_hostname"
fi

# Ajoute l'utilisateur "admin" au groupe "wheel" si nécessaire
admin_user="fariasgomes"
if ! id -nG "$admin_user" | grep -qw "wheel"; then
    log_warn "L'utilisateur $admin_user n'est pas dans le groupe wheel !"
    log "Ajout de l'utilisateur $admin_user au groupe wheel"
    usermod -aG wheel "$admin_user"
else
    log "L'utilisateur $admin_user appartient déjà au groupe wheel"
fi
````

# 1. Préparation de la machine

## 🌞 Exécution du script autoconfig.sh développé à la partie I

Préparation de la machine

sudo ./autoconfig.sh music.tp3.b1

## 🌞 Création d'un dossier où on hébergera les fichiers de musique

sudo mkdir -p /srv/music

## 🌞 Déposez quelques fichiers son là dedans

scp /chemin/vers/tes/fichiers/*.mp3 utilisateur@IP_VM:/srv/music/

# 2. Installation du service de streaming

## 🌞 Ajoutez les dépôts nécessaires pour installer Jellyfin

```sudo dnf install --nogpgcheck https://mirrors.rpmfusion.org/nonfree/el/rpmfusion-nonfree-release-$(rpm -E %rhel).noarch.rpm -y
sudo dnf config-manager --set-enabled crb
```

## 🌞 Installer le paquet jellyfin

sudo dnf install jellyfin -y

## 🌞 Afficher la liste des ports TCP en écoute

sudo ss -tuln | grep jellyfin

## 🌞 Ouvrir le port derrière lequel Jellyfin écoute

sudo firewall-cmd --add-port=8096/tcp --permanent
sudo firewall-cmd --reload

## 🌞 Visitez l'interface Web !

curl http://IP_VM:8096 



