 ## Partie I : Des beaux one liner

1. Intro
[yumee@node1 ~]$ free -mh | grep Mem | tr -s ' ' | cut -d' ' -f7
1.3Gi

2. Let's go
🌞 Afficher la quantité d'espace disque disponible
[yumee@node1 ~]$ df -h | grep rl_vbox | tr -s ' ' | cut -d' ' -f4
5.7G

🌞 Afficher combien de fichiers il est possible de créer

[yumee@node1 ~]$ df -i | grep rl_vbox | tr -s ' ' | cut -d' ' -f4
3913887

🌞 Afficher l'heure et la date
[yumee@node1 ~]$ date "+%D %T"
01/09/25 21:06:29

🌞 Afficher la version de l'OS précise
[yumee@node1 ~]$ cat /etc/os-release | grep PRETTY | cut -d'"' -f2
Rocky Linux 9.5 (Blue Onyx)

🌞 Afficher la version du kernel en cours d'utilisation précise
[yumee@node1 ~]$ uname -r
5.14.0-503.14.1.el9_5.x86_64

🌞 Afficher le chemin vers la commande python3
[yumee@node1 ~]$ which python3
/usr/bin/python3

🌞 Afficher l'utilisateur actuellement connecté
[yumee@node1 ~]$ echo $USER
yumee

🌞 Afficher le shell par défaut de votre utilisateur actuellement connecté
[yumee@node1 ~]$ cat /etc/passwd | grep "$USER" |cut -d':' -f7
/bin/bash

🌞 Afficher le nombre de paquets installés
[yumee@node1 ~]$ rpm -qa | wc -l
445

🌞 Afficher le nombre de ports en écoute
[yumee@node1 ~]$ ss -tuln | wc -l
11

## Partie II : Un premier ptit script
2.premier pas scripting

🌞 Ecrire un script qui produit exactement l'affichage demandé 