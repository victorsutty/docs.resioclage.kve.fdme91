

## Fichier de config à backuper 

>[!WARNING]
Pendant l'installation, bien penser à faire une sauvegarde du fichier de configuration de MediaWiki, pour au cas ou si ça plante à l'adresse :


```bash
$RACINE/LocalSetting.php 
```


Lors que l'on essaie de se connecter sur le MediaWiki en login, nous avons une erreur PHP, "mediawiki internal error", suivie de logs presques illisibles.

Pour réparer ce problème, il suffit de mettre à jour la langue en modifiant le fichier LocalSettings, ligne 95, fr en en.



Bug de traduction de la version française