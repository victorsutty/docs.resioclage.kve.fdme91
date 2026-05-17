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