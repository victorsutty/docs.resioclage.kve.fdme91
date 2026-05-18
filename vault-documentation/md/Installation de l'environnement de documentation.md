
[[Gestion de la documentation DSI avec Quartz et Markdown]]


Pour bénéficier d'un site ou l'on retrouve l'explication du code, des systèmes et des services hébergés chez resioclage, nous avons décidé de mettre en place Hugo, un moteur de rendu des pages Markdown.


## Téléchargement et vérification de Debian

Debian est un SE (Système d'Exploitation) très connu pour son caractère à prioriser l'open-source, 

Pour l'obtenir, rien de plus simple, ouvrez votre navigateur web habituel, puis tapez "debian" dans la barre de recherche, puis cliquez sur **Téléchargement de Debian**

![[2. googlesearch-seldebian.png]]
Sur le site, on retrouve de quoi télécharger le fichier d'installation, mais aussi la somme de contrôle. La **somme de contrôle** est plus importante qu'il n'y paraît, mais qu'est ce que c'est ? 

C'est une chaine de caractères, qu'on nomme parfois *checksum* qui permet de vérifier que le fichier téléchargé correspond à celui qui est distribué, parce qu'il pourrait être corrompu (il manque des bouts), ou il pourrait être modifié par un méchant, qui veut bousiller nos machines. 

La meilleure analogie que j'ai trouvée : Imaginez vous enfant, vous apprenez la vie. Si on vous dit de crier 

![[3. dwdmain-debian.png]]
En cliquant sur la somme de contrôle au format SHA512, le navigateur ouvre un fichier contenant les chaines de caractère suivi du nom de l'image. 

Dans notre cas, on va devoir noter celle de **debian-13.5.0-amd64-netinst.iso**
On copie en sélectionnant le texte et avec CTRL+C

![[7. sha-lookup.png]]
On ouvre donc un éditeur de texte et on copie colle dedans.

![[8. note-sha.png|542]]
A présent, on va devoir vérifier que le fichier est conforme à la chaine de caractère. En documentant, je ne sais pas bien comment vérifier. Je me souviens d'un outil qui s'apelle **shasum**, et je sais qu'il est présent sur tout les SE sauf Windows. Alors pour voir, j'ouvre le manuel en tapant `man shasum` dans le terminal, puis entrée

![[10. shasum-man.png]]Le grimoire s'ouvre !
On retrouve les options que nous allons pouvoir utiliser :

```
l'OPTION -a 256 (pour prendre l'algo adéquat)
```

```
[FILE], qui est donc notre fichier à vérifier.
```

![[11. shasum-man2.png]]
Entrons alors la commande. 

>[!INFO]
>Une fois que vous avez saisi shasum -a 256, pour saisir le chemin vers le fichier plus rapidement, utilisez la touche TAB. Commencez a taper Down pour Downloads, puis appuyez sur tab. ensuite, tapez debian, puis refaites tab pour compléter

![[12. shasum-complete.png]]
Après avoir appuyé sur entrée, la commande s'exécute. On retrouve la somme de controle.
C'est le texte un peu long et aléatoire.

![[13. shasum-result.png]]
Maintenant, il suffit de le comparer avec celui que nous avons noté tout à l'heure. Si c'est le même, c'est bon ! Pour ça, copiez la longue chaine de caractères, et retournez dans le bloc notes. Collez là en dessous.
![[14. note-sha2.png|476]]
Puis, faites Cmd+F ou CTRL+F pour chercher les occurences. Si il y a deux résultats, c'est good !
![[15. note-srch.png|482]]
## Création d'une machine virtuelle Debian

A l'heure ou cet article est écrit, le serveur qui fait tourner les machines virtuelles n'est pas idéal pour faire notre documentation. Il est lent, la résolution est mauvaise et il ne nous permet pas de faire des captures de qualité. 

Comme je suis sur un Mac et que l'architecture est spéciale (arm), j'ai décidé de documenter avec un environnement Proxmox, installé sur un lab. Par la suite, nous pousserons nos machines sur Vcenter, l'environnement de l'asso.

On assigne d'abord un nom à la machine virtuelle, ici ![[22. isosel.png]]


![[17. createct-name.png]]
Avant de créer la machine, il faut importer l'image de debian que nous avons téléchargée dans l'hyperviseur en sélectionnant le pool de stockage local (pve), puis en allant dans iso images. 
On peut cliquer sur Upload. 
![[18. createct-iso.png]]
En cliquant sur select file, on va choisir notre image. 

![[19. isosel.png]]

Ca se passe dans l'explorateur de votre SE.
![[20. upload.png]]
Cliquez sur ouvrir puis upload

![[21. upload done.png]]

Si le log montre Task OK, vous pouvez fermer la fenêtre. 

![[22. isosel.png]]

Dans Create:Virtual Machine, laissez les éléments par défaut. 
Dans la section Disks, laissez le stockage par défaut, 32G suffisent. 
Attention, choissisez bien votre pool de stockage. Dans la section CPU, 4 coeurs suffisent. Sur le type de CPU, choissisez host. ça nous permettra d'utiliser le CPU du serveur et pas une émulation qui rame

![[23. CPU.png]]
En mémoire, 4go suffisent. Si votre serveur est plus petit en terme de ram, choissisez 2go soit 2048

Dans network, laissez par défaut sauf si vous avez des réseaux sans internet
Appuyez sur finish en cochant la case Start after created

Rendez vous dans la machine vituelle sur le panneau a gauche puis console pour ouvrir le moniteur. Sélectionnez Graphical Install et appuyez sur entrée

![[24. Debinstall.png]]
Sélectionnez la langue pour l'installation (Français)

![[25. FR.png]]
Choisissez également France pour la situation géographique
![[26. FR2.png]]
Prenez la disposition de clavier en français puis Continuer

![[27. FRKB.png]]
Il est possible que le SE se plaigne de ne pas avoir recu une adresse IP via le DHCP. 
![[28. NODHCP.png]]
Dans ce cas, on peut soi même saisir l'adresse IP choisie, ce qui est même recommandé pour le serveur, afin de ne pas se retrouver avec une page web ou on ne sait pas ou elle est, qui change dans le temps. Les IP Statiques sont la référence pour les serveurs. 

Dans notre cas, nous nous sommes rendus compte que rien n'était joigniable car nous n'avions pas choisi la bonne interface réseau. Mais c'est bon à savoir dans le cas ou le DHCP n'est pas mis en place dans le réseau. 

![[29. MANUALIP.png]]
On peut à présent donner un nom à la machine.
![[30. hostname.png]]
Il faut donner un mot de passe pour le compte root, celui qui a tous les droits sur la machine. Nous **allons en saisir un simple**, qu'on changera après. On ne peut pas faire de copier coller, et pour éviter de ne plus pouvoir débloquer notre session si le clavier est mal configuré, c'est plus simple. 

>[!WARNING]
>Attention à cliquer sur la coche afficher le mot de passe pour vérifier ce que vous écrivez. Il peut arriver que votre clavier soit dans une autre langue que celle qui sera sur la future machine. 

![[35. flyingsaucer91300 - defroot.png]]
Cliquer sur continuer

Il faut maintenant créer un compte utilisateur différent de root, avec des droits réduits. 

Nous l'apelons **local-user**, car s'il on joint un annuaire comme LDAP ou TACACS, on pourra le dissocier. Il n'est pas nominatif, ce qui est moyen en terme de sécurité.

![[36. localuser.png]]
On doit également lui assigner un mot de passe, pareil.

![[37. palmtrees75 - user.png]]
### Partitionnement du disque

Dans notre contexte, nous allons utiliser tout le disque, pour simplifier l'installation. 

Cliquez sur continuer![[39. entiredisk2.png]]
On sélectionne le disque virtuel

![[40.disk.png]]
On sélectionne tout dans une partition
continuer

Et on applique les changements, puis oui en radiobox.

![[41. finaldisk.png]]

-*Faut il analyser d'autres supports d'installation ?* 

Séléctionnez Non

![[42. Install pkt.png]]
Le système va s'installer.
Maintenant, il faut choisir d'ou le système cherchera les MAJ. 

On sélectionne le miroir en france, et le miroir deb.devian.org pour l'archive

![[43. france.png]]
![[44. archive mirror.png]]
Dans notre environnement, nous n'avons pas de proxy, donc pas besoin d'en renseigner. Cliquez sur Continuer. 

Choissisez non pour l'étude d'utilisation des paquets.

## Séléction des utilitaires

>[!WARNING]
>ATTENTION : Retirez l'environnement de bureau Debian. Ici, nous sommes sur un futur serveur auquel on accèdera via une console SSH pour l'administration, et via une page web pour les lecteurs de la documentation. METTRE UN SERVEUR DE BUREAU SUR UN SERVEUR MULTIPLIE LA SURFACE D'ATTAQUE

Pour éditer la séléction, appuyer sur ESPACE ou cliquez sur les cases pour désellectionner les coches. 

![[46. Packets.png]]
*-Installer le programme de démarrage GRUB sur le disque principal ?* 
Oui. 

Il faut installer le bootloader. Sans ce programme, notre machine ne pourra pas démarrer. 

![[47. GRUB.png]]
Sélectionnez ensuite le disque sur lequel installer GRUB. 
**/dev/sda (scsi-0QEMU_QEMU_HARDDISK_drive-scsi0)**

![[48. GRUBsel.png]]
Quand l'installation est terminée, cliquez sur continuer.

![[49. Reboot.png]]

## Connexion à la machine

Quand la machine redémarre, vous arrivez sur la page de connexion. Vous pouvez vous connecter avec les identifiants que nous avons saisi tout à l'heure pendant l'installation, à savoir local-user et son mot de passe. 

![[50. Console Deb Login.png]]
Maintenant, il faut obtenir l'adresse IP. C'est celle ci qui nous permettra de nous connecter à distance, et de gérer le serveur plus efficacement. On entre alors la commande

```
ip a
```

On obtient l'interface boucle lo: ce qui ne nous intéresse pas. La deuxième est celle qu'on utilisera pour se connecter, soit l'interface ens18

![[52. IPA.png]]

Ici, l'adresse est 10.0.0.237

## Connexion à la machine à distance

Si votre ordinateur personnel ou de travail est dans le même réseau que la machine que nous avons installé, vous pouvez vous connecter au système à distance, via SSH (Secure Session H)

Ouvrez votre terminal ou votre console, puis entrez la commande suivante :

```
ssh local-user@10.0.0.
```

![[53. SSH fingerprint.png]]
A la première connexion, votre PC échange une identification de la machine. Il faut l'accepter en tapant yes puis entrée. 

Il faut ensuite saisir le mot de passe de local-user. Dès que vous voyez le shell de la machine, vous êtes connecté. 

![[54. Connected SSH.png]]

## Installation d'hugo

Pour obtenir un serveur web de documentation, il faut installler l'outil qui va générer les pages. Ce logiciel s'apelle Hugo et il est utilisé dans quasiment toutes les grandes boites de développement logiciel, sur les projets communautaires, mais aussi parfois dans les grandes entreprises d'IT. 

J'ai récupéré ça sur le blog d'un devops, mais voilà un brief : 

>[**Hugo**](https://gohugo.io/) est un **générateur de site statique** (SSG) écrit en Go, créé par Steve Francia en 2013. Son principal atout : la **vitesse**. Là où d'autres SSG mettent plusieurs secondes à builder un site, Hugo le fait en quelques dizaines de millisecondes

>On peut le comparer à un **compilateur ultra-rapide pour le web** : on écrit des fichiers Markdown, Hugo les assemble avec des templates Go et produit un site HTML complet, prêt à être servi par n'importe quel serveur web.

Source : https://blog.stephane-robert.info/docs/documenter/hugo/

Comme pour le MédiaWiki, nous allons suivre la documentation : https://gohugo.io/installation/linux/

Pour installer ce service, c'est un peu comme MediaWiki. On a deux solutions. On peut soit télécharger des binaires, ou compiler le projet pour en faire un binaire custom. 

Notre environnement ne va pas jusqu'ici, nous allons alors installer le logiciel avec le gestionnaire de paquets qui est sur debian, apt. 

![[55. Sudo.png]]
En essayant d'installer, on se rend compte que sudo, l'utilitaire pour exécuter une commande avec les permissions de l'administrateur, n'est pas installé. Temporairement, on va utiliser la commande su pour devenir administrateur.

```
su -
```

Entrez le mot de passe du superutilisateur.
On se retrouve alors dans le shell root. 

On peut alors installer hugo avec 

```
apt install hugo
```

![[56. Hugo PKT.png]]
Lors de la commande, le système de paquets nous propose d'installer les dépendances (paquets/logiciels) qui permettront le bon fonctionement d'hugo. Pour valider, on tape O puis entrée 

![[57. Hugo PKT2.png]]
Des lignes vont défiler, ce qui indique que le système télécharge lesdits logiciels.

Pour mettre à jour les articles et se synchroniser avec le système de fichiers, on doit utiliser git. 

Installons le comme dans la doc : https://git-scm.com/book/en/v2/Getting-Started-Installing-Git

Nous sommes sur debian, donc

```
apt install git-all
```

Pareil, on valide les paquets additionnels.
On peut maintenant vérifier que hugo est bien installé en demandant la version. 

```
hugo version
```

Si la console renvoie quelque chose, c'est installé.


## Création du projet

Dans le lien que j'ai mis, le quickstart, les développeurs nous montrent comment lancer un projet de zéro, ce qui n'est pas notre cas puisque nous avons déja des images et des fichiers Markdown. 

Dans notre environnement d'édition, la structure de fichiers et de répertoires est complètement libre. Dans Hugo, on a besoin de structurer les fichiers selon leur manière de faire. Il faut bien que le programme qui rend une page web sache ou prendre les fichiers. 

L'arborescence ressemble à ça (généré par ia, édité a la main)

```
mon-projet/            <-- le répertoire docs.ppe3.kve.fdme
├── content/          <-- La ou se trouveront les MD Obsidian
│   └── ma-doc.md
├── static/.          <-- Le dossier des images et icones
│   └── screenshots/  <-- Tes images devront être configurées 
├── themes/
├── hugo.toml
└── .git/
```

Il faut donc réarranger notre environnement, qui jusqu'ici, ressemblait à ça :

![[58. Obsidian éditeur.png]]
On a donc réorganisé la structure du répertoire pour matcher avec les exigences du moteur de rendu. 

![[59. Obsidian remake.png]]

FINALEMENT, on a décidé de conserver notre structure, mais de séparer notre espace d'édition avec l'espace d'hugo.

![[60. Remake2.png]]

Nous avons une nouvelle contrainte. Il faut que hugo puisse lire les titres, les chemins vers les images. Et vous savez quoi ? Ce n'est pas le meme format. 

Alors, on utilisera un outil très pratique pour convertir :


```
cp -r vault-documentation/img site-hugo/static/
```



```
hugo new project docs.kve.resioclage
```

On peut naviguer dans le répertoire qui a été créé.









## Protection, mesures de sécurité

Il est bon d'en choisir un robuste. Pour cela, il existe des sites comme Dashlane Password Generator ou des applications de bureau comme KeePass pour générer des chaines de caractères complexes. On peut également en générer un dans le terminal. C'est la méthode que je préfère. 

Faisons le. 

```
openssl 
```

En tapant openssl, on peut retrouver les algorithmes à utiliser. 

Je me suis posé la question de la complexité des mots de passe à mettre pour un compte admin. l'ANSSI, en france, publie des guides et recommendations destinés aux RSSI, Admins etc. 

Voici ce que l'un d'entre eux, qui est super complet cite :

>La robustesse d’un mot de passe est généralement mesurée au moyen de l’entropie, exprimée en bits. L’entropie d’un mot de passe peut être estimée en calculant l’ensemble des mots de passe possibles pour une longueur donnée et une complexité donnée. La longueur est une composante importante de la sécurité d’une authentification par mots de passe. Il est souvent plus efficace d’allonger un mot de passe que de chercher à le rendre plus complexe [5] pour en augmenter l’entropie.

[[anssi-guide-authentification_multifacteur_et_mots_de_passe.pdf]]

Faible à moyen Entre 9 et 11 ≈ 65
Moyen à fort Entre 12 et 14 ≈ 85
Fort à très fort Au moins 15 ≥ 100



Il est recommandé d’utiliser un sel choisi aléatoirement pour chaque compte et d’une longueur d’au moins 128 bits. 



En tapant openssl, on retrouve les algos dispo. ![[32. OpenSSL.png]]
Tout cela en utilisant une combinaison aléatoire
![[33. OpenSSL2.png]]
```
openssl rand -base64 45
```

45 octets purement aléatoires en mémoire

![[34. OpenSSL3.png]]
Copiez ce mot de passe dans un gestionnaire de mot de passe et imprimez le et stockez le dans un endroit sécurisé.

