# Creation de la VM : 

- Custom
- Worstation 17.5 or later (pour une bonne compatibilité)
- I will instal later (Si tu montes l'ISO directement ici, VMware lance un "Easy Install" automatique qui configure tout seul)
- Microsoft windows + windows server 2022 (La compatibilité entre le client Windows 10 et Windows Server 2022 n'est pas une contrainte dans ce projet car la communication repose uniquement sur le protocole HTTP (couche application du modèle OSI). Le client envoie une requête HTTP sur le port 80, le serveur Apache répond avec du HTML — ce mécanisme est indépendant du système d'exploitation. N'importe quel OS disposant d'un navigateur web peut accéder au MediaWiki.)
- name + PATH
- UEFI (standard de démarrage moderne qui remplace le BIOS ; désactivé le Secure Boot car nous sommes dans un environnement de laboratoire virtuel.)
- Number of processors : 1 Number of cores per processor : 2 Total processor cores : 2 (Mon PC a 4 core physique don j'en met 2 pour en laisser 2 pour l'hote windows)
- Memory : 3072 MB = 3 Go (Ne jamais dépasser **50% de la RAM physique** pour une VM)
- Use network address translation NAT (La VM accède à internet via le PC hôte. Simple, fonctionne partout. Parfait pour installer Apache, PHP, MariaDB) passage en bridge pour la demo final
- LSI Logic SAS recommended (C'est le **composant qui gère la communication** entre le système d'exploitation et le disque dur (virtuel ou physique).)
- NVMe (recommander pour la version windows server 2022)
- Create a new virtual disk
- tore virtual disk as a single file (demander pourquoi ?)
- Custom hardware (isertiuon de l'iso + USB Controller → Remove)

### Résumé final de ta VM

| Paramètre | Valeur              |
| --------- | ------------------- |
| OS        | Windows Server 2022 |
| CPU       | 1 socket × 2 cores  |
| RAM       | 3072 MB             |
| Disque    | 60 GB SCSI          |
| Réseau    | NAT                 |
| CD/DVD    | ISO fr-fr monté     |

"""""""""Problematique rencontré : qctiver l'autorisation de virtualisation (Ctrl + alt + echap : processeur : virtualisation désactiver.
Il fallait alors l'activer : restar : F10 : securty systeme : Virtualization Technology + enable) à partir de la la VM fonctionne !

Une fois sur la VM 

-choisir personnalisé pour installer que l'os sans recuperer les parametre les fichier etc d'un server existant (ce n'est pas notre cas)"""""""""""""

Mots de passe session administrateur VM : Fnaclogistique-1970
MDP root DB : Fnaclogistique-1970
MDP wikiuser DB : Fnaclogistique-1970

Logo en png dans fichier asset 

changer le nom de la machjine pour changer l'url
fichier host équivalent a DNS

**INSTALLATION TERMINER !** 

# Qu'est-ce que MediaWiki et WAMPSERVER ?

MediaWiki : 
	il s'agit 

WAMPServer : Windows-Apach-MySQL-Php
	Il s'agit d'un ensemble de programme permettant le bon fonctionnement d'un server WEB

# Télécharger le WampServer

- Aller sur wampserver.com
- télécharger le Wampserver 64 BITS
![[Screenshot 2026-05-07 at 09.08.41.png]]
- Lancer le fichier .exe
![[Screenshot 2026-05-07 at 09.09.49.png]]
- Suivant
![[Screenshot 2026-05-07 at 09.10.57.png]]
- Suivant
![[Screenshot 2026-05-07 at 09.11.32.png]]
- Message d'erreur
![[Screenshot 2026-05-07 at 09.11.48.png]]
Ce message d'erreur indique que l'installation ne peut pas ce faire car il manque des paquetages. Les paquetage VC++ c'est une bibliothèque de fonctions crée par microsoft pour que certain logiciel puissent fonctionner correctement. Cette Bibliothèque n'est pas installé nativement.

Il est indiquer que les paquetages manquant sont sur le site : 

- http://wampserver.aviatechno.net/ rubrique VC++ redistribuable package
![[Screenshot 2026-05-07 at 09.18.14.png]]
Je comprend sur le message d'erreur qu'il manque des paquetage x86 et x64bits

- Je télécharge comme indiqué la derrière version de ces paquetages dans la partie "manière simple d'installer C++ Redistribuable Packages".
![[Screenshot 2026-05-07 at 09.49.42.png]]
![[Screenshot 2026-05-07 at 09.44.17.png]]
La bibliothèque ce télécharge...

![[Screenshot 2026-05-07 at 09.48.42.png]]
Les paquetage sont installer ! 

- Je relance WampServer.
	Cette fois, je peut passer à l'étape suivante

- Je choisi le PATH du WampServer
![[Screenshot 2026-05-07 at 09.51.21.png]]

- Suivant
![[Screenshot 2026-05-07 at 09.53.18.png]]
L'installation ce fait.

- Je choisis un navigateur par défault
	Pourquoi ? 
	Car WampServer Utilise une page web LOCAL comme interface graphique

![[Screenshot 2026-05-07 at 09.55.21.png]]

- Je séléctionne msedge (Application Edge)
![[Screenshot 2026-05-07 at 10.00.43.png]]

- Je choisis un editeur de texte et son emplacement
	Pourquoi ?
	WampServer a besoin d'un éditeur de texte pour savoir dans quelle application ouvrir les fichiers de configuration de PHP, Apache, MariaDB etc..

![[Screenshot 2026-05-07 at 10.05.38.png]]
![[Screenshot 2026-05-07 at 10.07.34.png]]


- Wampserver est installer !
![[Screenshot 2026-05-07 at 10.09.30.png]]



# Configuration WampServer Pour VirtualHost MediaWiki

- Je démarre WampServer
![[Screenshot 2026-05-07 at 10.11.47.png]]

WampServer est maintenant accessible depuis la barre de tache, c'est par la que l'on peut ouvrir les services. 
![[Screenshot 2026-05-07 at 10.13.30.png]]


Depuis cette icon j'ouvre le localhost, comme prévu elle s'ouvre sur une page local Edge
![[Screenshot 2026-05-07 at 10.16.00.png]]

comme indiquer il n'y aucun projet, il faut donc commencer par crée le projet "RESIOCLAGE"

ERREUR RENCONTRÉ : Crée le repertoire pour le projet

En créent simplement un repertoir, je vois afficher simplement le nom du repértoir (projet) sans pouvoir cliquer dessus pour y accèder. il me dis que pour pouvoir utiliser ce repertoire en lien http il faut le déclarer en tant que virtual host.
![[Screenshot 2026-05-07 at 11.13.49.png]]

Pourquoi ? 
Sans Virtual Host → Tu as une chambre dans la maison de quelqu'un _(sous-dossier de localhost)_
Avec Virtual Host → Tu as ta propre maison avec ta propre adresse _(ton propre domaine local)_

Solution il faut donc crée un virtual host pour évité des conflit entre plusieurs projet car il partagent tous les meme cookies, session PHP, variable de configuration 

- Je crée un VirtualHost 
![[Screenshot 2026-05-07 at 11.40.41.png]]

- Je redemmarre les services pour que voir apparraitre le nouveau LocalHost
![[Screenshot 2026-05-07 at 11.42.21.png]]
![[Screenshot 2026-05-07 at 11.43.47.png]]


Le VirtualHost à bien était crée !!
![[Screenshot 2026-05-07 at 11.44.27.png]]

- Je clique sur l'interface "RESIOCLAGE"
![[Screenshot 2026-05-07 at 11.45.17.png]]

# Installation de MediaWiki dans le VirtualHost

*Il faut maintenant que je mette l'installe médiawiki dans resioclage...

Pour ce fait, il faut que je crée un dossier portant le nom du virtualhost, dans C:/wamp64/www

![[Screenshot 2026-05-07 at 12.16.52.png]]

- Je télécharge sur le site de mediawiki, le dossier zip
![[Screenshot 2026-05-07 at 12.20.55.png]]

- Je colle le dossier décompressé dans C:/wamp64/www/resioclage

![[Screenshot 2026-05-07 at 12.26.38.png]]
*Téléchargement un peut long..

A noter, il faut redemmarrer les services WampServer quand on rallume la VM.

- J'extrait tout ls doc zip du fichier MediaWiki

![[Screenshot 2026-05-11 at 15.12.38.png]]


ERREUR : Difficulté a faire apparaitre media wiki dans le VirtualHost Resioclage.

![[Screenshot 2026-05-11 at 15.41.16.png]]

- J'ai coller le dossier Mediawiki 1.45.3 dans resioclage
- Je me suis trompé de chemin lors de la création du VitrualHost
*j'ai mis C:/Wamp64/www au lieu de C:/wamp64/www/resioclage. d'ou l'importance de crée dabbord le dossier resioclage avant de crée le VirtualHost*

1 résolution : Je copie le CONTENANT de Mediawiki 1.45.3, je le colle dans le dossier resioclage
![[Screenshot 2026-05-11 at 19.36.04.png]]

Résolution 2 : Je supprime le VirtualHost avec le mauvais chemin et j'en crée un avec le bon chemin en le nommant resioclage comme le precedent.
![[Screenshot 2026-05-11 at 19.58.47.png]]

- Je redemmare les services WAMPSERVER et je relance le VirtualHost resioclage
*sa fonctionne !!*
![[Screenshot 2026-05-11 at 20.02.42.png]]
*il m'affiche un message d'erreur concernant le localsetting.PHP qu'il ne trouve pas*

Pourquoi ? 

localsetting.php c'est quoi ? C'est le fichier de configuration, c'est dedans que se trouve toute la configuration du site mediawiki selon "mes besoins". A noter le fichier local setting.php ne se trouve pas nativement dans le dossier décompresser, il se crée loraque l'on procede à la configuration. Se que l'on fait maintenant..

	Je clique sur "set on web"...
# Configuration fichier LocalSetting.php

![[Screenshot 2026-05-11 at 20.03.48.png]]

- Je vérifie maintenant les information liée à l'environnement. 

Voici les information que nous trouvons : Tout est ok excepter 3 points 

APCu : c'est une extension de PHP permettant de garder des informations en cache.
*Au lieu de recalculer des données complexe a chaque demande, il stock les resultat du server dans une mémoire vive, c'est l'APCu*
Dans notre cas il n'est pas nécessaire car peut de données.

GNU diff3 : c'est un utilitaire de reconciliation 
*Le wiki fusionne les modifications de plusieurs utilisateurs sans qu'ils s'en rendent compte (tant qu'ils ne touchent pas à la même phrase).
Dans notre cas il n'est pas nécessaire car peut de user.

Git non trouvé : c'est un processus volontaire 
*un snapshot de la VM est suffisant et plus complet.


![[Screenshot 2026-05-12 at 10.56.07.png]]

- Je choisi la base de donnée MariaDB comme convenu sur le TP puis je rentre les parametres suivants : 
*Je met l'adresse IP du localHost ; le nom de la base de donnée ; le compte root pour la base de données.*
![[Screenshot 2026-05-12 at 13.52.34.png]]

Je laisse la case se connecter en SSL découcher 

Pourquoi ? 
	Car il peut afficher un message d'erreur, SSL permet de crée un tunnel privé permettant de crypter les données d'un point A à un point B. dans notre cas il s'agis d'une VM avec un server WEB local. donc rien ne sort. De plus, il faut installer des certificats pour sur le server MariaDB qu'on na pas installer.

Je remplace le nom localhost par son IP comme recommander.
![[Screenshot 2026-05-12 at 13.49.58.png]]
![[Pasted image 20260512135146.png]]

Le compte root ne doit pas avoir de MDP...


![[Screenshot 2026-05-12 at 14.00.57.png]]
Il me demande crée un compte de la base données pour l'accès web.. ?

Pourquoi ?
	Le compte root comme vu précedemment est un compte d'installation, il a les pleins pouvoir sur toutes la base données, il est la pour crée la BDD ensuite MediaWiki oublie root pour ne pas prendre de risque. 
	
	C'est donc pour cela qu'on crée un compte accès web "wikiuser"
Il sert uniquement à lire, écrire ou modifier le contenu des pages (le texte de ton wiki). Il n'a pas le droit de supprimer la base de données entière ou de modifier d'autres projets sur ton Wamp.


- Je renseigne le nom du mediawiki ; je crée un compte admin 

![[Screenshot 2026-05-12 at 14.12.32.png]]
![[Screenshot 2026-05-12 at 14.13.22.png]]

Suivant 

![[Screenshot 2026-05-12 at 16.07.42.png]]
*Rédacteur autorisé seulement pour que les membre de l'associassion puissent consulter uniquement le contenu et les user approuvé puissent faire des modifications*

- Je selectione Créative common attribution - partage identique 
*Pour que je garde le droit d'auteur du contenue si ce dernier est partager ainsi que pour éviter qu'un utilisateur approuvé puissent modifier quelque chose est verrouillé la modification derrière lui*

- Je met également l'adresse mail de l'admin pour les retours.

- Je choisi le thème que je veut (apparence)
![[Screenshot 2026-05-12 at 16.23.28.png]]


- Je configure maintenant toute les options nécessaire pour ce Wiki
![[Screenshot 2026-05-13 at 10.58.24.png]]
![[Screenshot 2026-05-13 at 11.03.23.png]]

- J'active le téléversement des fichers pour garentire un backup en cas d'erreur,  je met le PATH![[Screenshot 2026-05-13 at 11.09.15.png]]

Avant de valider

- Je crée un dossier archive pour les fichier supprimer
![[Screenshot 2026-05-13 at 11.06.07.png]]

	Continuer

*Une dernière confirmation avant de lancer l'installation, les modifications sont encore possible*
![[Screenshot 2026-05-13 at 11.12.08.png]]


*L'installation s'est bien passer, la base de données est bien installée*
![[Screenshot 2026-05-13 at 11.14.01.png]]

	Continuer


*L'nstalation du MediaWiki est maintenant terminée !! 

Le fichier LocalSetting.php est maintenant téléchargeable.
![[Screenshot 2026-05-13 at 11.33.10.png]]

- Je place ce fichier à la racine du projet resioclage. 
![[Screenshot 2026-05-13 at 11.32.14.png]]

# Configuration et mise en page de RESIOCLAGE

Nous somme maintenant sur le mediawiki !

![[Screenshot 2026-05-13 at 11.40.31.png]]

Comme l'indique le message "*Connexion nécessaire*" il faut maintenant que se connecter


	Je me connecte au compte admin

![[Screenshot 2026-05-13 at 11.51.51.png]]

ERREUR : Page inaccessible..

![[Screenshot 2026-05-13 at 11.52.55.png]]

Pourquoi ? 
	Je vois que sur l'URL il est écrit MediaWiki hors il est censé etre écrit resioclage..
	De ce fait il est indiqué un message d'erreur DNS..

	Je vérifie le LocalSetting.php

![[Screenshot 2026-05-15 at 12.08.05.png]]

Je m'appercoit que la variable de configuration $wgServer qui définit l'identité du réseaux est 
http://mediawiki or il est censé pointer sur mon virtualhost resioclage.

	Je remplace mediawiki par resioclage

![[Screenshot 2026-05-15 at 18.02.54.png]]

	Je tente de me connecter une nouvelle fois

![[Screenshot 2026-05-15 at 12.11.48.png]]

Je peut maintenant acceder sans problème à la page d'accueil sans erreur 

	je modifie la page d'acceuil

![[Screenshot 2026-05-15 at 15.04.08.png]]

	Je remplace ce texte par celui que je souhaite pour la page d'acceuil de
	resioclage

Ici le language de prgrammation est le wikicode
![[Screenshot 2026-05-15 at 16.25.15.png]]

	Enregistrer les modifications

*Voici le résultat*
![[Screenshot 2026-05-15 at 16.26.14.png]]

Je peut voir que les lienss vers d'autre pages sont en rouge car il n'y a aucun contenu dedans. En cliquant sur le lien rouge je suis redirigé vers une interface qui me permet d'ecrire sur la page et de la générer en cliquant sur "enregistrer".

*Dans la page de préférence*

	J'ajoute en signature : administrateur 

![[Screenshot 2026-05-15 at 15.02.01.png]]

En fouillant je me rend compte que ma base de donnée est en MySql.  

# Migration de base données de MySQL vers MariaDB

	J'active le service MariaDB

![[Screenshot 2026-05-14 at 16.27.40.png]]

*J'exporte toutes les donnée de MySQL pour les mettre dans MariaDB*

	Boutton exporter

![[Screenshot 2026-05-14 at 16.40.30.png]]

	Exporter

![[Screenshot 2026-05-15 at 16.52.32.png]]

*Le fichier est télécharger*

![[Screenshot 2026-05-15 at 16.53.45.png]]

Sur l'interface MariaDB je crée une nouvelle base de données portant le meme nom que l'initial, et j'importe dans celle ci le fichier sql

![[Screenshot 2026-05-15 at 16.59.24.png]]
![[Screenshot 2026-05-15 at 17.01.51.png]]

Lorsque je reouvre resioclage un message d'erreur s'affiche concernant la base de donnée.

	Je modifie dans LocalSetting.php les ligne suivante :

![[Screenshot 2026-05-15 at 17.27.46.png]]	

Je précise le port de MariaDB 3307 ; le user de la BD et son MDP 

	Je redemarre WAMP et crée un nouv nouveau lien vers une nouvelles page

	Je verifie que la nouvelle page apparait bien dans MariaDB 

*BONNE NOUVELLE, elle apparait bien ce qui montre que resioclage est bien connecter a la BD de MariaDB*
![[Screenshot 2026-05-15 at 17.32.05.png]]

	Je supprime la BD de MySQL pour éviter les conflit

![[Screenshot 2026-05-15 at 17.36.11.png]]

A présent il existe qu'une seul base de données celle de Maria DB !


# Principe de sécurité par cloisonnement

MODIFICATION VIGILENCE !!!
	J'ai mis le compte root comme user système de la base de données, il s'agit la d'une erreur
	Le compte root possède tout les droits, en cas d'attaque le compte root peut corrompre toute la base de données or avec un compte "wikiuser" on peut cloisonner l'attaque la base de donnée resioclage. 


	Je supprime wikiuser de la BD MySQL

![[Screenshot 2026-05-15 at 21.11.41.png]]

	Je crée un wikiuser dans MariaDB avec ses droit :
	SELECT : Lire
	INSERT : Crée
	UPDATE : Modifier
	DELETE : Supprimer

	Je veille a ce que le nom d'hote soit LocalHost et non % (nativement)

![[Screenshot 2026-05-15 at 21.16.55.png]]
*WIKIUSER EST CRÉE*

	Je modifie le user système root par wikiuser

![[Screenshot 2026-05-15 at 21.21.18.png]]


# Gestion des permissions

*Voici les permissions accordé aux user anonyme*
![[Screenshot 2026-05-15 at 22.47.56.png]]
*On peut voir qu'aucune permission n'a était accordé*

	J'ajoute des permissions

