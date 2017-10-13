# Cahier Ubuntu



## Table des matières

[TOC]



## avertissement.

- Les options qui suivent les commandes ne sont pas exhaustives, pensez à aller voir la doc manpage (ou google...). J'en mets dans certains cas pour illustrer certaines des possibilités.
- Les sections ne sont pas exhaustives, elles ont pour objectifs de répondre à des questions aux quelles n'importe quel utilisateur sera confonté tôt ou tard.



## Contributeurs.

Nicolas Jäger, jagernicolas@legtux.org

*note :* n'hésitez pas à contribuer!



## Les utilisateurs.

Les utilisateurs permettent de discriminer les personnes utilisant le serveur. Lors de l'installation un premier utilisateur est créé, les autres devront être fait post-installation.

##### créer un utilisateur :

```shell
$ sudo adduser toto
```

*note :* il sera demandé durant le processus, d'entrer des informations telles que le mot de passe à associer au compte, le nom de la personne, son numéro de téléphone... Les champs autres que le mot de passe peuvent-être ignorés.

il existe une autre commande qui permet de créer un utilisateur,

```shell
$ sudo useradd -m -g groupe1 -G groupe2,groupe3 -s /bin/bash toto
```

*note :* `-g groupe1` est le groupe initial, `-G groupe2,groupe3` sont des groupes secondaires, `-s /bin/bash` est le shell qui sera utilisé au moment de la connection, `toto` est le nom de l'utilisateur.

Cette commande ne vous demandera pas de configurer le mot de passe.

##### changer ou configurer un mot de passe :

```shell
$ sudo passwd toto
```

*note :* si on oublie le nom d'utilisateur, le mot de passe édité sera celui de root.

##### créer plusieurs utilisateurs :

créer un nouveau fichier, par exemple utilisateurs.txt, dans lequel vous entrer les données suivantes :

```shell
#username:passwd:uid:gid:full name:home_dir:shell
toto:12345:1002:1007:Toto Exemple:/home/toto:/bin/bash
# etc.
```

*notes :*

- la première ligne est un commentaire est peut donc être ignorée. 
- `toto` est le `username`.
- `12345` est le `passwd`.
- `1002` est le `uid` (id de l'utilisateur). Si vide un nouveau sera créé.
- `1007` est le `gid` (id du group). Si vide un nouveau sera créé.
- `Toto Exemple` est le `full name`.
- `/home/toto` est le `home_dir`
- `/bin/bash` est le `shell`

##### desactiver un utilisateur :

```shell
$ sudo usermod --expiredate 1 toto
```

##### réactiver un utilisateur :

```shell
$ sudo usermod --expiredate "" toto
```

##### effacer un utilisateur :

```shell
$ sudo userdel -r
```

*note :* `-r` signifie que le dossier de travail sera effacé.

##### augmenter la sécurité des comptes utilisateurs :

ouvrez le fichier suivant,

```shell
$ sudo nano /etc/pam.d/common-password
```

ajoutez à la fin de la ligne suivante, l'option `minlen=8` pour forcer une taille minimum du mot de passe,

```shell
password [success=1 default=ignore] pam_unix.so obscure sha512
```

ajoutez aussi la ligne suivante :

```shell
password requisite pam_cracklib.so ucredit=-1 lcredit=-1 dcredit=-1 ocredit=-1
```

description :

- ucredit=-1, impose d'avoir au moins une majuscule.
- lcredit=-1, impose d'avoir au moins une minuscule.
- dcredit=-1, impose d'avoir au moins un chiffre.
- ocredit=-1, impose d'avoir au moins une charactère spécial.



## Les groupes.

Les groupes permettent de grouper des utilisateurs partageant des droits.

##### créer un groupe :

```shell
$ sudo addgroup groupe1
```

##### ajouter un utilisateur à un groupe :

```shell
$ sudo adduser toto groupe1
```

##### modifier le groupe initial d'un utilisateur :

```shell
$ sudo usermod -g groupe2 toto
```

##### ajouter des groupes secondaires à un utilisateur :

```shell
$ sudo usermod -a -G groupe2,group3,group4 toto
```

*note :* sans `-a` les groupes seront remplacés.

##### retirer un utilisateur d'un groupe :

```shell
$ deluser toto group2
```

##### effacer un groupe :

```shell
$ deluser --group groupe2
```



## Les droits sudo.

sudo permet à un utilisateur d'avoir la capacité à administrer le système totallement ou partiellement.

##### ajouter les droits sudo à un utilisateur :

```shell
$ sudo adduser toto sudo
```

*note :* ceci revient à ajouter toto au groupe sudo.

##### Le fichier /etc/sudoers :

Ce fichier regroupe les règles de droits et d'accès aux commandes sudo.

- `%sudo ALL=(ALL) ALL` indique que tous les utilisateurs du groupe sudo ont accès à la totalité des commandes.
- `alpha ALL=(ALL:ALL)ALL` indique que `alpha` a tous les droits, indépendament du fait d'appartenir au groupe sudo.
- `bravo ALL=(ALL:ALL) /sbin/reboot,/sbin/shutdown` indique que `bravo` est uniquement en mesure d'éxecuter les commandes `/sbin/reboot,/sbin/shutdown` en tant que sudo.
- `charlie myhostname= /sbin/reboot` indique que `charlie` peut uniqment éxecuter `/sbin/reboot` sur l'ordinateur ayant le hostname `myhostname`. Si on remplace `myhostname` par `ALL`, le droit est vrai partout
- `delta myhostname=(echo:admins) ALL` indique que `delta` peut éxecuter toutes les commandes sur `myhostname` et se faire passer pour l'utilisateur `echo` ou le groupe `admins`

*note :* il est important d'éditer le fichier sudoers via la commande visudo, car si vous faites une erreur de syntax, visudo la détectera au moment ou vous quittez l'éditeur. Si vous ignorez cette erreur, ou si vous aviez édité ce fichier via nano directement, et que donc, vous n'avez pas averti de l'erreur; **VOUS NE SEREZ PLUS EN MESURE DE FAIRE SUDO**. si un tel cas devait arriver, vous pouvez redémarer depuis un autre linux et editer en sudo/root le fichier, ou si vous aviez définit un mot de passe pour root (ce que je vous déconseille) vous pourriez vous connecter avec.



## Gérer les paquets (applications et autres).

un paquet contient les fichiers directement associés à une application ou bien encore à une librairie. Il arrive souvent qu'une application est ce qu'on appelle des dépendances, c'est à dire qu'il faut installer des paquets supplémentaires, permettant son fonctionnement.

##### installer des paquets :

```shell
$ sudo apt-get install openssh-server moon-buggy
```

##### désinstaller un paquet :

```sh
$ sudo apt-get remove openssh-server
```

*note :* pour retirer, au moment de la désinstallation, les fichiers de configuration liés à un paquet :

```shell
$ sudo apt-get remove --purge openssh-server
```

##### mettre à jour la base de données contenant la liste des paquets (cache) :

```shell
$ sudo apt-get update
```

*note :* cette commande met à jour la liste en se basant sur les dépôt connus par le système.

##### mettre à jour les paquets :

```shell
$ sudo apt-get upgrade
```

*note :* cette commande ne retire pas les anciens paquets, ni install de nouveaux paquets. Elle ne fait que mettre à jour les paquets déjà présents. En particulier, elle n'installera jamais de nouveau noyau. De même un paquet ayant une nouvelle dépendance, ne sera pas mis à jour, car cette commande ne l'installera pas.

ou bien :

```shell
$ sudo apt-get dist-upgrade
```

*notes :*

- cette commande mettra tout le système à jour et si il y a un besoin d'installer une nouvelle dépendance elle le fera.
- afin de s'assurer que la mise à jour ne brise rien sur le serveur, cette commande devrait dans un premier temps être éxecutée sur un serveur de test.

##### nettoyer son système de paquets inutiles :

```shell
$ sudo apt-get autoremove --purge
```

*notes :* 

- cette commande retire toutes les dépendances inutilles ansi que leur fichiers de configuration. 
- après une mise à jour, il est préférable d'attendre quelques jours afin de s'assurer que tout fonctionne. Dans le cas contraire, il <u>peut être</u> préférable de réinstaller l'ancienne version des paquets qui régleraient la situation.

##### installer un .deb :

```shell
$ sudo dpkg -i /chemin/vers/paquet.deb
```

cette commande permet d'installer un .deb que vous aviez pu télécharger manuellement, ou bien  être une ancienne version d'un paquet toujours sur le systême. Cette commande ne récupère pas les éventuelles dépendances. Pour régler cette situation, il faut utiliser la commande qui réparera les problèmes de dépendances.

##### réparer les problèmes liés aux paquets installés :

```shell
$ sudo apt-get -f install
```

*note :* recherche les éventuelles dépendances manquantes. Si elle n'est pas en mesure de trouver la dépendances dans les dépôts, elle proposera de désinstaller le paquet.



## Gérer les dépôts.

##### où trouver le fichier contenant les dépôts officiels :

```shell
/etc/apt/sources.list
```

##### où trouver les fichiers contenant les autres dépôts :

les autres dépôts sont contenus dans les fichiers .list se trouvant dans le dossier,

```shell
/etc/apt/sources.list.d/
```

##### anatomie d'une entrée du fichier sources.list :

```shell
deb http://us.archive.ubuntu.com/ubuntu/ xenial main restricted
```

description :

- `deb`, indique à apt que les fichiers de ce dépôt sont des binaires. Une autres valeur possible de ce champ est `deb-src`, indiquant que le dépôt contient des sources.
- `http://us.archive.ubuntu.com/ubuntu/`, indique le lien vers le dépôt. Un lien peut parfaitement être un point de montage du système.
- `xenial`, est le nom de la version de ubuntu correspondante au dépôt. Ubuntu 16.04 s'appelle Xenial Xerus.
- `main`, indique la section. Cette section contient les paquets libres maintenus par canonical. Les autres sections sont : `restricted`, contenant les paquets non-libres maintenus par canonical. `universe`, contient des paquets libres maintenus par la communauté. `multiverse`, contient des paquets non-libres maintenus par la communauté.

##### ajouter un dépôt :

```shell
$ sudo sh -c "echo 'deb https://dl.ring.cx/ring-nightly/ubuntu_16.04/ ring main' > /etc/apt/sources.list.d/ring-nightly-main.list"
```

*note :* après l'ajout d'une entrée il est necessaire de mettre à jours le cache.

##### effacer un dépôt ajouté :

trouver le fichier ajouté dans le dossier suivant et l'effacer,

```shell
/etc/apt/sources.list.d/
```

##### ajouter un PPA (Personal Package Archive) :

```shell
$ sudo apt-add-repository ppa:toto/logiciel-de-toto-1.0
```

*notes :*

- le contenu de ces dépôts ne sont contrôlés par personne. Les utilisés peuvent apporter des problèmes de sécurité.
- ajoute un .list dams 

##### où trouver les PPA ?

https://launchpad.net/ubuntu/+ppas

##### effacer un PPA :

de la même façon que, effacer un dépôt non officiel.

##### sauvegarder la liste des paquets installés :

```shell
$ dpkg –get-selections > packages.list
```

##### restaurer des paquets depuis une liste :

il faut au préalable avoir `dselect` installé et ensuite,

```shell
$ sudo dselect update
$ sudo dpkg --set-selections < packages.list
$ sudo apt-get dselect-upgrade
```



## OpenSSH.

version libre du protocole ssh.

##### paquet serveur à installer :

```shell
$ sudo apt-get install openssh-server
```

*note :* ce paquet permet de connecter la machine en créant un serveur ssh.

##### paquet client à installer :

```shell
$ sudo apt-get install openssh-client
```

*notes :* 

- ce paquet permet de se connecter aux machines ayant un serveur ssh
- il est parfaitement possibles d'avoir les paquets serveur et client afin d'être en mesure de se connecter à la machine et d'utiliser la machine pour se connecter à d'autres machines.

##### obtenir des informations sur l'état du  ssh :

```shell
$ systemctl status ssh
```

##### démarer/arrêter/redémarer le service :

```shell
$ sudo system start ssh
$ sudo system stop ssh
$ sudo system restart ssh
```

##### activer/desactiver le service :

l'activation (desactivation) indique au système de (ne pas) démarrer le service ssh à chaque démarage,

```shell
$ sudo systemctl enable ssh
$ sudo systemctl disable ssh
```

##### connection à un serveur :

```shell
$ ssh -p 12345 toto@123.123.123.123
```

description :

- -p 12345, indique que l'on veut se connecter au port 12345 du serveur.
- 123.123.123.123, indique l'addresse du serveur.

##### augmenter la sécurité du serveur ssh :

ouvrir le fichier `/etc/ssh/sshd_config` trouver la ligne `Port 22` et remplacer la valeur du port par un autre (exemple `Port 12345`).

ajoutez :

- `PermitRootLogin no` afin d'empécher toute connection ssh à root.
- `PasswordAuthentication no` afin d'empécher l'utilisation des mots de passe. Notez que dans ce cas il faudra mettre en place un système de clés.
- `AllowUsers toto tata` afin d'empécher les utilisateurs  autres que `toto` ou `tata` à se connecter.
- `AllowGroups sshusers` afin de n'autoriser que les utilisateurs du groupe `sshusers` à se connecter au serveur.

##### générer des clés d'autentification :

```shell
$ ssh-keygen
```

##### location de la clée publique :

```shell
~/.ssh/id_rsa.pub
```

##### copier la clé publique sur le serveur :

```shell
$ ssh-copy-id -i ~/.ssh/id_rsa.pub toto@123.123.123.123
```

note : vous pouvez aussi la copier manuellement. Ouvrez le fichier contenant la clé publique et ajoutez la totalité dans le fichier `~/.ssh/authorized_keys` de l'utilisateur sur le serveur. Notez bien qu'il faut ajouter et non écraser le fichier, sinon vous perdrez les autres clés autorisées.

##### faciliter ses connections ssh :

ajoutez dans le fichier `~.ssh/config`,

```shell
host monserveur
	hostname 123.123.123.123
	port 12345
	user toto
```

puis vous pourrez vous connecter tout simplement,

```shell
$ ssh monserveur
```



## Protéger son serveur avec fail2ban.

fail2ban permet de protéger son serveur contre les attaques dites "*brute force*".

##### installer fail2ban

```shell
$ sudo apt-get install fail2ban
```

##### location du fichier de configuration :

```shell
/etc/fail2ban/jail.conf
```

si vous ne le trouvez pas, vous pouvez copier un exemple,

```shell
$ sudo cp /etc/fail2ban/jail.conf /etc/fail2ban/jail.local
```

##### obtenir des informations sur l'état du service fail2ban :

```shell
$ systemctl status fail2ban
```

##### démarer/arrêter/redémarer le service :

```shell
$ sudo system start fail2ban
$ sudo system stop fail2ban
$ sudo system restart fail2ban
```

##### activer/desactiver le service :

l'activation (desactivation) indique au système de (ne pas) démarrer le daemon ssh à chaque démarage,

```shell
$ systemctl enable fail2ban
$ systemctl disable fail2ban
```

##### configurer fail2ban pour protéger son serveur ssh :

ouvrez le fichier `/etc/fail2ban/jail.conf`est ajoutez les lignes suivantes,

```shell
[ssh]
enable = true
port = 12345
filter = sshd
logpath = /var/log/auth.log
maxretry = 6
```

descirption :

- `[ssh]`, indique le service
- `enable = true`, indique d'activer la surveillance de ce service
- `port = 12345`, indique le port
- `filter = sshd`, filtre associé aux logs
- `logpath = /var/log/auth.log`, chemin vers le log
- `maxretry = 6`, nombre de tentative avant le bannisement



## Sécuriser son réseau avec ufw (uncomplicated firewall).

ufw est un outil de configuration de firewall. Il représente une alternative à iptables. 

##### installer ufw :

```shell
$ sudo apt-get install ufw
```

##### obtenir des informations sur l'état du service ufw :

```shell
$ systemctl status ufw
```

##### démarer/arrêter/redémarer le service :

```shell
$ sudo system start ufw
$ sudo system stop ufw
$ sudo system restart ufw
```

##### activer/desactiver le service :

l'activation (desactivation) indique au système de (ne pas) démarrer le daemon ufw à chaque démarage,

```shell
$ systemctl enable ufw
$ systemctl disable ufw
```

##### autoriser/interdire un port :

```shell
$ sudo ufw allow 22
$ sudo ufw deny 22
```

##### autoriser/interdire un protocol :

```shell
$ sudo ufw allow ssh
$ sudo ufw deny ssh
```

*note :* vous pouvez faire la même chose avec différents protocoles, `http/tcp`, `ftp`, `smtp`, ...

##### autoriser/interdire une ip en spécifiant un port :

```shell
$ sudo ufw allow from 123.123.123.123 to any port 12345
$ sudo ufw deny from 123.123.123.123 to any port 12345
```

##### effacer des règles :

```shell
$ sudo ufw delete allow ssh
```

##### visualiser les règles ajoutées :

```shell
$ sudo ufw show added
```

##### visualiser l énsemble des règles :

```shell
$ sudo ufw status
```

##### visualiser des règles en les numérotant :

```shell
$ sudo ufw status numbered
```

ceci permet par exemple d'effacer une règle avec son numéro,

```shell
$ sudo ufw delete 2
```

##### visualiser les règles pour un domaine de ports :

```shell
$ sudo ufw allow 1234:5678/tcp
```

##### visualiser les règles pour une addresse ip :

```shell
$ sudo ufw allow from 123.123.123.123
```



## contrôler systemd avec systemctl.

nous ne présenterons qu'une desciption sommaire de systemctl. Systemctl permet de contrôler systemd.

##### obtenir des informations sur l'état d'un service :

```shell
$ systemctl status monservice
```

##### démarer/arrêter/redémarer le service :

```shell
$ sudo system start monservice
$ sudo system stop monservice
$ sudo system restart monservice
```

##### activer/desactiver le service :

l'activation (desactivation) indique au système de (ne pas) démarrer le service à chaque démarage,

```shell
$ sudo systemctl enable monservice
$ sudo systemctl disable monservice
```

##### lister les services :

```shell
$ systemctl list-units --type=service
```

vous retournera,

```shell
UNIT                      LOAD   ACTIVE SUB       DESCRIPTION 

dbus.service              loaded active running   D-Bus System Message Bus 
getty@tty1.service        loaded active running   Getty on tty1           
mod-static-nodes.service  loaded active exited    Create list of required st
ldconfig.service          loaded active exited    Rebuild Dynamic Linker Cac
NetworkManager.service    loaded active running   Network Manager
```

description :

- `UNIT`, le nom de l'unité systemd.
- `LOAD`, indique si le fichier de configuration a été parsé. Les configurations marquées comme `loaded`sont convservées en mémoire.
- `ACTIVE`, indique si l'unité est en fonction.
- `SUB`, donne des informations supplémentaires sur l'état de l'unité.
- `DESCRIPTION`, description textuelle de l'unité.

notes :

- l'exemple précédent peut paraître bien différent de ce que vous verrez.
- l'option `--type=service` permet de filtrer les services.

##### afficher les dépendances d'un service :

```shell
$ systemctl list-dependencies monservice
```

##### afficher toutes les unités possibles :

```shell
$ systemctl list-units --all
```



