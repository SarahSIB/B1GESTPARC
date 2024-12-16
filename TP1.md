
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

