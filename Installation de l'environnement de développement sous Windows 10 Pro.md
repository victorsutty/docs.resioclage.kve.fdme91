
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


## Création de la machine virtuelle

![[vmware.png|559]]

## Gestion des utilisateurs

Sur PHPMy Admin, on retrouve la table "users" qui contient la liste des utilisateurs 


