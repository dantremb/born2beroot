Voici mes notes pour Born2beRoot

-------------------------ligne-de-commande------------------------

su                  		: Login as root
chage -l <user>				: Check password history
sudo passwd <user>  		: Modify password
getent group <group name>	: Show groups members
sudo visudo					: Superuser config file
sudo groupadd				: Create a new group

apt-get update     			: Get update
apt-get upgrade     		: Install update
	"-y" after apt command will auto accept during install.
		exemple : apt install <program> -y

ssh-keygen          		: Make SSH key


------------------------------  LVM  -----------------------------
CONSIGNE : Vous devez créer au minimum 2 partitions chiffrées en 
utilisant LVM. Voici un exemple de partition attendue pour votre 
machine virtuelle.

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

------------------------------  ZSH  -----------------------------
J'ai installer Oh my zsh qui est le frameworl le plus populaire.
Il nous permet de se déplacer plus rapidement dans le terminal et
corrige nos erreur à l'entrer des commandes.

apt-get install zsh
sh -c "$(wget https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh -O -)"

------------------------------  SSH  -----------------------------
CONSIGNE : Un service SSH sera actif sur le port 4242 uniquement. 
Pour des questions de sécurité, on ne devra pas pouvoir se connecter 
par SSH avec l’utilisateur root.

apt install openssh-server
systemctl status ssh

On dois aller dans le dossier SSH pour aller modifier les infos du server.

/etc/ssh/sshd_config


----------------------------  FIREWALL  --------------------------
CONSIGNE : Vous allez configurer votre système d’exploitation avec 
le pare-feu UFW et ainsi ne laisser ouvert que le port 4242

apt-get install ufw
sudo ufw allow ssh
sudo ufw allow 4242
sudo ufw status numbered
sudo ufw status verbose
sudo ufw delete (rule number)

-------------------------  LIBPAM PWQUALITY  ---------------------
CONSIGNE : /START/
• Votre mot de passe devra expirer tous les 30 jours.
• Le nombre minimum de jours avant de pouvoir modifier un mot de
  passe sera configuré à 2.
• L’utilisateur devra recevoir un avertissement 7 jours avant que son
  mot de passe n’expire.
• Votre mot de passe sera de 10 caractères minimums dont une majuscule
  et un chiffre, et ne devra pas comporter plus de 3 caractères identiques
  onsécutifs.6Born2beRoot
• Le mot de passe ne devra pas comporter le nom de l’utilisateur.
• La règle suivante ne s’applique pas à l’utilisateur root : le mot de
  passe devra comporter au moins 7 caractères qui ne sont pas présents
  dans l’ancien mot de passe.
• Bien entendu votre mot de passe root devra suivre cette politique
\END\ 

$$apt-get install libpam-pwquality

$$edit /etc/pam.d/common-password

	Ajouté minlen=10 à la fin de cette ligne.
	password [success=2 default=ignore] pam_unix.so obscure sha512

	et la ligne juste au dessus
	password requisite pam_pwquality.so retry=3 lcredit =-1 ucredit=-1 dcredit=-1 
	maxrepeat=3 usercheck=0 difok=7 enforce_for_root

$$edit /etc/login.defs

	PASS_MAX_DAYS 30
	PASS_MIN_DAYS 5
	PASS_WARN_AGE 7

---------------------------  CREATE GROUPS  ----------------------


	