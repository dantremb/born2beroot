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
			free -m
			df -Bg
			ss
			cat /proc/stat
			lscpu
			sudo crontab -e -u root
			apt-get update	
			apt-get upgrade

HDD:		lsblk
			mkfs [options] [-t type fs-options] device [size]
			(ex: sudo mkfs -t ext4 /dev/sdb1)

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
	- 	Cr???? une partition virtuel ou meme physique pour installer un
		OS sur celui ci qui pourra ??tre ouvert dans une fenetre de
		notre OS courant. On lui donne une partie de la puissance de
		notre Machine.

2. Pourquoi avoir choisi CentOs ou Debian
	-	Debian est un meilleur choix pour le devellopement a cause
		qu'il supporte plusieurs architecture et qu'il contient
		beaucoups de packages cr???? par une communaut?? active.

3. La principal diff??rence entre CentOs et Debian
	-	Centos est de classe industrielle donc les grandes compagnie
		ce tourne vers celui-ci pour sont support de 10 ans fait
		par Red-Hat qui est la version payantes de CentOs.
		Debian lui est mis a jour r??gulierement et donc demande plus
		de travail de mise a jour mais est parfait pour le
		d??veloppement pour rester a jour sur les nouvelles
		technologies.

4. Le but de la Machine Virtuelle
	-	Une des utilit?? est de pouvoir tester les logiciels en conception 
		dans un environement ferm??. Alors si le programme cause des
		progl??me au OS il est plus facile de diagnostiquer les probl??mes
		et on n'alt??re pas l'int??grit?? de notre OS principal.
		Et pour les compagnies il peuvent s??par?? la puissance d'une
		machine pour plusieurs server sur des architecture diff??rente
		sans a devoir ajouter des ??l??ments physique.

5. Diff??rence entre APT et Aptitude
	-	Ils sont tous les 2 des gestionaires de packets mais Aptitude
		est une am??lioration de apt avec un menu textuel et quelque
		automatisation suppl??mentaire.

6. Qu'est ce que APPArmor
	-	APPArmor est un logiciel de s??curit?? qui associe un profil de
		s??curit?? a chaque programme qui restreint les acc??s au OS
		Il utilise les chemins d'acc??s qui est sa principal diff??rence
		avec SELINUX qui a besoin des attribut ??tendus.

7. V??rifier si le script affiche le message au 10 minutes

8. Se connecter avec un User qui ne doit pas ??tre root

9. Le mot de passe doit correspondre au r??gles demand??

10. Afficher le status UFW
	-	sudo ufw status numbered

11. Afficher le status SSH
	-	systemctl status ssh

12. Afficher la disro linux
	-	uname -a

13. Afficher si un utilisateur avec notre login est pr??sent sur
	la machine et qu'il appartient au groupe sudo et user42
	-	groups "user"

14. V??rifier si le mot de passe de cette utilisateur est 
	conforme au r??gle aussi
	-	chage -l "user"

15. Cr???? un nouvelle utilisateur et choisir un mot de passe
	celon les regles en place.
	-	sudo adduser "user"

16. Expliquer comment on peut changer les r??gles de mot de passe
	-	/etc/pam.d/common-password

17. Cr???? un nouveau groupe nomm?? "evaluating" et ajouter le
	nouvel utilisateur et v??rifier si il est pr??sent
	-	sudo addgroup evaluating
	-	sudo usermod -aG evaluating "user"
	-	getent group evaluating

18. Afficher le hostname qui est ""user"42
	-	hostname

19. Changer le hostname, red??marrer, v??rifier a nouveau, si tous
	fonctionne on change a nouveau pour "user"42 et on red??marre
	-	sudo hostnamectl set_hostname "nouveau nom"
	-	/etc/hosts   ---> edit 127.0.0.1 "old hostname"

20. Afficher les partitions
	-	lsblk

21. Qu'est ce que LVM et quel est sont utilit??.
	- **Voir plus bas**

22. V??fifier si SUDO est install??
	-	sudo

23. Ajouter le nouvelle utilisateur dans le groupe SUDO
	-	sudo usermod -aG sudo "user"

24. Qu'est ce que SUDO et son utilit??.
	-	Un programme qui donne les droits d'un utilisateur
		a un autre utilisateur.

25. Faire un exemple d'utilisation de sudo 
	(Exemple : installer un programme)

26. V??fifier que /var/log/sudo existe avec un fichier a l'int??rieur

27. V??rifier que les commandes sudo sont pr??sente

28. Qu'est ce que UFW et sont utilit??
	-	Un outlils de configuration en ligne de commande pour la
		configuration d'un pare-feu

29. Afficher les r??gles de UFW
	-	sudo ufw status numbered

30. Ajouter une r??gle pour ouvrir le port 8080
	-	sudo ufw allow 8080

31. V??fifier si la r??gle est active

32. Supprimer la r??gle 8080
	-	sudo ufw delete (rule number)
34. Qu'est ce que SSH et son utilit??
	-	Le SSH permet la connexion d'une machine distante (serveur) 
	via une liaison s??curis??e dans le but de transf??rer des fichiers
	ou des commandes en toute s??curit??.

35. Afficher le status SSH sur le port 4242 seulement
	-	sudo service ssh status

36. Se connecter depuis le Mac sur la machine virtuelle a 
l'aide de SSH
	-	ssh dantremb@127.0.0.1 -p 4242

37. On ne doit pas pouvoir se connecter avec l'utilisateur ROOT
	-	ssh root@127.0.0.1 -p 4242

38. Qu'est ce que CRON
	-	Cron est une horloge ou l'on peut insserer notre ligne de
		commande a ??tre execut?? selon la periode demand??.

39. Comment fonctionne le script
	-	lscpu = info sur les processeurs
	-	free -m = affiche les infos memmoire (-m en megaoctets)
	-	df -Bg = affiche espace disponible sur les disques
	-	ss = analyseur de socket pour le reseau
	-	cat /proc/stat = fichier de statistic du processeur

	-	grep = retourne les lignes avec un mots choisi
	-	awk = transforme les lignes en mot ou $1 est le premier mot
	-	echo = imprimme a l'??cran le message

39. Comment le faire d??marrer au 10 minutes
	-	sudo crontab -e -u root
	-	ajouter le temp et le script
	-	on ajoute wall qui est un programme qui envoi le message
		sur le terminal.

40. Retirer la r??gle de d??marrer au Startup puis red??marer la
	machine. Le script doit toujours ??tre pr??sent sur la machine
	mais n'a pas d??marrer.

Bonus.	MARIADB:	Un systeme de gestion de base de donn??es libre
					identique a mySQL. Il on le m??me cr??ateur.
					My ??tait son 1e enfant et Maria sont 2e :D

		LIGHTTPD:	Un server web HTML l??ger et rapide

		PHP:		Hypertext Preprocessor : c'est un langage qui
					a pour but de g??n??rer des pages web HTML ou
					autre pour seulement envoyer la page final aux
					utilisateur. Donc le code source est prot??g??s.

		WORDPRESS:	est un syst??me de gestion de contenu open-source
					pour cr???? du contenu sur le web facilement. 43%
					des sites sont cr???? a partir de wordpress.

		FAIL2BAN:	une application cherchant des tentatives r??p??t??es 
					de connexions infructueuses et proc??de ?? un ban 
					en ajoutant une r??gle au pare-feu iptables pour 
					bannir l'adresse IP de la source.

------------------------------  LVM  -----------------------------
LVM est une m??thode et un logiciel de gestion de l'utilisation des 
espaces de stockage d'un ordinateur. Il permet de g??rer, s??curiser 
et optimiser de mani??re souple les espaces de stockage en ligne dans 
les syst??mes d'exploitation de type UNIX.

Voici des partitions que l'on peut cr???? qui sont utilis?? par les
apps sous linux.

/swap    :  Lorque l'on ?? plus de m??moire RAM on peut utiliser une
			partition swap pour compl??ter la m??moire RAM. Utiliser
			lors de travail sur de gros fichier qui d??passe la m??moire
			RAM install?? sur la machine.

/(root)  :  La partition qui contient le syst??me d'exploitation en
			plus du dossier de l'utilisateur ROOT.

/home    : 	La partition qui contient les dossiers utilisateur.
			?? chaque utilisateur cr???? un dossier du meme nom est
			cr???? sur la partition /home/<username>.

/srv 	 :  Partition utilis?? pour les scripts et donn??es de server.

/tmp     :	Partition pour stocker des donn??es temporaire que certaines
			programmes utilise.

/var	 :	Partition qui contient des fichiers dans lesquels le 
			syst??me ??crit des donn??es au cours de son fonctionnement.

			/car/cache	: Cache des programmes.

			/var/games	: Donn??es relatives au jeux.

			/var/lib	: Biblioth??ques de donn??es dynamiques

			/var/lock	: Contient les fichiers de verrouillage
						  cr????s par des programmes pour indiquer
						  qu'ils utilisent un fichier.

			/var/log	: contient les fichiers journaux

			/var/run	: Contient les informations valide jusqu'au
						  red??marage du systm??me.

			/var/spool	: Espaces pour les nouvelles, files d'attente 
					  	  d'impression et autre files d'attente.

			/var/mail	: contient les fichiers concernant les
						  server de messagerie. Une partition s??par??
						  est conseill?? car la majeure partie du
						  syst??me continuera de fonctionner m??me si 
						  vous ??tes spamm??.
