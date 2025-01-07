# Networking-basics-1

Voici la traduction en français et au format Markdown du curriculum, ainsi qu'un dictionnaire des termes techniques mentionnés :

# Programme [C#24] Fondations v2 - Partie 3

Moyenne : 4,6%

## Badge de projet

Bases du réseau #1

**Amateur**

Par : Sylvain Kalache
Poids : 1

Votre score sera mis à jour au fur et à mesure de votre progression.

## Description

### Ressources

Lire ou regarder :

- Qu'est-ce que localhost
- Qu'est-ce que 0.0.0.0
- Qu'est-ce que le fichier hosts
- Exemples Netcat

man ou help :

- ifconfig
- telnet
- nc
- cut

## Objectifs d'apprentissage

À la fin de ce projet, vous devriez être capable d'expliquer à n'importe qui, sans l'aide de Google :

### Général
- Qu'est-ce que localhost/127.0.0.1
- Qu'est-ce que 0.0.0.0
- Qu'est-ce que /etc/hosts
- Comment afficher les interfaces réseau actives de votre machine

## Exigences

### Général
- Éditeurs autorisés : vi, vim, emacs
- Tous vos fichiers seront interprétés sur Ubuntu 20.04 LTS
- Tous vos fichiers doivent se terminer par une nouvelle ligne
- Un fichier README.md, à la racine du dossier du projet, est obligatoire
- Tous vos fichiers de script Bash doivent être exécutables
- Vos scripts Bash doivent passer Shellcheck (version 0.7.0 via apt-get) sans aucune erreur
- La première ligne de tous vos scripts Bash doit être exactement #!/usr/bin/env bash
- La deuxième ligne de tous vos scripts Bash doit être un commentaire expliquant ce que fait le script

## Tâches

### 0. Changer votre IP domestique
obligatoire

Écrivez un script Bash qui configure un serveur Ubuntu avec les exigences ci-dessous.

Exigences :

- localhost résout vers 127.0.0.2
- facebook.com résout vers 8.8.8.8.

Exemple :

```bash
sylvain@ubuntu$ ping localhost
PING localhost (127.0.0.1) 56(84) bytes of data.
64 bytes from localhost (127.0.0.1): icmp_seq=1 ttl=64 time=0.012 ms
^C
--- localhost ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 0.012/0.012/0.012/0.000 ms
sylvain@ubuntu$
sylvain@ubuntu$ ping facebook.com
PING facebook.com (157.240.11.35) 56(84) bytes of data.
64 bytes from edge-star-mini-shv-02-lax3.facebook.com (157.240.11.35): icmp_seq=1 ttl=63 time=15.4 ms
^C
--- facebook.com ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 15.432/15.432/15.432/0.000 ms
sylvain@ubuntu$
sylvain@ubuntu$ sudo ./0-change_your_home_IP
sylvain@ubuntu$
sylvain@ubuntu$ ping localhost
PING localhost (127.0.0.2) 56(84) bytes of data.
64 bytes from localhost (127.0.0.2): icmp_seq=1 ttl=64 time=0.012 ms
64 bytes from localhost (127.0.0.2): icmp_seq=2 ttl=64 time=0.036 ms
^C
--- localhost ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1000ms
rtt min/avg/max/mdev = 0.012/0.024/0.036/0.012 ms
sylvain@ubuntu$
sylvain@ubuntu$ ping facebook.com
PING facebook.com (8.8.8.8) 56(84) bytes of data.
64 bytes from facebook.com (8.8.8.8): icmp_seq=1 ttl=63 time=8.06 ms
^C
--- facebook.com ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 8.065/8.065/8.065/0.000 ms
```

Dans cet exemple, nous pouvons voir que :

- avant d'exécuter le script, localhost résout vers 127.0.0.1 et facebook.com résout vers 157.240.11.35
- après avoir exécuté le script, localhost résout vers 127.0.0.2 et facebook.com résout vers 8.8.8.8

Si vous exécutez ce script sur une machine que vous continuerez à utiliser, assurez-vous de rétablir localhost à 127.0.0.1. Sinon, beaucoup de choses cesseront de fonctionner !

Repo :

Dépôt GitHub : holbertonschool-network
Répertoire : basics_1
Fichier : 0-change_your_home_IP

0/2 pts

### 1. Afficher les IPs attachées
obligatoire

Écrivez un script Bash qui affiche toutes les adresses IPv4 actives sur la machine sur laquelle il est exécuté.

Exemple :

```bash
sylvain@ubuntu$ ./1-show_attached_IPs | cat -e
10.0.2.15$
127.0.0.1$
sylvain@ubuntu$
```

Évidemment, les IPs affichées peuvent être différentes selon la machine sur laquelle vous exécutez le script.

Notez que nous pouvons voir notre IP localhost :)

Repo :

Dépôt GitHub : holbertonschool-network
Répertoire : basics_1
Fichier : 1-show_attached_IPs

0/1 pt

### 2. Écoute de port sur localhost
obligatoire

Écrivez un script Bash qui écoute sur le port 98 sur localhost.

Terminal 0

Démarrage de mon script.

```bash
sylvain@ubuntu$ sudo ./2-port_listening_on_localhost
```

Terminal 1

Connexion à localhost sur le port 98 en utilisant telnet et en tapant du texte.

```bash
sylvain@ubuntu$ telnet localhost 98
Trying 127.0.0.2...
Connected to localhost.
Escape character is '^]'.
Hello world
test
```

Terminal 0

Réception du texte de l'autre côté.

```bash
sylvain@ubuntu$ sudo ./2-port_listening_on_localhost
Hello world
test
```

Pour les besoins de l'exercice, cette connexion est entièrement établie au sein de localhost. Ce n'est pas vraiment excitant en soi, mais nous pouvons utiliser ce script à travers les réseaux également. Essayez de l'exécuter entre votre PC local et votre serveur distant pour vous amuser !

Comme vous pouvez le voir, cela peut s'avérer très utile dans une multitude de situations. Peut-être que vous déboguez des problèmes de connexion socket, ou que vous essayez de vous connecter à un logiciel et vous n'êtes pas sûr si le problème vient du logiciel ou du réseau, ou que vous travaillez sur des règles de pare-feu... Un autre outil à ajouter à votre boîte à outils de débogage !

Repo :

Dépôt GitHub : holbertonschool-network
Répertoire : basics_1
Fichier : 2-port_listening_on_localhost

0/1 pt

## Dictionnaire des termes techniques

1. **localhost** : Nom d'hôte qui se réfère à l'ordinateur local. Généralement associé à l'adresse IP 127.0.0.1.

2. **127.0.0.1** : Adresse IP de bouclage (loopback) qui pointe vers l'ordinateur local.

3. **0.0.0.0** : Adresse IP non routée qui a plusieurs significations selon le contexte, souvent utilisée pour représenter "toutes les adresses IP" sur la machine locale.

4. **/etc/hosts** : Fichier système qui mappe les noms d'hôtes aux adresses IP.

5. **Netcat (nc)** : Utilitaire réseau pour lire et écrire des données à travers des connexions réseau.

6. **ifconfig** : Commande pour configurer les interfaces réseau.

7. **telnet** : Protocole réseau utilisé sur Internet ou les réseaux locaux pour fournir une communication bidirectionnelle en mode texte.

8. **cut** : Commande Unix pour couper des sections de lignes de fichiers.

9. **IPv4** : Version 4 du protocole Internet, utilisant des adresses 32 bits.

10. **Shellcheck** : Outil d'analyse statique pour les scripts shell.

11. **Bash** : Bourne Again SHell, un interpréteur de commandes Unix.

12. **Ubuntu** : Distribution Linux basée sur Debian.

13. **LTS** : Long Term Support, version d'un logiciel bénéficiant d'un support à long terme.

14. **ping** : Utilitaire pour tester l'accessibilité d'un hôte sur un réseau IP.

15. **ICMP** : Internet Control Message Protocol, utilisé pour envoyer des messages d'erreur et des informations opérationnelles.

16. **IP** : Internet Protocol, protocole de communication de base pour Internet.

17. **Socket** : Point de terminaison d'une communication bidirectionnelle entre deux programmes s'exécutant sur un réseau.

18. **Port** : Point de terminaison de communication dans un système d'exploitation.

19. **Pare-feu** : Système de sécurité réseau qui surveille et contrôle le trafic réseau entrant et sortant.

Citations:
[1] https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/29334386/eb50ae0c-c3d0-4fc3-b69c-841575b9fb5f/paste.txt
