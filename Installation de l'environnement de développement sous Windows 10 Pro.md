
## Préambule

En ce mois de mars, le conseil directif de notre association s'est rassemblé pour définir un projet informatique visant à favoriser le partage d'informations d'orientation, au sein des adhérents. Ce projet à été cité éperdument pour ces trois raisons que nous avons relevé :

- Car le volume physique de savoirs écrits, archivés et stockés dans la bibliothèque augmente et nous n'avons pas de bonne méthode pour garder le travail propre
- Une grande partie des adhérents étant une population qui travaille en journée, la consultation est plus à même de se faire en soirée, alors que les locaux sont soumis à une fermeture plus tot, selon les habitudes de la mairie.
- Une majorité des adhérents habitent loin des locaux, certains déménagent après avoir trouvé un emploi, mais veulent toujours accéder à la documentation. 

## Introduction technique

Au sein de la DSI, il existe deux environnements de MédiaWiki. 
la **production** et celui de **développement**

L'un est fait pour tester les fonctionnalités, tenter de résoudre des bugs, et visuellement déceler le fonctionnement de la plateforme. On l'appelle l'environnement de **développement et d'expérimentation.** C'est sur ce médium qu'on peut se permettre de tester des choses, sans risquer la compromission de la production.

![[illustpng-dev.png]]

L'autre, activement utilisé par les adhérents, celui sur lequel les gens se connectent, intéragissent, consultent et parfois modifient des informations, accessible depuis l'adresse ```wiki.resioclage.info```, est appelé **environnement de production.** Son fonctionnemment, son intégrité et sa sécurité sont essentiels pour l'association. Lors de la manipulation des programmes, on prendra garde à sauvegarder, et à appliquer la procédure d'accès. 


![[illustpng-prod.png]]

## Besoins préalables

- Un ordinateur sous Microsoft Windows, de noyau Linux, ou un Macintosh d'Apple
- L'accès à un compte privilégié (Administrateur) sur cet ordinateur
- Un hyperviseur de type 1 hébergé sur un serveur (VMware ESXi, Microsoft Hyper-V, Proxmox) ou un hypervisueur de type 2 (Une application de virtualisation sur cet ordinateur comme UTM, VmWare Workstation Pro, VirtualBox)
- Un peu de temps

## Préparation

Selon le choix de votre hyperviseur (soit de type 1 ou 2), il sera nécessaire de créer une machine virtuelle, un véritable ordinateur virtuel. Nous choissisons de CREER notre environnement sur VmWare Workstation Pro (type2) car :

- Faire des sauvegardes de manière réccurente, sous la forme de "snapshots" est simple
- L'ergonomie y est bonne, et le visualiseur réactif
- Nous pouvons normer l'affichage en 4:3 pour produire une documentation homogène
- Une fois la machine terminée, nous pouvons éventuellement la pousser dans l'hyperviseur de type 1, vCenter dans notre cas. 

## Téléchargement de l'image de Windows 10

Pour faire fonctionner la machine virtuelle, nous avons besoin d'un système d'exploitation choisi pour l'environnement de développement. Dans notre cas, nous utiliserons Microsoft Windows 10 car la version est bien supportée par nos systèmes. 

Allez sur un ordinateur, si possible sous Windows, ouvrez un navigateur pus tapez "Windows 1 0". Les premiers résultats apparaitront, pour le télécharger chez Microsoft. 

![[w10-google.png]]

Scrollez, ou descendez la page avec le curseur à droite

![[w10-msdwd2.png]]

Scrollez, ou descendez la page avec le curseur à droite

![[w10-msdwd3.png]]

Cliquez sur le bouton **"Télécharger"**

![[w10-msdwd5.png]]

Un téléchargement se lance. Votre navigateur devrait vous notifier de celui-ci. 

![[w10-msdwd6.png]]

Rendez vous dans votre dossier des téléchargements puis double-cliquez sur l'exécutable que nous venons de télécharger. 

![[w10-msdwd7.png]]

Une fenêtre apparait, demandant d'accepter les contrats. Cliquez sur Accepter. 

![[w10-msdwd9.png]]

Le logiciel charge les fichiers.

![[w10-msdwd10.png]]

Dans notre cas, nous devons produire une image au format .iso, et pas créer une clef USB pour un vrai ordinateur.

Sélectionnez la deuxième option

![[w10-msdwd12.png]]

Décochez l'option "Utilisez les options recommandées pour ce PC" puis cliquez sur suivant

![[w10-msdwd13.png]]

Sélectionnez la langue qui vous convient et l'architecture 64bit

![[w10-msdwd14.png]]

Choisissez le média **"Fichier ISO"**

![[w10-msdwd16.png]]

>[!ERROR] Il se peut que l'installateur se plaigne de ne pas avoir assez d'espace disque. Dans ce cas, faites le ménage sur votre ordinateur en supprimant les fichiers lourds (vidéos par exemple) dont vous n'avez plus besoin, puis recommencez

![[w10-msdwdfail.png]]

L'installateur demande à choisir le répertoire de téléchargement, qui est par défaut C:\Téléchargemnts. Gardez le en cliquant sur Enregistrer. 

![[w10-msdwd17.png]]


L'installateur va télécharger les fichiers et charger. Cela peut prendre un peu de temps.


![[time.png]]


Une fois téléchargé, l'emplacement du téléchargement est confirmé. Cliquez sur **Terminer**


![[w10-msdwd22.png]]

On retrouve alors le fichier image **"Windows 10"** 

![[w10-msdwd24-expl.png]]



## Création de la machine virtuelle

A l'ouverture, vmware se présente comme une application graphique qui comporte une barre de menus (Fichier, Modifier, Afficher etc), un panneau latéral qui liste les machines virtuelles car on peut en avoir plusieurs, puis une fenêtre de visualisation. C'est l'équivalent d'un écran relié à un PC.

Pour créer l'ordinateur virtuel que nous allons utiliser pour héberger l'environnement de développement de mediawiki, nous allons cliquer sur Fichier > **Nouvelle machine virtuelle**
Ou faire la combinaison de touches Ctrl+N pour être plus direct. 


![[newvm-vmware.png]]

Une fenêtre s'ouvre. C'est l'assistant de création (wizard) qui nous permettra de configurer l'ordinateur virtuel en spécifiant des paramètres propres à celle-ci tels que :

- Le nombre de coeurs alloués au processeur
- La quantité de RAM (la mémoire vive)
- La quantité de l'espace de stockage sur le disque dur
- Le type de noyau
- La version de VmWare

Faisons le choix de suivre le wizard en **mode personalisé,** car nous avons besoin de régler un paramètre important, favorisant l'intégration de notre machine vers l'hyperviseur de type 1.

![[assistantonly-vmware.png]]

En cliquant sur suivant, le wizard demande de sélectionner une image d'installation. Nous l'avons dernièrement téléchargée sur notre poste. Sélectionnons-là avec le bouton **Parcourir...**

![[assistantonly1-vmware.png]]

Une fenêtre de séléction s'ouvre. Sélectionnez le fichier image, puis cliquez sur ouvrir. 
Pour l'exemple, le chemin vers le fichier est D:/Windows 10.iso et pas C:/ car elle se situe sur un autre disque. Ne prenez pas compte de cette variation. 

IMAGE MANQUANTE SELECTION

![[assistantonly2-vmware.png]]

En cliquant sur suivant, on est amené à chosir un nom pour notre machine. Ici, on l'apellera W10-MEDIAWIKI. On retrouve aussi l'emplacement du disque virtuel de la machine qui peut poser plusieur Go (64go, comme défini dans le wizard). Nous l'avons placé dans le disque secondaire D:/ car nous ne voulons pas remplir notre disque C:

![[assistantonlynaming-vmware.png]]

![[assistantonlynamingok-vmware.png]]

![[hwsel28gig-vmware.png|429]]

![[hwselquadcore-vmware.png|433]]

ORGANISER LES IMAGES

![[assistantvdisksize-vmware.png]]


![[assistantresume-vmware.png]]

Une fiche de résumé apparait, nous permettant de verifier que les informations sont correctes. Nous pouvons à présent cliquer sur **Terminer** pour lancer la machine. 

## Lancement de la machine virtuelle

Lors du lancement, un onglet s'ouvre et l'espace de virtualisation s'ouvre. Un court instant après, le texte "Press any key to boot from CD or DVD...." s'affiche. Il faut rapidement appuyer sur une touche du clavier pour lancer l'installation de Windows



![[bootkey-vmware.png]]

>[!INFO]
>Si vous ne pressez pas la touche assez rapidement, l'ordinateur virtuel démarera dans le vide et pas sur l'image d'installation de Windows. Vous devez fermer la machine et la redémarrer. Pour cela, vous pouvez suivre cet article : [[Redémarrer une machine virtuelle plantée]]

![[efitimout-vmware.png]]

L'ordinateur virtuel démare. 

![[load-vmware.png]]

Si vous voyez l'icone de chargement, alors l'installateur de windows se lance. 

![[loadw10-vmware.png]]

Le wizard d'installation de Windows apparait. Cliquez sur installer maintenant, puis Séléctionnez la langue selon vos besoins. 

![[w10-install-now.png]]

![[w10-install-regionlg.png]]

La prochaine fenetre vous demandera une clé d'activation, car windows est un produit commercial propriétaire et payant. Nous n'avons pas de clé. Cliquez sur
**Je n'ai pas de clé de produit**

![[w10-install-productkey.png]]

Sélectionnez la version Pro dans la liste déroulante

![[w10-install-selprodpro.png]]

Acceptez de nouveau les termes du contrat. 

![[w10-install-acceptlic.png]]

Cliquez sur installer uniquement Windows

![[w10-install-onlyinst.png]]

Maintenant, il faut sélectionner le disque virtuel que nous avons créé tout à l'heure. 
Suivant

![[w10-install-seldrive.png]]

Maintenant, windows va s'installer. 

![[w10-install-load.png]]

Lorsque l'installation est (presque) terminée, l'ordinateur doit redémarrer. 

![[w10-install-installrunreboot.png]]

Après son redémarrage, la machine va traverser plusieurs redémarrages.

![[w10-install-installhw.png]]

![[w10-install-wait.png]]

Il faut à nouveau régler les paramètres régionnaux

![[w10-install-regionlg2.png]]

![[w10-install-regionlg3.png]]

Ignorer pour la deuxième disposition 

![[w10-install-regionlg4.png]]

![[w10-install-net.png]]

Cliquez sur "Configurer pour une organisation"

![[w10-install-adjoinornot.png]]

Cliquez sur Joindre le domaine à la place

![[w10-install-domainjoin.png]]

Saisissez le nom d'utilisatueur de la machine virtuelle, ici mgmt-mediawiki

![[w10-install-username.png]]

Une série de demandes d'installation de programmes de service et publicitaires vous seront demandés. Ignorez les tous.

![[w10-install-notedgesel.png]]

![[w10-install-noreg.png]]

![[w10-install-noloc.png]]

![[w10-install-onlytelem.png]]

![[w10-install-noman.png]]

![[w10-install-ast.png]]

![[w10-install-nopub.png]]

![[w10-install-noexp.png]]

![[w10-install-nocort.png]]

Le système continue de s'installer.

![[w10-install-load2.png]]

![[w10-install-load3.png]]

Après tous ces messages, l'environnement de bureau de Windows 10 apparait. Nous pouvons à présent installer les programmes requis pour installer MediaWiki pour le développement.

![[w10-install-loaded.png]]

## Installer les additions invité

Le logiciel VMWare propose une pile à installer sur le système d'exploitation virtuel pour faciliter l'ergonomie et améliorer les performances. 

Installez le en cliquant sur "Installer Tools" en bas à droite

![[w10-install-installed.png]]

Windows détecte le disque virtuel d'installation des additions. 

![[w10-install-vmtoolsload.png]]

Ouvrez l'explorateur de fichiers, puis naviguez dans le lecteur DVD. Cliquez dessus

![[w10-install-vmtoolsexpl.png]]

Un UAC (fenêtre d'autorisation) apparait, cliquez sur Oui

![[w10-install-vmtoolsexpluacok.png]]

La fenêtre d'installation apparait, cliquez sur suivant

![[w10-install-vmtool2.png]]

Choisissez l'installation complète 

![[w10-install-vmtoolcompl.png]]

Cliquez sur Installer

![[w10-install-vminst.png]]

![[w10-install-vminst2.png]]

L'installateur vous demande de redémarrer la machine virtuelle. Cliquez sur Oui

![[w10-install-vminstreboot.png]]

La machine redémarre 

![[w10-install-vminstreboot1.png]]

## Préparation de l'environnement

Avant de commencer à développer, il est conseillé de mettre Windows 10 à jour, pour éviter des problèmes de stabilité.

![[w10-maj.png]]

Quand vous êtes de nouveau sur le bureau, cliquez sur le bouton windows, puis saisissez "upd", puis appuyez sur entrée pour ouvrir le panneau des mises à jour.

![[w10-majsrc.png]]

Le panneau de Windows Update apparait. 
Scrollez, vous verrez que l'outil commence à chercher les mises à jour. 

![[w10-majdwd.png]]

Attendez que les installations soit terminées, pas seulement leur téléchargement

![[w10-majdwdwait.png]]

![[w10-majdwdwaitinst2.png]]

Lorsque tout les statuts sont sous Redémarrage en attente, cliquez sur Redémarrer. 

![[w10-majdwdrdytoreboot.png]]

![[w10-coreupd.png]]

Après les MAJ, l'ordinateur virtuel redémarre

![[w10-conn.png]]

L'ordinateur est désormais à jour !

![[w10-edge.png]]

Ouvrez le navigateur Edge en double cliquant sur son icone

La fenêtre du navigateur s'ouvre. Une panoplie de demandes vous sera faite. À chaque demande, refusez les services additionnels en cliquant toujours comme sur la procédure

![[w10-edgeskip.png]]

![[w10-edgeskip1.png]]

![[w10-edgeskip2.png]]

![[w10-edgeskip3.png]]

Enfin, vous arrivez sur la barre de recherche. 

## Installation de WampServer

Tapez "https://wampserver.aviatechno.net/". Vous pouvez copier coller ce texte dans la machine virtuelle. 

![[w10-wamprealsite.png]]

Ce site référence et distribue les paquets d'installation de WAMPServer, ses mises à jour, ses plugins, mais aussi un outil permettant d'installer les dépendances C++ sans prise de tête. 

Cliquez sur Wampserver x.x.x, en dessous de "Last Wampserver full install version", pour télécharger wampserver. 
 ![[w10-wampdwd.png]]

Le navigateur vous notifie que le téléchargement est fait. 

### Vérification de l'authenticité de WAMP

Avant d'exécuter l'installateur, il faut vérifier si son contenu est bien celui annoncé sur le site, pour s'assurer de son authenticité. Pour cela, nous allons utiliser le hash MD5 qui est présent sur le site. On ne peut pas le voir, car le site est trop zoomé. 

Dézoomez en allant cliquer sur ... dans la barre supérieure du navigateur puis - à coté du pourcentage d'échelle

![[w10-wampresizemd5.png]]

Sélectionnez le hash, puis copiez le à l'aide de CTRL + C

![[w10-wampresizemd5tooglectrl.png]]

Minimisez Edge, et ouvrez l'explorateur de fichiers. 
wampserver est bien présent. 

![[w10-deskreddwdexpl.png]]

Afin de vérifier l'authenticité, cliquez sur le menu windows, puis tapez "cmd" puis entrée pour ouvrir le shell. 

![[w10-deskcmd.png]]

Tapez ```cd Downloads``` pour naviguer dans le répertoire des téléchargements

![[w10-deskcmddir2.png]]

Tapez ```certutil -hashfile``` puis tapez wamp puis appuyez sur tabultation pour autocompléter le nom de l'exécutable puis espace puis tapez MD5. Appuyez sur entrée si la commande correspond à : hormis les x pour la version

```
certutil -hashfile wampserverx.x.x_x64.exe MD5
```


![[w10-deskcmddir3.png]]

La commande s'exécute

![[w10-deskcmdhashok.png]]

Ouvrez le bloc notes

![[w10-blcnotes.png]]

Collez le hash de tout à l'heure avec CTRL + V

![[w10-blcnotes3.png]]

notez sa provenance

![[w10-blcnotes4.png]]

reprenez le hash de l'invite de commande en le séléctionnant, puis en faisant CTRL + MAJ + C

![[w10-blcnotes6.png]]

et collez le dans le bloc notes pour le comparer avec celui du site.

![[w10-blcnotes7.png]]

Si vous avez du mal à lire le hash sans sauter des yeux comme moi, on peut les comparer dans l'invite de commande directement. 

![[w10-blcnotes8.png]]

Reprenez le shell et tapez 

```
if "le hash site" == "le hash certutil" (echo OK)
```

![[w10-blcnotes9.png]]


![[w10-blcnotes10.png]]

Si le code renvoie OK, nous pouvons passer à l'installation de WAMP

![[w10-blcnotes10attentionresult.png]]

Fermez l'invite de commande et le bloc notes, puis descendez sur le site jusqu'a 
### Best way to install Visual C++ Redistributable Packages

![[w10-wampscrolldown.png]]

Le site propose un executable qui installe toutes les dépendances d'un coup. Cliquez sur le lien, 
puis cliquez sur télécharger en sélectionnant le miroir

![[w10-wampredist.png]]

![[dwd-aio.png]]

![[dwd-aiodwd.png]]

Comme avec Wamp, il faut vérifier l'authenticité du fichier. Copiez le MD5

![[dwd-aiodwdmd5.png]]

Notez le dans le bloc notes

![[dwd-aiodwdmd5redistblc.png]]

ouvrez l'invite de commande 

![[w10-cmdopen.png]]

Allez dans le répertoire de téléchargement avec ```cd Desktop``` puis listez les élements avec ```dir``` 
Utilisez l'auto-complétion pour le nom du fichier d'installation

![[w10-certutilruntime.png]]

```
certutil -hashfile Visual-C-Runtimes-All-in-One-xx-xx.zip
```

![[w10-certutilruntime3.png]]

```
if "le hash site" == "le hash certutil" (echo OK)
```

![[w10-certutilsha1purée.png]]

>[!INFO] Ici, la comparaison échoue, car les deux caractères ne sont pas en majuscule.




>[!INFO] Une dépendance, c'est un ou plusieurs logiciels qu'on doit installer avant d'installer le programme désiré. Si ces logiciels ne sont pas installés avant, le logiciel en question ne fonctionnera pas ou pas bien. 


Une fois que les deux sources sont vérifiées, nous allons extraire le fichier .zip des dépendances. Pour ce faire, cliquez droit, puis cliquez sur Extraire tout

![[w10-extract.png]]

Cliquez sur extraire
![[w10-extract2.png]]

Une fois que les fichiers sont extraits, on peut exécuter le script "install_all" pour lancer l'installation groupée de tous les paquets.

![[w10-batchinstall.png]]

Cliquez sur Exécuter

![[w10-batchinstalladmex.png]]

Pensez a cliquer sur oui a chaque prompt UAC 

![[w10-batchinstalladmexuac.png]]


L'installation progressra sous forme de plusieurs installations.


![[loadred.png]]

On peut à présent ouvrir l'installateur de Wamp

![[w10-wampinstall.png]]

en prenant soin de dire oui à l'UAC

![[w10-wampinstalluac.png]]

Sélectionnez la langue

![[w10-wampinstalllang.png]]

Accepter la licence

![[w10-wampinstallic.png]]

Nous avons installer les redist, donc suivant

![[w10-wampinstallredistwarn.png]]

Garder le chemin racine de Wamp

![[w10-wampinstallrootc.png]]

Chargement

![[w10-wampinstallrootc5.png]]

Choisir les applications par défaut, en mettant tout sur oui. 

![[w10-wampinstallrootc7.png]]
Suivant
![[w10-wampinstallinf.png]]

Installation terminée, cliquez sur terminer

![[w10-wampinstallend.png]]

Avec l'explorateur de fichiers, naviguez dans le disque local C:

![[w10-wamproot.png]]

Rendez vous dans wamp64

![[w10-wampwww.png]]

On y retrouve le répertoire www 

## Gestion des utilisateurs

Sur PHPMy Admin, on retrouve la table "users" qui contient la liste des utilisateurs 


