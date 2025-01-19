#!/bin/bash

TIME=$(date '+%H:%M:%S')

echo "$TIME [INFO] DEMARRAGE DU SCRIPT"

if [[ "$(id -u)" != 0 ]]; then
    echo "$TIME [ERROR] Il faut exécuter ce script en tant que root"
    exit
else
    echo "$TIME [INFO] le script est en root"
fi

if [[ "$SELINUX_STATUS" != "permissive" ]]; then
    echo "$TIME [WARN] SELinux est encore activé "
    echo "$TIME [INFO] Désactivation de SELinux temporaire (setenforce)"
    setenforce 0
    echo "$TIME [INFO] Désactivation de SELinux définitive (fichier de config)"
    sed -i 's/SELINUX=enforcing/SELINUX=permissive/' /etc/selinux/config
else
    echo "$TIME [INFO] SELinux est déjà en mode permissive."
fi

FIREWALL=$(firewall-cmd --state)
if [[ "$FIREWALL" != "running" ]]; then
    echo "$TIME [ERROR] Le firewall est pas activé. actives le."
    exit
else
    echo "$TIME [INFO] Le firewall est déjà activé"

    fi

SSH_PORT=$(ss -tuln | grep ':22 ')
if [[ -n "$SSH_PORT" ]]; then
    echo "$TIME [WARN] Le service SSH tourne toujours sur le port 22/TCP"
    NEW_PORT=$((RANDOM % 64511 + 1025))
    echo "$TIME [INFO] Modification du fichier de configuration SSH pour écouter sur le port $NEW_PORT/TCP"
    sed -i "s/Port 22/Port $NEW_PORT/" /etc/ssh/sshd_config
    sed -i "s/#Port 22/Port $NEW_PORT/" /etc/ssh/sshd_config
    echo "$TIME [INFO] Redémarrage du service SSH"
    systemctl restart sshd
    firewall-cmd --zone=public --add-port=$NEW_PORT/tcp --permanent
    firewall-cmd --zone=public --remove-port=22/tcp --permanent
    firewall-cmd --reload
    echo "$TIME [INFO] Ouverture du port $NEW_PORT/TCP dans firewalld"
else
    echo "$TIME [INFO] SSH n'écoute pas sur le port 22"
fi

CURRENT_HOSTNAME=$(hostname)
if [[ "$CURRENT_HOSTNAME" == "localhost" ]]; then
    NEW_HOSTNAME="backup.tp3"
    echo "$TIME [WARN] le nom de la machine est encore localhost !"
    echo "$TIME [INFO] Changement du nom pour $NEW_HOSTNAME"
    if hostnamectl set-hostname "$NEW_HOSTNAME"; then
      echo "$TIME [INFO] Le nom de la machine a été modifié en : $NEW_HOSTNAME"
    else
      echo "$TIME [ERROR] Le nom de la machine n'a pas été modifié."
      exit 1
    fi
    echo "$TIME [INFO] Le nom de la machine est déjà : $CURRENT_HOSTNAME"
fi

IS_WHEEL=$(groups yumee | grep -o wheel)
if [[ "$IS_WHEEL" != "wheel" ]]; then
    echo "$TIME [WARN] yumee n'est pas dans le groupe wheel !"
    echo "$TIME [INFO] yumee vient d'être ajouté au groupe wheel"
    usermod -aG wheel yumee
    echo "$TIME [INFO] yumee est désormais dans le groupe wheel"
else
    echo "$TIME [INFO] yumee est déjà dans le groupe wheel"
fi

echo "$TIME [INFO] FIN DU SCRIPT"


14:07:09 [INFO] DEBUT DU SCRIPT
14:07:09 [INFO] le script est en root
14:07:09 [WARN] SELinux est encore activé
14:07:09 [INFO] Désactivation de SELinux temporaire (setenforce)
14:07:09 [INFO] Désactivation de SELinux définitive (fichier de config)
14:07:09 [INFO] Le firewall est déjà activé
14:07:09 [INFO] SSH n'écoute pas sur le port 22
14:07:09 [INFO] Le nom de la machine est déjà : backup.tp3
14:07:09 [INFO] yumee est déjà dans le groupe wheel
14:07:09 [INFO] FIN DU SCRIPT