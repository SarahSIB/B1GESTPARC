# Partie II : Serveur de streaming

**Pour le serveur de streaming, on va utiliser un outil vraiment flex qui s'appelle [Jellyfin](https://jellyfin.org/).**

C'est libre, c'est opensource, c'est beau, ça marche super bien, y'a tout hein. On aura une jolie interface web pour écouter notre meilleure musique :d

![Memusic](./img/music.jpg)

⚠️⚠️⚠️ **Sauf si explicitement mentionné, dans cette partie, toutes les manipulations sont à faire sur `music.tp3.b1`.**

- [Partie II : Serveur de streaming](#partie-ii--serveur-de-streaming)
  - [1. Préparation de la machine](#1-préparation-de-la-machine)
  - [2. Installation du service de streaming](#2-installation-du-service-de-streaming)
- [Dependencies resolved.](#dependencies-resolved)
- [Package                                     Architecture             Version                   Repository                      Size](#package-------------------------------------architecture-------------version-------------------repository----------------------size)
- [Transaction Summary](#transaction-summary)
  - [(2/2): epel-release-9-7.el9.noarch.rpm                                                                34 kB/s |  19 kB     00:00](#22-epel-release-9-7el9noarchrpm----------------------------------------------------------------34-kbs---19-kb-----0000)

## 1. Préparation de la machine

🌞 **Exécution du script `autoconfig.sh` développé à la partie I**

[yumee@music opt]$ sudo ./autoscript.sh
10:13:48 [INFO] DEMARRAGE DU SCRIPT
10:13:48 [INFO] le script est en root
10:13:48 [WARN] SELinux est encore activé
10:13:48 [INFO] Désactivation de SELinux temporaire (setenforce)
10:13:48 [INFO] Désactivation de SELinux définitive (fichier de config)
10:13:48 [INFO] Le firewall est déjà activé
10:13:48 [INFO] SSH n'écoute pas sur le port 22
10:13:48 [INFO] yumee est déjà dans le groupe wheel
10:13:48 [INFO] FIN DU SCRIPT


🌞 **Création d'un dossier où on hébergera les fichiers de musique**
[yumee@music opt]$ sudo mkdir /srv/music
[sudo] password for yumee:
Sorry, try again.
[sudo] password for yumee:
[yumee@music opt]$ ls /srv/
music

🌞 **Déposez quelques fichiers son là dedans**
PS C:\Users\sarah> scp -P 10559 "C:\Users\sarah\Music\Placebo - This Picture (Official Music Video).mp3" yumee@10.3.1.11:/tmp/
yumee@10.3.1.11's password:
Placebo - This Picture (Official Music Video).mp3
PS C:\Users\sarah> scp -P 10559 "C:\Users\sarah\Music\Placebo - This Picture (Official Music Video).mp3" yumee@10.3.1.11:/srv/music
yumee@10.3.1.11's password:
Placebo - This Picture (Official Music Video).mp3

PS C:\Users\sarah> scp -P 10559 "C:\Users\sarah\Music\HIDEAWAY.mp3" yumee@10.3.1.11:/tmp/
yumee@10.3.1.11's password:
HIDEAWAY.mp3

[yumee@music ~]$ sudo mv /tmp/HIDEAWAY.mp3 /srv/music/
[yumee@music ~]$ ls -al /srv/music/
total 9812
drwxr-xr-x. 2 root  root       83 Jan 15 11:21  .
drwxr-xr-x. 3 root  root       19 Jan 15 10:25  ..
-rw-r--r--. 1 yumee yumee 4899071 Jan 15 11:18  HIDEAWAY.mp3
-rw-r--r--. 1 yumee yumee 5141217 Jan 15 10:57 'Placebo - This Picture (Official Music Video).mp3'

## 2. Installation du service de streaming

🌞 **Ajoutez les dépôts nécessaires pour installer Jellyfin**

[yumee@music ~]$ sudo dnf install --nogpgcheck https://mirrors.rpmfusion.org/nonfree/el/rpmfusion-nonfree-release-$(rpm -E %rhel).noarch.rpm -y
sudo dnf config-manager --set-enabled crb
Rocky Linux 9 - BaseOS                                                                               9.2 kB/s | 4.1 kB     00:00
Rocky Linux 9 - BaseOS                                                                               1.8 MB/s | 2.3 MB     00:01
Rocky Linux 9 - AppStream                                                                             11 kB/s | 4.5 kB     00:00
Rocky Linux 9 - AppStream                                                                            1.9 MB/s | 8.4 MB     00:04
Rocky Linux 9 - Extras                                                                               7.9 kB/s | 2.9 kB     00:00
Rocky Linux 9 - Extras                                                                                14 kB/s |  16 kB     00:01
rpmfusion-nonfree-release-9.noarch.rpm                                                                25 kB/s |  10 kB     00:00
Dependencies resolved.
=====================================================================================================================================
 Package                                     Architecture             Version                   Repository                      Size
=====================================================================================================================================
Installing:
 rpmfusion-nonfree-release                   noarch                   9-1                       @commandline                    10 k
Installing dependencies:
 epel-release                                noarch                   9-7.el9                   extras                          19 k
 rpmfusion-free-release                      noarch                   9-1                       extras                         9.2 k

Transaction Summary
=====================================================================================================================================
Install  3 Packages

Total size: 38 k
Total download size: 28 k
Installed size: 34 k
Downloading Packages:
(1/2): rpmfusion-free-release-9-1.noarch.rpm                                                          43 kB/s | 9.2 kB     00:00
(2/2): epel-release-9-7.el9.noarch.rpm                                                                34 kB/s |  19 kB     00:00
-------------------------------------------------------------------------------------------------------------------------------------
Total                                                                                                 34 kB/s |  28 kB     00:00
Running transaction check
Transaction check succeeded.
Running transaction test
Transaction test succeeded.
Running transaction
  Preparing        :                                                                                                             1/1
  Installing       : epel-release-9-7.el9.noarch                                                                                 1/3
  Running scriptlet: epel-release-9-7.el9.noarch                                                                                 1/3
Many EPEL packages require the CodeReady Builder (CRB) repository.
It is recommended that you run /usr/bin/crb enable to enable the CRB repository.

  Installing       : rpmfusion-free-release-9-1.noarch                                                                           2/3
  Installing       : rpmfusion-nonfree-release-9-1.noarch                                                                        3/3
  Running scriptlet: rpmfusion-nonfree-release-9-1.noarch                                                                        3/3
  Verifying        : epel-release-9-7.el9.noarch                                                                                 1/3
  Verifying        : rpmfusion-free-release-9-1.noarch                                                                           2/3
  Verifying        : rpmfusion-nonfree-release-9-1.noarch                                                                        3/3

Installed:
  epel-release-9-7.el9.noarch            rpmfusion-free-release-9-1.noarch            rpmfusion-nonfree-release-9-1.noarch

Complete!

🌞 **Installer le paquet `jellyfin`**

- sudo dnf install jellyfin 

🌞 **Lancer le service `jellyfin`**

[yumee@music ~]$ sudo systemctl start jellyfin
[sudo] password for yumee:

🌞 **Afficher la liste des ports TCP en écoute**
[yumee@music ~]$ sudo ss -lnpt
State      Recv-Q     Send-Q          Local Address:Port            Peer Address:Port     Process
LISTEN     0          128                   0.0.0.0:10559                0.0.0.0:*         users:(("sshd",pid=1383,fd=3))
LISTEN     0          512                   0.0.0.0:8096                 0.0.0.0:*         users:(("jellyfin",pid=12783,fd=310))
LISTEN     0          128                      [::]:10559                   [::]:*         users:(("sshd",pid=1383,fd=4))

[yumee@music ~]$ sudo ss -lntp | grep jellyfin
LISTEN 0      512          0.0.0.0:8096       0.0.0.0:*    users:(("jellyfin",pid=12783,fd=310))

🌞 **Ouvrir le port derrière lequel Jellyfin écoute**

[yumee@music ~]$ sudo firewall-cmd --permanent --add-port=8096/tcp
success
[yumee@music ~]$ sudo firewall-cmd --reload
success
🌞 **Visitez l'interface Web !**

- depuis ton ordi, ton OS (pas la VM) t'ouvres un navigateur, et tu visites 'http://10.3.1.11:8096'
- **paf ! Jellyfin**
  - tu devrais pouvoir configurer le dossier où est la musique à la première connexion
  - te créer un user et un mot de passe étou
  - **puis écouter du son !** 🎵🎵🎵
[yumee@music ~]$  curl http://10.3.1.11:8096/web/index.html
<!doctype html><html>