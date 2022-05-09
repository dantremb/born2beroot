Voici mes notes pour Born2beRoot

-------------------------ligne-de-commande------------------------
UFW :		sudo ufw allow "program"
			sudo ufw allow "port"
			sudo ufw status numbered
			sudo ufw delete (rule number)

SSH	:		systemctl status ssh
			sudo service ssh status
			/etc/ssh/sshd_config
			ssh "user"@127.0.0.1 -p 4242

SUDO:		su
			sudo visudo

USER:		sudo adduser "user"

PASSWORD:	chage -l "user"
			sudo passwd "user"
			/etc/pam.d/common-password

GROUPS:		getent group "group name"
			groups "user"
			sudo groupadd "group name"
			sudo usermod -aG "group" "user"

SYSTEM:		sudo systemctl restart "service"
			uname -a
			hostname
			sudo hostnamectl set_hostname "nouveau nom"
			/etc/hosts   ---> edit 127.0.0.1 "old hostname"
			lsblk
			free -m
			df -Bg
			ss
			cat /proc/stat
			lscpu
			sudo crontab -e -u root
			apt-get update	
			apt-get upgrade

-----------------------------  BONUS  ----------------------------

MARIADB:	sudo mysql
			show database;
			CREATE DATABASE "database";
			CREATE USER 'database'@'localhost' IDENTIFIED BY 'password';
			GRANT ALL PRIVILEGES ON wordpress.* TO 'wordpress'@'localhost';
			FLUSH PRIVILEGES; (non-obligatoire avec GRANT)
			quit;

LIGHTTPD:	systemctl status lighttpd
			/etc/lighttpd/lighttpd.conf
				- edit ---> server.document-root        = "/var/www"

PHP:		sudo systemctl status php7.3-fpm   <-- votre version
			/etc/php/7.4/fpm/php.ini
				- edit ---> cgi.fix_pathinfo=1
			/etc/php/7.4/fpm/pool.d/www.conf
				- edit ---> "bin-path" => "/usr/bin/php-cgi",
							"socket" => "/var/run/lighttpd/php.socket"
				- to   ---> "host" => "127.0.0.1",
							"port" => "9000",

WORDPRESS:	/var/www/wordpress/wp-config.php
				- edit ---> define( 'DB_NAME', 'database' )
							define( 'DB_USER', 'user' )
							define( 'DB_PASSWORD', 'password' )

FAIL2BAN:	sudo systemctl status fail2ban
			/etc/fail2ban/jail.local
				- edit ---> enabled  = true
							maxretry = 3
							findtime = 10m
							bantime  = 1d
							port     = 4242
			sudo fail2ban-client status
			sudo fail2ban-client status sshd
			sudo tail -f /var/log/fail2ban.log

----------------------------  DEFENSE  ---------------------------

1. Comment fonctionne une Machine virtuelle
	- 	Créé une partition virtuel ou meme physique pour installer un
		OS sur celui ci qui pourra être ouvert dans une fenetre de
		notre OS courant. On lui donne une partie de la puissance de
		notre Machine.

2. Pourquoi avoir choisi CentOs ou Debian
	-	Debian est un meilleur choix pour le devellopement a cause
		qu'il supporte plusieurs architecture et qu'il contient
		beaucoups de packages créé par une communauté active.

3. La principal différence entre CentOs et Debian
	-	Centos est de classe industrielle donc les grandes compagnie
		ce tourne vers celui-ci pour sont support de 10 ans fait
		par Red-Hat qui est la version payantes de CentOs.
		Debian lui est mis a jour régulierement et donc demande plus
		de travail de mise a jour mais est parfait pour le
		développement pour rester a jour sur les nouvelles
		technologies.

4. Le but de la Machine Virtuelle
	-	Une des utilité est de pouvoir tester les logiciels en conception 
		dans un environement fermé. Alors si le programme cause des
		proglème au OS il est plus facile de diagnostiquer les problèmes
		et on n'altère pas l'intégrité de notre OS principal.
		Et pour les compagnies il peuvent séparé la puissance d'une
		machine pour plusieurs server sur des architecture différente
		sans a devoir ajouter des éléments physique.

5. Différence entre APT et Aptitude
	-	Ils sont tous les 2 des gestionaires de packets mais Aptitude
		est une amélioration de apt avec un menu textuel et quelque
		automatisation supplémentaire.

6. Qu'est ce que APPArmor
	-	APPArmor est un logiciel de sécurité qui associe un profil de
		sécurité a chaque programme qui restreint les accès au OS
		Il utilise les chemins d'accès qui est sa principal différence
		avec SELINUX qui a besoin des attribut étendus.

7. Vérifier si le script affiche le message au 10 minutes

8. Se connecter avec un User qui ne doit pas être root

9. Le mot de passe doit correspondre au règles demandé

10. Afficher le status UFW
	-	sudo ufw status numbered

11. Afficher le status SSH
	-	systemctl status ssh

12. Afficher la disro linux
	-	uname -a

13. Afficher si un utilisateur avec notre login est présent sur
	la machine et qu'il appartient au groupe sudo et user42
	-	groups "user"

14. Vérifier si le mot de passe de cette utilisateur est 
	conforme au règle aussi
	-	chage -l "user"

15. Créé un nouvelle utilisateur et choisir un mot de passe
	celon les regles en place.
	-	sudo adduser "user"

16. Expliquer comment on peut changer les règles de mot de passe
	-	/etc/pam.d/common-password

17. Créé un nouveau groupe nommé "evaluating" et ajouter le
	nouvel utilisateur et vérifier si il est présent
	-	sudo addgroup evaluating
	-	sudo usermod -aG evaluating "user"
	-	getent group evaluating

18. Afficher le hostname qui est ""user"42
	-	hostname

19. Changer le hostname, redémarrer, vérifier a nouveau, si tous
	fonctionne on change a nouveau pour "user"42 et on redémarre
	-	sudo hostnamectl set_hostname "nouveau nom"
	-	/etc/hosts   ---> edit 127.0.0.1 "old hostname"

20. Afficher les partitions
	-	lsblk

21. Qu'est ce que LVM et quel est sont utilité.
	- **Voir plus bas**

22. Véfifier si SUDO est installé
	-	sudo

23. Ajouter le nouvelle utilisateur dans le groupe SUDO
	-	sudo usermod -aG sudo "user"

24. Qu'est ce que SUDO et son utilité.
	-	Un programme qui donne les droits d'un utilisateur
		a un autre utilisateur.

25. Faire un exemple d'utilisation de sudo 
	(Exemple : installer un programme)

26. Véfifier que /var/log/sudo existe avec un fichier a l'intérieur

27. Vérifier que les commandes sudo sont présente

28. Qu'est ce que UFW et sont utilité
	-	Un outlils de configuration en ligne de commande pour la
		configuration d'un pare-feu

29. Afficher les régles de UFW
	-	sudo ufw status numbered

30. Ajouter une règle pour ouvrir le port 8080
	-	sudo ufw allow 8080

31. Véfifier si la règle est active

32. Supprimer la règle 8080
	-	sudo ufw delete (rule number)
34. Qu'est ce que SSH et son utilité
	-	Le SSH permet la connexion d'une machine distante (serveur) 
	via une liaison sécurisée dans le but de transférer des fichiers
	ou des commandes en toute sécurité.

35. Afficher le status SSH sur le port 4242 seulement
	-	sudo service ssh status

36. Se connecter depuis le Mac sur la machine virtuelle a 
l'aide de SSH
	-	ssh dantremb@127.0.0.1 -p 4242

37. On ne doit pas pouvoir se connecter avec l'utilisateur ROOT
	-	ssh root@127.0.0.1 -p 4242

38. Qu'est ce que CRON
	-	Cron est une horloge ou l'on peut insserer notre ligne de
		commande a être executé selon la periode demandé.

39. Comment fonctionne le script
	-	lscpu = info sur les processeurs
	-	free -m = affiche les infos memmoire (-m en megaoctets)
	-	df -Bg = affiche espace disponible sur les disques
	-	ss = analyseur de socket pour le reseau
	-	cat /proc/stat = fichier de statistic du processeur

	-	grep = retourne les lignes avec un mots choisi
	-	awk = transforme les lignes en mot ou $1 est le premier mot
	-	echo = imprimme a l'écran le message

39. Comment le faire démarrer au 10 minutes
	-	sudo crontab -e -u root
	-	ajouter le temp et le script
	-	on ajoute wall qui est un programme qui envoi le message
		sur le terminal.

40. Retirer la règle de démarrer au Startup puis redémarer la
	machine. Le script doit toujours être présent sur la machine
	mais n'a pas démarrer.

Bonus.	MARIADB:	Un systeme de gestion de base de données libre
					identique a mySQL. Il on le même créateur.
					My était son 1e enfant et Maria sont 2e :D

		LIGHTTPD:	Un server web HTML léger et rapide

		PHP:		Hypertext Preprocessor : c'est un langage qui
					a pour but de générer des pages web HTML ou
					autre pour seulement envoyer la page final aux
					utilisateur. Donc le code source est protégés.

		WORDPRESS:	est un système de gestion de contenu open-source
					pour créé du contenu sur le web facilement. 43%
					des sites sont créé a partir de wordpress.

		FAIL2BAN:	une application cherchant des tentatives répétées 
					de connexions infructueuses et procède à un ban 
					en ajoutant une règle au pare-feu iptables pour 
					bannir l'adresse IP de la source.

------------------------------  LVM  -----------------------------
LVM est une méthode et un logiciel de gestion de l'utilisation des 
espaces de stockage d'un ordinateur. Il permet de gérer, sécuriser 
et optimiser de manière souple les espaces de stockage en ligne dans 
les systèmes d'exploitation de type UNIX.

Voici des partitions que l'on peut créé qui sont utilisé par les
apps sous linux.

/swap    :  Lorque l'on à plus de mémoire RAM on peut utiliser une
			partition swap pour complèter la mémoire RAM. Utiliser
			lors de travail sur de gros fichier qui dépasse la mémoire
			RAM installé sur la machine.

/(root)  :  La partition qui contient le système d'exploitation en
			plus du dossier de l'utilisateur ROOT.

/home    : 	La partition qui contient les dossiers utilisateur.
			À chaque utilisateur créé un dossier du meme nom est
			créé sur la partition /home/<username>.

/srv 	 :  Partition utilisé pour les scripts et données de server.

/tmp     :	Partition pour stocker des données temporaire que certaines
			programmes utilise.

/var	 :	Partition qui contient des fichiers dans lesquels le 
			système écrit des données au cours de son fonctionnement.

			/car/cache	: Cache des programmes.

			/var/games	: Données relatives au jeux.

			/var/lib	: Bibliothèques de données dynamiques

			/var/lock	: Contient les fichiers de verrouillage
						  créés par des programmes pour indiquer
						  qu'ils utilisent un fichier.

			/var/log	: contient les fichiers journaux

			/var/run	: Contient les informations valide jusqu'au
						  redémarage du systmème.

			/var/spool	: Espaces pour les nouvelles, files d'attente 
					  	  d'impression et autre files d'attente.

			/var/mail	: contient les fichiers concernant les
						  server de messagerie. Une partition séparé
						  est conseillé car la majeure partie du
						  système continuera de fonctionner même si 
						  vous êtes spammé.
