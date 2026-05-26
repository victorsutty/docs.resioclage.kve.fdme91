![[Screenshot 2026-05-16 at 16.32.10.png]]
LOG MDP mediawiki

LOG MDP ROOT : mediawiki

LOG wikiuser mariadb : mediawiki

Log root mariadb root : untrucksimple

LOG wikiadmin : untrucksimple


![[Screenshot 2026-05-16 at 16.43.25.png]]


MAJ
![[Screenshot 2026-05-16 at 16.44.27.png]]

	Je passe en ip statique dans le chemin indiquer ci dessous

![[Screenshot 2026-05-21 at 12.05.14.png]]

ON EST CENSÉ METTRE CETTE CONFIG DANS nano /etc/netplan/50(tab)
![[Screenshot 2026-05-21 at 12.26.54.png]]

(verifier les fichier de conf local setting apache) voir si tout fonctionne et normalement c'est ok




* apt-get (paquet maria php apache etc...)

* cd /tmp 
![[Screenshot 2026-05-16 at 16.50.35.png]]
Lien de telechargement media wiki dans le rep tmp

*extaraire*

![[Screenshot 2026-05-16 at 16.51.57.png]]

*creation dossier var/lib/mediawiki pour mediawiki*
*mv dossier mediawiki ver /va/lib/mediawiki*

- Configurer MariaDB
![[Screenshot 2026-05-16 at 16.56.23.png]]

Connexion a maria avec root sans password !!! a corriger

Création wikiuser pour Maria
![[Screenshot 2026-05-16 at 16.58.54.png]]



- création de la base de donnée dans maria (avec nom)
![[Screenshot 2026-05-16 at 17.00.16.png]]

On précise que l'on utilise cette BD qu'oin vient de crée
![[Screenshot 2026-05-16 at 17.01.03.png]]


- donner les droit a wikiuser 
![[Screenshot 2026-05-16 at 17.03.08.png]]

- commit 
![[Screenshot 2026-05-16 at 17.03.55.png]]


- changement du MDP pour plus de sécu 
![[Screenshot 2026-05-16 at 17.06.51.png]]


- configuration apache2

![[Screenshot 2026-05-16 at 17.10.40.png]]

-donner les droit a apache pour qu'il puisse exectuer le dossier mediawiki 1...... ( il appartient au groupe : www/data)
![[Screenshot 2026-05-16 at 17.13.41.png]]
![[Screenshot 2026-05-16 at 17.14.33.png]]
les droit son donner a www mais il n'ont pas le droit d'exectuer 

	Je change les droit chmod, je donne les droit 755

![[Screenshot 2026-05-16 at 17.16.09.png]]

tout les droits sont donner 
![[Screenshot 2026-05-16 at 17.16.56.png]]


	Je crée le fichier de configuration Apache2
Nom du nano = chemin
![[Screenshot 2026-05-16 at 17.19.46.png]]
![[Screenshot 2026-05-16 at 17.33.32.png]]


	 Je modifie le fichier de conf apache par défault par le fichier conf que je vient de crée

*on utilise plus se fichier de conf par default*
![[Screenshot 2026-05-16 at 17.36.45.png]]

	J'active le fichier de conf mediawiki
	
![[Screenshot 2026-05-16 at 17.38.23.png]]

	Je lance Apache2

![[Screenshot 2026-05-16 at 17.39.34.png]]

	Je demmare sur le navigateur l'ip qui heberge mediawiki
*Bonne nouvelle : il est accesible sur le réseau*

![[Screenshot 2026-05-16 at 17.50.19.png]]


Il nous manque une extention php qu'il faut installer...

![[Screenshot 2026-05-16 at 17.54.05.png]]

*Le message d'erreur intl à disparu, on a résolu le probleme*
![[Screenshot 2026-05-16 at 17.54.47.png]]

	je configure le local setting

		j'envoie le localsetting dans /var/lib/.....

je peut me connecter a resioclage sur le server linux 

![[Screenshot 2026-05-16 at 18.26.27.png]]









![[Screenshot 2026-05-16 at 16.35.14.png]]
debut 


# Permission

- Ouvrir LocalSettings.php (/var/lib.....)
	Je vois ces permissions par défaut

![[Screenshot 2026-05-21 at 09.31.37.png]]

	J'ajoute des permissions pour les differents user

![[Screenshot 2026-05-21 at 09.55.52.png]]

"$wgWhitelistRead = Autoriser à LIRE sans login"


# Ajout du logo

	Je doit envoyer le logo télécharger sur la VM

"Je passe par le terminal de l'ordinateur pour le transferer en scp sur la VM via le user mediawiki car root bloque le scp par défaut"
scp ~ chemin du logo~
![[Screenshot 2026-05-21 at 10.13.08.png]]

*Bien recu dans /home/mediawiki !*
![[Screenshot 2026-05-21 at 10.15.40.png]]

Transferer le logo recu chez le user mediawiki vers le rep asset chez root
![[Screenshot 2026-05-21 at 10.19.51.png]]

	Je change les parametre dans LocalSettings.php (nano)
	
![[Screenshot 2026-05-21 at 10.49.49.png]]

Le logo à bien était modifié !
![[Screenshot 2026-05-21 at 10.51.10.png]]

# Rendre le site accessible via hostname + HTTPS (Certificat SSL auto-signé)

	Je change le HostName de la VM qui héberge MediaWiki

![[Screenshot 2026-05-21 at 11.03.17.png]]

	J'installe l'outils avahi-deamon
	apt install avahi-daemon avahi-utils -y

Avahi daemon = permet de communiquer entre machine du meme réseau via les hostname

Avahi-utils = permet de monitorer Avahi

*Avahi est installer ! il faut maintenant démarrer le processus*
![[Screenshot 2026-05-21 at 11.07.25.png]]

## Passage en HTTPS

	Je verifie que openssl est installer ?

![[Screenshot 2026-05-21 at 11.13.27.png]]

Si not found installer avec la commande : apt install openssl

	Je configure le certificat

Grace à cette commande :
sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/private/mediawiki.key -out /etc/ssl/certs/mediawiki.crt -subj "/CN=resioclage-kve.local"

![[Screenshot 2026-05-21 at 11.39.30.png]]

	Je rajoute le certificat SSL dans le fichier de configuration Apache2(mediawiki.conf)
	
	Je modifie le port - 443
	Je change le ServerName par le HostNameDeLaMachine.local
	Je rajoute SSLEngine et les chemins du certificat et de la clé

![[Screenshot 2026-05-21 at 11.43.59.png]]

	Je modifie le $wgServer dans le fichier LocalSettings.php 
	Je rajoute le "S" a HTTP 
	Je modifie l'IP par le HostName

![[Screenshot 2026-05-21 at 11.48.00.png]]

!! Ne devons activer SSL sur apache !! 
![[Screenshot 2026-05-21 at 11.51.14.png]]

	a2enmod ssl

![[Screenshot 2026-05-21 at 11.53.21.png]]


*Arriver sur la page google, il affiche cette page*
![[Screenshot 2026-05-22 at 21.09.28.png]]
![[Screenshot 2026-05-22 at 21.20.45.png]]
Pourquoi ? 
	

Solution ?
	






!! Apache affiche la page par defaut, il devrais afficher notre mediawiki !!
![[Screenshot 2026-05-21 at 11.54.29.png]]

	Je rajoute la ligne ServerAlias * dans le fichier apache2 mediawiki.conf
*Il ignore la page par défaut et affiche notre mediawiki*
![[Screenshot 2026-05-21 at 11.57.55.png]]

CHANGER L'IP À FAIRE !!!
![[Screenshot 2026-05-21 at 12.05.14.png]]
