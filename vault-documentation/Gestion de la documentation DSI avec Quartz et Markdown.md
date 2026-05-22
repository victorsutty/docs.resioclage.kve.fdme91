
Au sein de la DSI, nous somme plusieurs membres à faire des actions, comme l'installation de logiciels, services et matériels. Une bonne pratique est de documenter ce travail, de manière consistante et procédurale. 

Comme support, nous avons eu la possibilité d'utiliser le standard en entreprise ou familial (Word ou Powerpoint) qui est très convoité pour sa simplicité. 

Aujourd'hui, de nombreuses communautés sélectionnent la voie du moderne, en privilégiant un accès uniforme et rapide à la documentation. On peut le retrouver sur 

docs.docker.com
docs.

### Lancer le serveur Quartz

Sur la VM présente sur l'hyperviseur VCenter vcenter7.info.local, nous avons déployé une machine virtuelle pour héberger ce service. On peut la gérer soit via le KVM via Vcenter en console directe, ou s'y connecter en SSH. 

Le plus pratique selon nous est de se connecter à distance avec le terminal Linux ou Windows depuis les postes dans le domaine info.local. 

Via l'adresse IP

```
ssh system@172.16.1.69
```

Via le domaine

```
ssh system@docs.kve.resioclage
```

Une fois connecté, on retrouve le dossier **quartz** dans le répertoire /home/system/. Ce répertoire à été initialement cloné à partir de l'URL suivante :

```
https://github.com/jackyzha0/quartz.git
```

On peut se placer à l'intérieur avec la commande **cd**

```
cd /quartz
```

On y retrouve un répertoire nommé content, qui contient bien évidemment le contenu, par exemple les fichiers texte en markdown et les images. 

L'idée est d'y copier l'intérieur du dossier **vault-documentation** de notre repo Github dedans :
https://github.com/victorsutty/docs.resioclage.kve.fdme91

Pour ce faire, on peut sortir du dossier quartz avec cd ..

Puis cloner le repo :

```
git clone https://github.com/victorsutty/docs.resioclage.kve.fdme91
```

Et lancer la copie.

```
cp -r docs.resioclage.kve.fdme91/vault-documentation/* /quartz/content/
```

## Lancer le serveur Quartz

Quand on a déja initialisé le projet, on peut lancer la commande **npx** soir **Node Package Execute** qui va exécuter le paquet quartz installé sur le gestionnaire npm (node package manager)

voir : https://laconsole.dev/blog/differences-npm-npx

```
npx quartz build --serve
```

![[quartz-init.png]]

Le serveur web est donc accessible sur le port 8080. 


## MAJ automatique

Quand on push du contenu sur github avec la console git push ou avec obsidian, on aimerait que le serveur se mette à jour automatiquement (qu'il dispose des derniers documents). Une sorte de script qui fasse effacer, puis git clone à nouveau, puis qui relance le serveur. 

Mais on pense qu'il y a mieux. ça semble un peu laborieux comme technique. 


https://quartz.jzhao.xyz/setting-up-your-GitHub-repository