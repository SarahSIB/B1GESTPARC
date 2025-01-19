
## 1. Analyse du service

🌞 **S'assurer que le service `sshd` est démarré**

- avec une commande `systemctl status`
  [yumee@web ~]$ systemctl status
● web.tp1.b1
    State: running
    Units: 285 loaded (incl. loaded aliases)
     Jobs: 0 queued
   Failed: 0 units
    Since: Fri 2024-11-29 16:18:20 CET; 1h 5min ago
  systemd: 252-46.el9_5.2.0.1
   CGroup: /
           ├─init.scope
           │ └─1 /usr/lib/systemd/systemd --switched-root --system --deserialize 31
           ├─system.slice
           │ ├─NetworkManager.service
           │ │ └─700 /usr/sbin/NetworkManager --no-daemon
           │ ├─auditd.service
           │ │ └─642 /sbin/auditd
           │ ├─chronyd.service
           │ │ └─696 /usr/sbin/chronyd -F 2
           │ ├─crond.service
           │ │ └─735 /usr/sbin/crond -n
           │ ├─dbus-broker.service
           │ │ ├─672 /usr/bin/dbus-broker-launch --scope system --audit
           │ │ └─673 dbus-broker --log 4 --controller 9 --machine-id 378284796ace4565b6bde1d33686ab2f --max-bytes 53687>
           │ ├─firewalld.service
           │ │ └─676 /usr/bin/python3 -s /usr/sbin/firewalld --nofork --nopid
           │ ├─rsyslog.service
           │ │ └─789 /usr/sbin/rsyslogd -n
           │ ├─sshd.service
           │ │ └─782 "sshd: /usr/sbin/sshd -D [listener] 0 of 10-100 startups"
           │ ├─systemd-journald.service
lines 1-29

> *Vous pouvez utiliser des commandes comme `sudo systemctl list-units -t service -a` 
>
[yumee@web ~]$ sudo systemctl list-units -t service -a
[sudo] password for yumee:
  UNIT                                   LOAD      ACTIVE   SUB     DESCRIPTION                                                     >
  auditd.service                         loaded    active   running Security Auditing Service
● autofs.service                         not-found inactive dead    autofs.service
  chronyd.service                        loaded    active   running NTP client/server
  crond.service                          loaded    active   running Command Scheduler
  dbus-broker.service                    loaded    active   running D-Bus System Message Bus
● display-manager.service                not-found inactive dead    display-manager.service
  dm-event.service                       loaded    inactive dead    Device-mapper event daemon
  dnf-makecache.service                  loaded    inactive dead    dnf makecache
  dracut-cmdline.service                 loaded    inactive dead    dracut cmdline hook
  dracut-initqueue.service               loaded    inactive dead    dracut initqueue hook
  dracut-mount.service                   loaded    inactive dead    dracut mount hook
  dracut-pre-mount.service               loaded    inactive dead    dracut pre-mount hook
  dracut-pre-pivot.service               loaded    inactive dead    dracut pre-pivot and cleanup hook
  dracut-pre-trigger.service             loaded    inactive dead    dracut pre-trigger hook
  dracut-pre-udev.service                loaded    inactive dead    dracut pre-udev hook
  dracut-shutdown-onfailure.service      loaded    inactive dead    Service executing upon dracut-shutdown failure to perform cleanup
  dracut-shutdown.service                loaded    active   exited  Restore /run/initramfs on shutdown
● ebtables.service                       not-found inactive dead    ebtables.service
  emergency.service                      loaded    inactive dead    Emergency Shell
  firewalld.service                      loaded    active   running firewalld - dynamic firewall daemon
  getty@tty1.service                     loaded    active   running Getty on tty1
  initrd-cleanup.service                 loaded    inactive dead    Cleaning Up and Shutting Down Daemons
  initrd-parse-etc.service               loaded    inactive dead    Mountpoints Configured in the Real Root
  initrd-switch-root.service             loaded    inactive dead    Switch Root
  initrd-udevadm-cleanup-db.service      loaded    inactive dead    Cleanup udev Database
● ip6tables.service                      not-found inactive dead    ip6tables.service
● ipset.service                          not-found inactive dead    ipset.service
● iptables.service                       not-found inactive dead    iptables.service
  irqbalance.service                     loaded    inactive dead    irqbalance daemon
  kdump.service                          loaded    active   exited  Crash recovery kernel arming
  kmod-static-nodes.service              loaded    active   exited  Create List of Static Device Nodes
lines 1-32

🌞 **Analyser les processus liés au service SSH**

[yumee@web ~]$ ps aux | grep sshd

root         782  0.0  0.5  16796  9472 ?        Ss   16:18   0:00 sshd: /usr/sbin/sshd -D [listener] 0 of 10-100 startups
root       14862  0.0  0.6  20156 11520 ?        Ss   17:20   0:00 sshd: yumee [priv]
yumee      14866  0.0  0.4  20356  7272 ?        S    17:20   0:00 sshd: yumee@pts/1
yumee      14903  0.0  0.1   6408  2432 pts/1    S+   17:40   0:00 grep --color=auto sshd


> Il est possible de repérer le numéro des processus liés à un service avec la commande `systemctl status sshd`
> [yumee@web ~]$ systemctl status sshd
● sshd.service - OpenSSH server daemon
     Loaded: loaded (/usr/lib/systemd/system/sshd.service; enabled; preset: enabled)
     Active: active (running) since Fri 2024-11-29 16:18:29 CET; 1h 23min ago
       Docs: man:sshd(8)
             man:sshd_config(5)
   Main PID: 782 (sshd)
      Tasks: 1 (limit: 11083)
     Memory: 4.5M
        CPU: 386ms
     CGroup: /system.slice/sshd.service
             └─782 "sshd: /usr/sbin/sshd -D [listener] 0 of 10-100 startups"

Nov 29 16:18:29 localhost.localdomain sshd[782]: Server listening on :: port 22.
Nov 29 16:18:29 localhost.localdomain systemd[1]: Started OpenSSH server daemon.
Nov 29 16:34:05 vbox sshd[14576]: Accepted password for yumee from 192.168.56.1 port 51522 ssh2
Nov 29 16:34:05 vbox sshd[14576]: pam_unix(sshd:session): session opened for user yumee(uid=1000) by yumee(uid=0)
Nov 29 17:12:53 vbox sshd[14749]: Accepted password for yumee from 10.1.1.3 port 51655 ssh2
Nov 29 17:12:53 vbox sshd[14749]: pam_unix(sshd:session): session opened for user yumee(uid=1000) by yumee(uid=0)
Nov 29 17:18:21 webtp1b1 sshd[14821]: Accepted password for yumee from 10.1.1.3 port 51660 ssh2
Nov 29 17:18:21 webtp1b1 sshd[14821]: pam_unix(sshd:session): session opened for user yumee(uid=1000) by yumee(uid=0)
Nov 29 17:20:38 web.tp1.b1 sshd[14862]: Accepted password for yumee from 10.1.1.3 port 51663 ssh2
Nov 29 17:20:38 web.tp1.b1 sshd[14862]: pam_unix(sshd:session): session opened for user yumee(uid=1000) by yumee(uid=0)

🌞 **Déterminer le port sur lequel écoute le service SSH**

[yumee@web ~]$ sudo ss -alnpt | grep ssh
[sudo] password for yumee:
LISTEN 0      128          0.0.0.0:22        0.0.0.0:*    users:(("sshd",pid=782,fd=3))
LISTEN 0      128             [::]:22           [::]:*    users:(("sshd",pid=782,fd=4))

🌞 **Consulter les logs du service SSH**

- les logs du service sont consultables avec une commande `journalctl`
  - donnez une commande `journalctl` qui permet de consulter les logs du service SSH[yumee@web ~]$ journalctl -xe -u sshd
~
Nov 29 16:18:29 localhost.localdomain systemd[1]: Starting OpenSSH server daemon...
░░ Subject: A start job for unit sshd.service has begun execution
░░ Defined-By: systemd
░░ Support: https://wiki.rockylinux.org/rocky/support
░░
░░ A start job for unit sshd.service has begun execution.
░░
░░ The job identifier is 219.
Nov 29 16:18:29 localhost.localdomain sshd[782]: Server listening on 0.0.0.0 port 22.
Nov 29 16:18:29 localhost.localdomain sshd[782]: Server listening on :: port 22.
Nov 29 16:18:29 localhost.localdomain systemd[1]: Started OpenSSH server daemon.
░░ Subject: A start job for unit sshd.service has finished successfully
░░ Defined-By: systemd
░░ Support: https://wiki.rockylinux.org/rocky/support
░░
░░ A start job for unit sshd.service has finished successfully.
░░
░░ The job identifier is 219.
Nov 29 16:34:05 vbox sshd[14576]: Accepted password for yumee from 192.168.56.1 port 51522 ssh2
Nov 29 16:34:05 vbox sshd[14576]: pam_unix(sshd:session): session opened for user yumee(uid=1000) by yumee(uid=0)
Nov 29 17:12:53 vbox sshd[14749]: Accepted password for yumee from 10.1.1.3 port 51655 ssh2
Nov 29 17:12:53 vbox sshd[14749]: pam_unix(sshd:session): session opened for user yumee(uid=1000) by yumee(uid=0)
Nov 29 17:18:21 webtp1b1 sshd[14821]: Accepted password for yumee from 10.1.1.3 port 51660 ssh2
Nov 29 17:18:21 webtp1b1 sshd[14821]: pam_unix(sshd:session): session opened for user yumee(uid=1000) by yumee(uid=0)
Nov 29 17:20:38 web.tp1.b1 sshd[14862]: Accepted password for yumee from 10.1.1.3 port 51663 ssh2
Nov 29 17:20:38 web.tp1.b1 sshd[14862]: pam_unix(sshd:session): session opened for user yumee(uid=1000) by yumee(uid=0)

## 
- AUSSI, il existe un fichier de log, dans lequel le service SSH enregistre toutes les tentatives de connexion
[yumee@web ~]$ sudo tail -n 10 /var/log/secure
Nov 29 18:24:08 vbox sudo[14948]: pam_unix(sudo:session): session closed for user root
Nov 29 18:25:46 vbox sudo[14953]:   yumee : TTY=pts/1 ; PWD=/home/yumee ; USER=root ; COMMAND=/bin/tail -n 10 /var/log
Nov 29 18:25:46 vbox sudo[14953]: pam_unix(sudo:session): session opened for user root(uid=0) by yumee(uid=1000)
Nov 29 18:25:46 vbox sudo[14953]: pam_unix(sudo:session): session closed for user root
Nov 29 18:25:49 vbox sudo[14956]:   yumee : TTY=pts/1 ; PWD=/home/yumee ; USER=root ; COMMAND=/bin/tail -n 10 /var/log/auth.log
Nov 29 18:25:49 vbox sudo[14956]: pam_unix(sudo:session): session opened for user root(uid=0) by yumee(uid=1000)
Nov 29 18:25:49 vbox sudo[14956]: pam_unix(sudo:session): session closed for user root
Nov 29 18:26:43 vbox sudo[14961]:   yumee : TTY=pts/1 ; PWD=/home/yumee ; USER=root ; COMMAND=/bin/journalctl -u sshd -n 10
Nov 29 18:26:43 vbox sudo[14961]: pam_unix(sudo:session): session opened for user root(uid=0) by yumee(uid=1000)
Nov 29 18:26:43 vbox sudo[14961]: pam_unix(sudo:session): session closed for user roo

## 2. Modification du service

Dans cette section, on va aller visiter et modifier le fichier de configuration du serveur SSH.

Comme tout fichier de configuration, celui de SSH se trouve dans le dossier `/etc/`.

Plus précisément, il existe un sous-dossier `/etc/ssh/` qui contient toute la configuration relative à SSH spécifiquement

🌞 **Identifier le fichier de configuration du serveur SSH**
[yumee@web ~]$ cd /etc/ssh/
[yumee@web ssh]$ ls
moduli      ssh_config.d  sshd_config.d       ssh_host_ecdsa_key.pub  ssh_host_ed25519_key.pub  ssh_host_rsa_key.pub
ssh_config  sshd_config   ssh_host_ecdsa_key  ssh_host_ed25519_key    ssh_host_rsa_key
[yumee@web ssh]$ ls -l sshd_config
-rw-------. 1 root root 3667 Nov  5 04:37 sshd_config


🌞 **Modifier le fichier de conf**

- exécutez un `echo $RANDOM` pour **demander à votre shell de vous fournir un nombre aléatoire**
[yumee@web ssh]$ echo $RANDOM
26841*
- **changez le port d'écoute du serveur SSH** pour qu'il écoute sur ce numéro de port
  - [yumee@web ssh]$ sudo nano /etc/ssh/sshd_config
  - [yumee@web ssh]$ sudo cat /etc/ssh/sshd_config | grep Port
Port 26841
#GatewayPorts no
- **gérer le firewall**
 [yumee@web ssh]$ sudo firewall-cmd --permanent --remove-port=22/tcp
[sudo] password for yumee:
Warning: NOT_ENABLED: 22:tcp
success
[yumee@web ssh]$ sudo firewall-cmd --reload
success
[yumee@web ssh]$ sudo firewall-cmd --permanent --add-port=26841/tcp
sudo firewall-cmd --reload
success
success
  - vérifier avec un `firewall-cmd --list-all` que le port est bien ouvert
 [yumee@web ssh]$ sudo firewall-cmd --list-all
public (active)
  target: default
  icmp-block-inversion: no
  interfaces: enp0s3 enp0s8
  sources:
  services: cockpit dhcpv6-client ssh
  ports: 26841/tcp
  protocols:
  forward: yes
  masquerade: no
  forward-ports:
  source-ports:
  icmp-blocks:
  rich rules:
    - vous filtrerez la sortie de la commande avec un `| grep TEXTE`
  rich rules:
[yumee@web ssh]$ sudo firewall-cmd --list-all | grep port
  ports: 26841/tcp
  forward-ports:
  source-ports:*9*-
🌞 **Redémarrer le service**
[yumee@web ssh]$[yumee@web ssh]$ systemctl status sshd
● sshd.service - OpenSSH server daemon
     Loaded: loaded (/usr/lib/systemd/system/sshd.service; enabled; preset: enabled)
     Active: active (running) since Fri 2024-11-29 23:50:21 CET; 24s ago
       Docs: man:sshd(8)
             man:sshd_config(5)
   Main PID: 15277 (sshd)
      Tasks: 1 (limit: 11083)
     Memory: 1.4M
        CPU: 8ms
     CGroup: /system.slice/sshd.service
             └─15277 "sshd: /usr/sbin/sshd -D [listener] 0 of 10-100 startups"

Nov 29 23:50:21 web.tp1.b1 systemd[1]: Starting OpenSSH server daemon...
Nov 29 23:50:21 web.tp1.b1 sshd[15277]: Server listening on 0.0.0.0 port 26841.
Nov 29 23:50:21 web.tp1.b1 sshd[15277]: Server listening on :: port 26841.
Nov 29 23:50:21 web.tp1.b1 systemd[1]: Started OpenSSH server daemon.
[yumee@web ssh]$ sudo ss -lnpt
State      Recv-Q     Send-Q         Local Address:Port          Peer Address:Port     Process
LISTEN     0          128                  0.0.0.0:26841              0.0.0.0:*         users:(("sshd",pid=15277,fd=3))
LISTEN     0          128                     [::]:26841                 [::]:*         users:(("sshd",pid=15277,fd=4))

🌞 **Effectuer une connexion SSH sur le nouveau port**
PS C:\Users\sarah> ssh -p 26841 yumee@10.1.1.1
yumee@10.1.1.1's password:
Last login: Sat Nov 30 00:06:56 2024 from 10.1.1.3
[yumee@web ~]$ sudo systemctl restart sshd
[sudo] password for yumee:
[yumee@web ~]$ sudo firewall-cmd -- permanent --remove-port=26841/tcp
usage: 'firewall-cmd --help' for usage information or see firewall-cmd(1) man page
firewall-cmd: error: unrecognized arguments: -- permanent --remove-port=26841/tcp
[yumee@web ~]$ sudo firewall-cmd --permanent --remove-port=26841/tcp
success
[yumee@web ~]$ sudo firewall-cmd --permanent --add-port=22/tcp
success
[yumee@web ~]$ sudo firewall-cmd --reload
success
[yumee@web ~]$ exit
logout
Connection to 10.1.1.1 closed.
PS C:\Users\sarah> ssh yumee@10.1.1.1
yumee@10.1.1.1's password:
Last login: Sat Nov 30 00:08:41 2024 from 10.1.1.3






## 1. Mise en place
🌞 **Installer le serveur NGINX**

- il faut faire une commande `dnf install`
  [yumee@web ~]$ dnf search nginx
Rocky Linux 9 - BaseOS                                                                      2.1 MB/s | 2.3 MB     00:01
Rocky Linux 9 - AppStream                                                                   4.9 MB/s | 8.3 MB     00:01
Rocky Linux 9 - Extras                                                                       23 kB/s |  16 kB     00:00
=============================================== Name Exactly Matched: nginx ================================================
nginx.x86_64 : A high performance web server and reverse proxy server
============================================== Name & Summary Matched: nginx ===============================================
nginx-all-modules.noarch : A meta package that installs all available Nginx modules
nginx-core.x86_64 : nginx minimal core
nginx-filesystem.noarch : The basic directory layout for the Nginx server
nginx-mod-http-image-filter.x86_64 : Nginx HTTP image filter module
nginx-mod-http-perl.x86_64 : Nginx HTTP perl module
nginx-mod-http-xslt-filter.x86_64 : Nginx XSLT module
nginx-mod-mail.x86_64 : Nginx mail modules
nginx-mod-stream.x86_64 : Nginx stream modules
pcp-pmda-nginx.x86_64 : Performance Co-Pilot (PCP) metrics for the Nginx Webserver

[yumee@web ~]$ sudo dnf install nginx
[sudo] password for yumee:
Rocky Linux 9 - BaseOS                                                                       11 kB/s | 4.1 kB     00:00
Rocky Linux 9 - AppStream                                                                    12 kB/s | 4.5 kB     00:00
Rocky Linux 9 - Extras                                                                      8.0 kB/s | 2.9 kB     00:00
Dependencies resolved.
============================================================================================================================
 Package                          Architecture          Version                              Repository                Size
============================================================================================================================
Installing:
 nginx                            x86_64                2:1.20.1-20.el9.0.1                  appstream                 36 k
Installing dependencies:
 nginx-core                       x86_64                2:1.20.1-20.el9.0.1                  appstream                566 k
 nginx-filesystem                 noarch                2:1.20.1-20.el9.0.1                  appstream                8.4 k
 rocky-logos-httpd                noarch                90.15-2.el9                          appstream                 24 k

Transaction Summary
============================================================================================================================
Install  4 Packages

Total download size: 634 k
Installed size: 1.8 M
Is this ok [y/N]: y
Downloading Packages:
(1/4): nginx-filesystem-1.20.1-20.el9.0.1.noarch.rpm                                         71 kB/s | 8.4 kB     00:00
(2/4): rocky-logos-httpd-90.15-2.el9.noarch.rpm                                             123 kB/s |  24 kB     00:00
(3/4): nginx-1.20.1-20.el9.0.1.x86_64.rpm                                                   158 kB/s |  36 kB     00:00
(4/4): nginx-core-1.20.1-20.el9.0.1.x86_64.rpm                                              2.0 MB/s | 566 kB     00:00
----------------------------------------------------------------------------------------------------------------------------
Total                                                                                       925 kB/s | 634 kB     00:00
Running transaction check
Transaction check succeeded.
Running transaction test
Transaction test succeeded.
Running transaction
  Preparing        :                                                                                                    1/1
  Running scriptlet: nginx-filesystem-2:1.20.1-20.el9.0.1.noarch                                                        1/4
  Installing       : nginx-filesystem-2:1.20.1-20.el9.0.1.noarch                                                        1/4
  Installing       : nginx-core-2:1.20.1-20.el9.0.1.x86_64                                                              2/4
  Installing       : rocky-logos-httpd-90.15-2.el9.noarch                                                               3/4
  Installing       : nginx-2:1.20.1-20.el9.0.1.x86_64                                                                   4/4
  Running scriptlet: nginx-2:1.20.1-20.el9.0.1.x86_64                                                                   4/4
  Verifying        : rocky-logos-httpd-90.15-2.el9.noarch                                                               1/4
  Verifying        : nginx-filesystem-2:1.20.1-20.el9.0.1.noarch                                                        2/4
  Verifying        : nginx-2:1.20.1-20.el9.0.1.x86_64                                                                   3/4
  Verifying        : nginx-core-2:1.20.1-20.el9.0.1.x86_64                                                              4/4

Installed:
  nginx-2:1.20.1-20.el9.0.1.x86_64      nginx-core-2:1.20.1-20.el9.0.1.x86_64  nginx-filesystem-2:1.20.1-20.el9.0.1.noarch
  rocky-logos-httpd-90.15-2.el9.noarch

Complete!
🌞 **Démarrer le service NGINX**

[yumee@web ~]$ sudo systemctl start nginx
[sudo] password for yumee:

🌞 **Déterminer sur quel port tourne NGINX**

- vous devez filtrer la sortie de la commande utilisée pour n'afficher que les lignes demandées
[yumee@web ~]$ sudo ss -tulnp
Netid State  Recv-Q Send-Q Local Address:Port  Peer Address:Port Process
udp   UNCONN 0      0          127.0.0.1:323        0.0.0.0:*     users:(("chronyd",pid=696,fd=5))
udp   UNCONN 0      0              [::1]:323           [::]:*     users:(("chronyd",pid=696,fd=6))
tcp   LISTEN 0      128          0.0.0.0:22         0.0.0.0:*     users:(("sshd",pid=15416,fd=3))
tcp   LISTEN 0      511          0.0.0.0:80         0.0.0.0:*     users:(("nginx",pid=15665,fd=6),("nginx",pid=15664,fd=6))
tcp   LISTEN 0      128             [::]:22            [::]:*     users:(("sshd",pid=15416,fd=4))
tcp   LISTEN 0      511             [::]:80            [::]:*     users:(("nginx",pid=15665,fd=7),("nginx",pid=15664,fd=7))
[yumee@web ~]$ sudo ss -tulnp | grep nginx
tcp   LISTEN 0      511          0.0.0.0:80        0.0.0.0:*    users:(("nginx",pid=15665,fd=6),("nginx",pid=15664,fd=6))
tcp   LISTEN 0      511             [::]:80           [::]:*    users:(("nginx",pid=15665,fd=7),("nginx",pid=15664,fd=7))

- ouvrez le port concerné dans le firewall
  
[yumee@web ~]$ sudo firewall-cmd --permanent --add-port=80/tcp
[sudo] password for yumee:
Sorry, try again.
[sudo] password for yumee:
success
[yumee@web ~]$ sudo firewall-cmd --reload
success



🌞 **Déterminer les processus liés au service NGINX**

[yumee@web ~]$ ps aux | grep nginx
root       15664  0.0  0.0  11292  1592 ?        Ss   01:10   0:00 nginx: master process /usr/sbin/nginx
nginx      15665  0.0  0.2  15532  5304 ?        S    01:10   0:00 nginx: worker process
yumee      15702  0.0  0.1   6408  2304 pts/0    S+   01:35   0:00 grep --color=auto nginx

🌞 **Déterminer le nom de l'utilisateur qui lance NGINX**

[yumee@web ~]$ cat /etc/passwd | grep nginx
nginx:x:996:993:Nginx web server:/var/lib/nginx:/sbin/nologin

🌞 **Test !**

[yumee@web ~]$ curl http://10.1.1.1:80 | head -n 7
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0<!doctype html>
<html>
  <head>
    <meta charset='utf-8'>
    <meta name='viewport' content='width=device-width, initial-scale=1'>
    <title>HTTP Server Test Page powered by: Rocky Linux</title>
    <style type="text/css">
100  7620  100  7620    0     0   531k      0 --:--:-- --:--:-- --:--:--  572k

## 2. Analyser la conf de NGINX

🌞 **Déterminer le path du fichier de configuration de NGINX**
[yumee@web ~]$ ls -al /etc/nginx/
total 84
drwxr-xr-x.  4 root root 4096 Nov 30 00:28 .
drwxr-xr-x. 82 root root 8192 Dec 15 19:11 ..
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

🌞 *[yumee@web ~]$ cat /etc/nginx/nginx.conf | grep 'server {' -A 10
    server {
        listen       80;
        listen       [::]:80;
        server_name  _;
        root         /usr/share/nginx/html;

        # Load configuration files for the default server block.
        include /etc/nginx/default.d/*.conf;

        error_page 404 /404.html;
        location = /404.html {
--
#    server {
#        listen       443 ssl http2;
#        listen       [::]:443 ssl http2;
#        server_name  _;
#        root         /usr/share/nginx/html;
#
#        ssl_certificate "/etc/pki/nginx/server.crt";
#        ssl_certificate_key "/etc/pki/nginx/private/server.key";
#        ssl_session_cache shared:SSL:1m;
#        ssl_session_timeout  10m;
#        ssl_ciphers PROFILE=SYSTEM;
[yumee@web ~]$ cat /etc/nginx/nginx.conf | grep 'include'
include /usr/share/nginx/modules/*.conf;
    include             /etc/nginx/mime.types;
    # See http://nginx.org/en/docs/ngx_core_module.html#include
    include /etc/nginx/conf.d/*.conf;
        include /etc/nginx/default.d/*.conf;
#        include /etc/nginx/default.d/*.conf;
[yumee@web ~]$ cat /etc/nginx/nginx.conf | grep 'user'
user nginx;
    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '"$http_user_agent" "$http_x_forwarded_for"
 
## 3. Déployer un nouveau site web

🌞 **Créer un site web**
[yumee@web ~]$ pwd
/home/yumee
[yumee@web ~]$ sudo mkdir /home/yumee/var
[sudo] password for yumee:
[yumee@web ~]$ cd var
[yumee@web var]$ sudo mkdir /home/yumee/var/www
[yumee@web var]$ cd www
[yumee@web www]$ sudo mkdir /home/yumee/var/www/tp1_parc
[yumee@web www]$ cd tp1_parc

[yumee@web tp1_parc]$
🌞 **Gérer les permissions**
[yumee@web tp1_parc]$ cat /etc/nginx/nginx.conf | grep 'yumee'
[yumee@web tp1_parc]$ sudo chown -R nginx:nginx /home/yumee/var/www/tp1_parc
[yumee@web tp1_parc]$ ls -al /home/yumee/var/www/tp1_parc
total 4
drwxr-xr-x. 2 nginx nginx 24 Dec 16 04:11 .
drwxr-xr-x. 3 root  root  22 Dec 16 04:09 ..
-rw-r--r--. 1 nginx nginx 39 Dec 16 04:11 index.html

🌞 **Adapter la conf NGINX**
[yumee@web www]$ ls
index.html
[yumee@web www]$ mkdir tp1_parc
mkdir: cannot create directory ‘tp1_parc’: Permission denied
[yumee@web www]$ sudo mkdir tp1_parc
[yumee@web www]$ cd tp1_parc/
[yumee@web tp1_parc]$
[yumee@web tp1_parc]$ ls
[yumee@web tp1_parc]$ touch index.html
touch: cannot touch 'index.html': Permission denied
[yumee@web tp1_parc]$ sudo touch index.html
[yumee@web tp1_parc]$ nano index.html
[yumee@web tp1_parc]$ [yumee@web tp1_parc]$ sudo nano index.html
[yumee@web tp1_parc]$ sudo cat index.html
[yumee@web tp1_parc]$ sudo nano index.html
[yumee@web tp1_parc]$ sudo cat index.html
azelbazjkebazijebuiazebjazebjazevj
[yumee@web tp1_parc]$ lcd
-bash: lcd: command not found
[yumee@web tp1_parc]$ cd
[yumee@web ~]$
[yumee@web ~]$ ls
var
🌞 **Visitez votre super site web**
[yumee@web ~]$ curl localhost:23045
azelbazjkebazijebuiazebjazebjazevj


## III - MONITORING
1. Installation
curl https://get.netdata.cloud/kickstart.sh > /tmp/netdata-kickstart.sh && sh /tmp/netdata-kickstart.sh --no-updates --stable-channel --disable-telemetry

2. Un peu d'analyse de service
Un service netdata a été créé suite à l'installation.
🌞 Démarrer le service netdata
[yumee@monitoring ~]$ systemctl status netdata
● netdata.service - Real time performance monitoring
     Loaded: loaded (/usr/lib/systemd/system/netdata.service; enabled; preset: enabled)
     Active: active (running) since Wed 2025-01-08 19:15:58 CET; 6min ago
    Process: 761 ExecStartPre=/bin/mkdir -p /var/cache/netdata (code=exited, status=0/SUCCESS)
    Process: 770 ExecStartPre=/bin/chown -R netdata /var/cache/netdata (code=exited, status=0/SUCCESS)
    Process: 774 ExecStartPre=/bin/mkdir -p /run/netdata (code=exited, status=0/SUCCESS)
    Process: 780 ExecStartPre=/bin/chown -R netdata /run/netdata (code=exited, status=0/SUCCESS)
   Main PID: 786 (netdata)
      Tasks: 90 (limit: 11082)
     Memory: 201.3M
        CPU: 11.501s
     CGroup: /system.slice/netdata.service
             ├─ 786 /usr/sbin/netdata -P /run/netdata/netdata.pid -D
             ├─ 834 "spawn-plugins    " "  " "                        " "  "
             ├─1251 bash /usr/libexec/netdata/plugins.d/tc-qos-helper.sh 1
             ├─1260 /usr/libexec/netdata/plugins.d/apps.plugin 1
             ├─1262 /usr/libexec/netdata/plugins.d/debugfs.plugin 1
             ├─1263 /usr/libexec/netdata/plugins.d/ebpf.plugin 1
             ├─1267 /usr/libexec/netdata/plugins.d/go.d.plugin 1
             ├─1273 /usr/libexec/netdata/plugins.d/network-viewer.plugin 1
             ├─1288 /usr/libexec/netdata/plugins.d/systemd-journal.plugin 1
             └─1337 "spawn-setns                                         " " "

Jan 08 19:15:58 monitoring.tp1.b1 systemd[1]: Starting Real time performance monitoring...
Jan 08 19:15:58 monitoring.tp1.b1 systemd[1]: Started Real time performance monitoring.

🌞 Déterminer sur quel port tourne Netdata
[yumee@monitoring ~]$ sudo ss -tlnp | grep netdata
[sudo] password for yumee:
LISTEN 0      4096         0.0.0.0:19999      0.0.0.0:*    users:(("netdata",pid=786,fd=6))
LISTEN 0      4096       127.0.0.1:8125       0.0.0.0:*    users:(("netdata",pid=786,fd=44))
LISTEN 0      4096            [::]:19999         [::]:*    users:(("netdata",pid=786,fd=7))
LISTEN 0      4096           [::1]:8125          [::]:*    users:(("netdata",pid=786,fd=42))
[yumee@monitoring ~]$ sudo firewall-cmd --permanent --add-port=19999/tcp
success
[yumee@monitoring ~]$ sudo firewall-cmd --reload

🌞 Visiter l'interface Web

sarah@LAPTOP-NQC7EJ07 MINGW64 ~
$ curl http://10.1.1.4:19999 | head -n 7
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0<!doctype html><html lang="en" dir="ltr"><head><meta charset="utf-8"/><title>Netdata</title><script>const CONFIG = {
      cache: {
        agentInfo: false,
        cloudToken: true,
        agentToken: true,
      }
    }
  6  106k    6  6929    0     0   204k      0 --:--:-- --:--:-- --:--:--  205k
curl: (23) Failure writing output to destination, passed 1460 returned 1263



3. 
  
🌞 Ajouter un check


[yumee@monitoring ~]$ sudo nano /etc/netdata/health.d/tcpcheck.conf
[sudo] password for yumee:

check:
  name: WEB_web.tp1.b1
  type: tcp
  host: 10.1.1.4
  port: 6789
  timeout seconds: 1

[yumee@monitoring ~]$ sudo systemctl restart netdata

🌞 Ajouter un check

[yumee@monitoring ~]$ sudo nano /etc/netdata/health.d/tcpcheck.conf
[sudo] password for yumee:

check:
  name: SSH_web.tp1.b1
  type: tcp
  host: 10.1.1.4
  port: 9876
  timeout seconds: 1

[yumee@monitoring ~]$ sudo systemctl restart netdata


4. Ajouter des alertes
https://discordapp.com/api/webhooks/1326629149963456642/cmq2kKkj0umS5PqUftReyc-oh-4p8CtffleAlqn7hOKHwNjWt_-TO0HuvSDNeLGBt78L
🌞 Configurer l'alerting avec Discord
m
suivre la doc officielle

[yumee@monitoring ~]$ sudo nano /etc/netdata/health_alarm_notify.conf
[sudo] password for yumee:







[yumee@monitoring ~]$ sudo systemctl restart netdata
🌞 Tester que ça fonctionne

[yumee@monitoring ~]$ sudo /usr/libexec/netdata/plugins.d/alarm-notify.sh test

# SENDING TEST WARNING ALARM TO ROLE: sysadmin
/etc/netdata/health_alarm_notify.conf: line 4: unexpected EOF while looking for matching `"'
/etc/netdata/health_alarm_notify.conf: line 5: syntax error: unexpected end of file
time=2025-01-08T20:30:58.196+01:00 comm=alarm-notify.sh source=health level=error tid=8104 thread=alarm-notify msg_id=6db0018e83e34320ae2a659d78019fb7 node=monitoring.tp1.b1 instance=test.chart alert_id=1 alert_unique_id=1 alert=test_alarm alert_class=Test alert_recipient=sysadmin alert_duration=1 alert_value=100 alert_value_old=90 alert_status=WARNING alert_value_old=CLEAR alert_units=units alert_summary="a test alarm" alert_info="this is a test alarm to verify notifications work" request="'/usr/libexec/netdata/plugins.d/alarm-notify.sh' 'sysadmin' 'monitoring.tp1.b1' '1' '1' '1' '1736364658' 'test_alarm' 'test.chart' 'WARNING' 'CLEAR' '100' '90' '/usr/libexec/netdata/plugins.d/alarm-notify.sh' '1' '1' 'units' 'this is a test alarm to verify notifications work' 'new value' 'old value' 'evaluated expression' 'expression variable values' '0' '0' '' '' 'Test' 'command to edit the alarm=0=monitoring.tp1.b1' '' '' 'a test alarm' " msg="[ALERT NOTIFICATION]: Failed to load config file '/etc/netdata/health_alarm_notify.conf'."
time=2025-01-08T20:30:58.206+01:00 comm=alarm-notify.sh source=health level=error tid=8106 thread=alarm-notify msg_id=6db0018e83e34320ae2a659d78019fb7 node=monitoring.tp1.b1 instance=test.chart alert_id=1 alert_unique_id=1 alert=test_alarm alert_class=Test alert_recipient=sysadmin alert_duration=1 alert_value=100 alert_value_old=90 alert_status=WARNING alert_value_old=CLEAR alert_units=units alert_summary="a test alarm" alert_info="this is a test alarm to verify notifications work" request="'/usr/libexec/netdata/plugins.d/alarm-notify.sh' 'sysadmin' 'monitoring.tp1.b1' '1' '1' '1' '1736364658' 'test_alarm' 'test.chart' 'WARNING' 'CLEAR' '100' '90' '/usr/libexec/netdata/plugins.d/alarm-notify.sh' '1' '1' 'units' 'this is a test alarm to verify notifications work' 'new value' 'old value' 'evaluated expression' 'expression variable values' '0' '0' '' '' 'Test' 'command to edit the alarm=0=monitoring.tp1.b1' '' '' 'a test alarm' " msg="[ALERT NOTIFICATION]: Cannot find sendmail command in the system path. Disabling email notifications."
# FAILED

# SENDING TEST CRITICAL ALARM TO ROLE: sysadmin
/etc/netdata/health_alarm_notify.conf: line 4: unexpected EOF while looking for matching `"'
/etc/netdata/health_alarm_notify.conf: line 5: syntax error: unexpected end of file
time=2025-01-08T20:30:58.272+01:00 comm=alarm-notify.sh source=health level=error tid=8130 thread=alarm-notify msg_id=6db0018e83e34320ae2a659d78019fb7 node=monitoring.tp1.b1 instance=test.chart alert_id=1 alert_unique_id=1 alert=test_alarm alert_class=Test alert_recipient=sysadmin alert_duration=1 alert_value=100 alert_value_old=90 alert_status=CRITICAL alert_value_old=WARNING alert_units=units alert_summary="a test alarm" alert_info="this is a test alarm to verify notifications work" request="'/usr/libexec/netdata/plugins.d/alarm-notify.sh' 'sysadmin' 'monitoring.tp1.b1' '1' '1' '2' '1736364658' 'test_alarm' 'test.chart' 'CRITICAL' 'WARNING' '100' '90' '/usr/libexec/netdata/plugins.d/alarm-notify.sh' '1' '2' 'units' 'this is a test alarm to verify notifications work' 'new value' 'old value' 'evaluated expression' 'expression variable values' '0' '0' '' '' 'Test' 'command to edit the alarm=0=monitoring.tp1.b1' '' '' 'a test alarm' " msg="[ALERT NOTIFICATION]: Failed to load config file '/etc/netdata/health_alarm_notify.conf'."
time=2025-01-08T20:30:58.280+01:00 comm=alarm-notify.sh source=health level=error tid=8132 thread=alarm-notify msg_id=6db0018e83e34320ae2a659d78019fb7 node=monitoring.tp1.b1 instance=test.chart alert_id=1 alert_unique_id=1 alert=test_alarm alert_class=Test alert_recipient=sysadmin alert_duration=1 alert_value=100 alert_value_old=90 alert_status=CRITICAL alert_value_old=WARNING alert_units=units alert_summary="a test alarm" alert_info="this is a test alarm to verify notifications work" request="'/usr/libexec/netdata/plugins.d/alarm-notify.sh' 'sysadmin' 'monitoring.tp1.b1' '1' '1' '2' '1736364658' 'test_alarm' 'test.chart' 'CRITICAL' 'WARNING' '100' '90' '/usr/libexec/netdata/plugins.d/alarm-notify.sh' '1' '2' 'units' 'this is a test alarm to verify notifications work' 'new value' 'old value' 'evaluated expression' 'expression variable values' '0' '0' '' '' 'Test' 'command to edit the alarm=0=monitoring.tp1.b1' '' '' 'a test alarm' " msg="[ALERT NOTIFICATION]: Cannot find sendmail command in the system path. Disabling email notifications."
# FAILED

# SENDING TEST CLEAR ALARM TO ROLE: sysadmin
/etc/netdata/health_alarm_notify.conf: line 4: unexpected EOF while looking for matching `"'
/etc/netdata/health_alarm_notify.conf: line 5: syntax error: unexpected end of file
time=2025-01-08T20:30:58.338+01:00 comm=alarm-notify.sh source=health level=error tid=8156 thread=alarm-notify msg_id=6db0018e83e34320ae2a659d78019fb7 node=monitoring.tp1.b1 instance=test.chart alert_id=1 alert_unique_id=1 alert=test_alarm alert_class=Test alert_recipient=sysadmin alert_duration=1 alert_value=100 alert_value_old=90 alert_status=CLEAR alert_value_old=CRITICAL alert_units=units alert_summary="a test alarm" alert_info="this is a test alarm to verify notifications work" request="'/usr/libexec/netdata/plugins.d/alarm-notify.sh' 'sysadmin' 'monitoring.tp1.b1' '1' '1' '3' '1736364658' 'test_alarm' 'test.chart' 'CLEAR' 'CRITICAL' '100' '90' '/usr/libexec/netdata/plugins.d/alarm-notify.sh' '1' '3' 'units' 'this is a test alarm to verify notifications work' 'new value' 'old value' 'evaluated expression' 'expression variable values' '0' '0' '' '' 'Test' 'command to edit the alarm=0=monitoring.tp1.b1' '' '' 'a test alarm' " msg="[ALERT NOTIFICATION]: Failed to load config file '/etc/netdata/health_alarm_notify.conf'."
time=2025-01-08T20:30:58.347+01:00 comm=alarm-notify.sh source=health level=error tid=8158 thread=alarm-notify msg_id=6db0018e83e3432cipient=sysadmin alert_duration=1 alert_value=100 alert_value_old=90 alert_status=CLEAR alert_value_old=CRITICAL alert_units=units alert_summary="a test alarm" alert_info="this is a test alarm to verify notifications work" request="'/usr/libexec/netdata/plugins.d/alarm-notify.sh' 'sysadmin' 'monitoring.tp1.b1' '1' '1' '3' '1736364658' 'test_alarm' 'test.chart' 'CLEAR' 'CRITICAL' '100' '90' '/usr/libexec/netdata/plugins.d/alarm-notify.sh' '1' '3' 'units' 'this is a test alarm to verify notifications work' 'new value' 'old value' 'evaluated expression' 'expression variable values' '0' '0' '' '' 'Test' 'command to edit the alarm=0=monitoring.tp1.b1' '' '' 'a test alarm' " msg="[ALERT NOTIFICATION]: Cannot find sendmail command in the system path. Disabling email notifications."
# FAILED
[yumee@monitoring ~]$ sudo nano /etc/netdata/health_alarm_notify.conf



[yumee@monitoring ~]$ sudo /usr/libexec/netdata/plugins.d/alarm-notify.sh test

# SENDING TEST WARNING ALARM TO ROLE: sysadmin
time=2025-01-08T20:32:26.599+01:00 comm=alarm-notify.sh source=health level=error tid=8279 thread=alarm-notify msg_id=6db0018e83e34320ae2a659d78019fb7 node=monitoring.tp1.b1 instance=test.chart alert_id=1 alert_unique_id=1 alert=test_alarm alert_class=Test alert_recipient=sysadmin alert_duration=1 alert_value=100 alert_value_old=90 alert_status=WARNING alert_value_old=CLEAR alert_units=units alert_summary="a test alarm" alert_info="this is a test alarm to verify notifications work" request="'/usr/libexec/netdata/plugins.d/alarm-notify.sh' 'sysadmin' 'monitoring.tp1.b1' '1' '1' '1' '1736364746' 'test_alarm' 'test.chart' 'WARNING' 'CLEAR' '100' '90' '/usr/libexec/netdata/plugins.d/alarm-notify.sh' '1' '1' 'units' 'this is a test alarm to verify notifications work' 'new value' 'old value' 'evaluated expression' 'expression variable values' '0' '0' '' '' 'Test' 'command to edit the alarm=0=monitoring.tp1.b1' '' '' 'a test alarm' " msg="[ALERT NOTIFICATION]: Cannot find sendmail command in the system path. Disabling email notifications."
time=2025-01-08T20:32:27.085+01:00 comm=alarm-notify.sh source=health level=info tid=8305 thread=alarm-notify msg_id=6db0018e83e34320ae2a659d78019fb7 node=monitoring.tp1.b1 instance=test.chart alert_id=1 alert_unique_id=1 alert=test_alarm alert_class=Test alert_recipient=sysadmin alert_duration=1 alert_value=100 alert_value_old=90 alert_status=WARNING alert_value_old=CLEAR alert_units=units alert_summary="a test alarm" alert_info="this is a test alarm to verify notifications work" request="'/usr/libexec/netdata/plugins.d/alarm-notify.sh' 'sysadmin' 'monitoring.tp1.b1' '1' '1' '1' '1736364746' 'test_alarm' 'test.chart' 'WARNING' 'CLEAR' '100' '90' '/usr/libexec/netdata/plugins.d/alarm-notify.sh' '1' '1' 'units' 'this is a test alarm to verify notifications work' 'new value' 'old value' 'evaluated expression' 'expression variable values' '0' '0' '' '' 'Test' 'command to edit the alarm=0=monitoring.tp1.b1' '' '' 'a test alarm' " msg="[ALERT NOTIFICATION]: sent discord notification to 'alerts' for notification to 'sysadmin' for transition from CLEAR to WARNING, of alert 'test_alarm' = 'new value', of instance 'test.chart', context '' on host 'monitoring.tp1.b1'"
# OK

# SENDING TEST CRITICAL ALARM TO ROLE: sysadmin
time=2025-0GI1-08T20:32:27.131+01:00 comm=alarm-notify.sh source=health level=error tid=8318 thread=alarm-notify msg_id=6db0018e83e34320ae2a659d78019fb7 node=monitoring.tp1.b1 instance=test.chart alert_id=1 alert_unique_id=1 alert=test_alarm alert_class=Test alert_recipient=sysadmin alert_duration=1 alert_value=100 alert_value_old=90 alert_status=CRITICAL alert_value_old=WARNING alert_units=units alert_summary="a test alarm" alert_info="this is a test alarm to verify notifications work" request="'/usr/libexec/netdata/plugins.d/alarm-notify.sh' 'sysadmin' 'monitoring.tp1.b1' '1' '1' '2' '1736364747' 'test_alarm' 'test.chart' 'CRITICAL' 'WARNING' '100' '90' '/usr/libexec/netdata/plugins.d/alarm-notify.sh' '1' '2' 'units' 'this is a test alarm to verify notifications work' 'new value' 'old value' 'evaluated expression' 'expression variable values' '0' '0' '' '' 'Test' 'command to edit the alarm=0=monitoring.tp1.b1' '' '' 'a test alarm' " msg="[ALERT NOTIFICATION]: Cannot find sendmail command in the system path. Disabling email notifications."
time=2025-01-08T20:32:27.506+01:00 comm=alarm-notify.sh source=health level=info tid=8337 thread=alarm-notify msg_id=6db0018e83e34320ae2a659d78019fb7 node=monitoring.tp1.b1 instance=test.chart alert_id=1 alert_unique_id=1 alert=test_alarm alert_class=Test alert_recigpient=sysadmin alert_duration=1 alert_value=100 alert_value_old=90 alert_status=CRITICAL alert_value_old=WARNING alert_units=units alert_summary="a test alarm" alert_info="this is a test alarm to verify notifications work" request="'/usr/libexec/netdata/plugins.d/alarm-notify.sh' 'sysadmin' 'monitoring.tp1.b1' '1' '1' '2' '1736364747' 'test_alarm' 'test.chart' 'CRITICAL' 'WARNING' '100' '90' '/usr/libexec/netdata/plugins.d/alarm-notify.sh' '1' '2' 'units' 'this is a test alarm to verify notifications work' 'new value' 'old value' 'evaluated expression' 'expression variable values' '0' '0' '' '' 'Test' 'command to edit the alarm=0=monitoring.tp1.b1' '' '' 'a test alarm' " msg="[ALERT NOTIFICATION]: sent discord notification to 'alerts' for notification to 'sysadmin' for transition from WARNING to CRITICAL, of alert 'test_alarm' = 'new value', of instance 'test.chart', context '' on host 'monitoring.tp1.b1'"
# OK

# SENDING TEST CLEAR ALARM TO ROLE: sysadmin
time=2025-01-08T20:32:27.562+01:00 comm=alarm-notify.sh source=health level=error tid=8350 thread=alarm-notify msg_id=6db0018e83e34320ae2a659d78019fb7 node=monitoring.tp1.b1 instance=test.chart alert_id=1 alert_unique_id=1 alert=test_alarm alert_class=Test alert_recipient=sysadmin alert_duration=1 alert_value=100 alert_value_old=90 alert_status=CLEAR alert_value_old=CRITICAL alert_units=units alert_summary="a test alarm" alert_info="this is a test alarm to verify notifications work" request="'/usr/libexec/netdata/plugins.d/alarm-notify.sh' 'sysadmin' 'monitoring.tp1.b1' '1' '1' '3' '1736364747' 'test_alarm' 'test.chart' 'CLEAR' 'CRITICAL' '100' '90' '/usr/libexec/netdata/plugins.d/alarm-notify.sh' '1' '3' 'units' 'this is a test alarm to verify notifications work' 'new value' 'old value' 'evaluated expression' 'expression variable values' '0' '0' '' '' 'Test' 'command to edit the alarm=0=monitoring.tp1.b1' '' '' 'a test alarm' " msg="[ALERT NOTIFICATION]: Cannot find sendmail command in the system path. Disabling email notifications."
time=2025-01-08T20:32:27.948+01:00 comm=alarm-notify.sh source=health level=info tid=8369 thread=alarm-notify msg_id=6db0018e83e34320ae2a659d78019fb7 node=monitoring.tp1.b1 instance=test.chart alert_id=1 alert_unique_id=1 alert=test_alarm alert_class=Test alert_recipient=sysadmin alert_duration=1 alert_value=100 alert_value_old=90 alert_status=CLEAR alert_value_old=CRITICAL alert_units=units alert_summary="a test alarm" alert_info="this is a test alarm to verify notifications work" request="'/usr/libexec/netdata/plugins.d/alarm-notify.sh' 'sysadmin' 'monitoring.tp1.b1' '1' '1' '3' '1736364747' 'test_alarm' 'test.chart' 'CLEAR' 'CRITICAL' '100' '90' '/usr/libexec/netdata/plugins.d/alarm-notify.sh' '1' '3' 'units' 'this is a test alarm to verify notifications work' 'new value' 'old value' 'evaluated expression' 'expression variable values' '0' '0' '' '' 'Test' 'command to edit the alarm=0=monitoring.tp1.b1' '' '' 'a test alarm' " msg="[ALERT NOTIFICATION]: sent discord notification to 'alerts' for notification to 'sysadmin' for transition from CRITICAL to CLEAR, of alert 'test_alarm' = 'new value', of instance 'test.chart', context '' on host 'monitoring.tp1.b1'"
# OK