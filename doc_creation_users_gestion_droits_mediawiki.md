# Création des utilisateurs et gestion des droits dans MediaWiki

## 1. Objectif du document

Ce document décrit la procédure de création des utilisateurs et de gestion des droits pour le MediaWiki de l’association **ResioClage**.

La gestion des utilisateurs se fait via l’interface d’administration de MediaWiki, principalement à partir des **pages spéciales**.

Cette procédure remplace l’utilisation directe de phpMyAdmin pour la création des comptes utilisateurs. phpMyAdmin doit être réservé aux cas exceptionnels de dépannage.

## 2. Contexte du wiki ResioClage

Le MediaWiki ResioClage est configuré comme un **wiki privé**.

Cela signifie que :

- les visiteurs anonymes n’ont pas accès à l’ensemble du wiki ;
- certaines pages publiques restent visibles sans connexion ;
- les adhérents disposent d’un compte utilisateur ;
- les administrateurs gèrent les comptes, les pages et les droits ;
- la création de compte est réservée aux administrateurs.

## 3. Rôles utilisés dans le wiki

Le fonctionnement du wiki repose sur trois profils principaux :

| Rôle             | Description                                   | Accès                               |
| ---------------- | --------------------------------------------- | ----------------------------------- |
| Visiteur anonyme | Personne non connectée                        | Accès limité aux pages publiques    |
| Adhérent         | Utilisateur connecté validé par l’association | Accès aux pages privées en lecture  |
| Administrateur   | Responsable du wiki                           | Accès complet et gestion du contenu |

## 4. Pages publiques accessibles sans connexion

Même si le wiki est privé, certaines pages doivent rester accessibles aux visiteurs anonymes.

Pages publiques  :

- `Accueil`
- `Objectifs de l'Association`
- `Adhérer`
- page de connexion

Ces pages permettent de présenter l’association, d’expliquer son objectif et de fournir la procédure d’adhésion.

## 5. Configuration des accès dans LocalSettings.php

La configuration des droits se fait dans le fichier :

```text
LocalSettings.php
```

Avec WampServer, il se trouve dans :

```text
C:\wamp64\www\mediawiki\LocalSettings.php
```

ou dans le dossier correspondant au nom du wiki.

## 6. Configuration recommandée

Ajouter ou vérifier les lignes suivantes dans `LocalSettings.php` :

```php
# --- Gestion des accès ResioClage ---

# Wiki privé par défaut
$wgGroupPermissions['*']['read'] = false;
$wgGroupPermissions['*']['edit'] = false;
$wgGroupPermissions['*']['createaccount'] = false;

# Utilisateurs connectés
$wgGroupPermissions['user']['read'] = true;
$wgGroupPermissions['user']['edit'] = false;
$wgGroupPermissions['user']['createpage'] = false;
$wgGroupPermissions['user']['createtalk'] = false;

# Administrateurs
$wgGroupPermissions['sysop']['read'] = true;
$wgGroupPermissions['sysop']['edit'] = true;
$wgGroupPermissions['sysop']['createpage'] = true;
$wgGroupPermissions['sysop']['createtalk'] = true;
$wgGroupPermissions['sysop']['createaccount'] = true;

# Bureaucrates
$wgGroupPermissions['bureaucrat']['createaccount'] = true;
$wgGroupPermissions['bureaucrat']['userrights'] = true;

# Pages publiques accessibles sans connexion
$wgWhitelistRead = [
    "Accueil",
    "Objectifs de l'Association",
    "Adhérer",
    "Spécial:Connexion",
    "Special:UserLogin",
    "Special:Userlogin"
];
```

## 7. Explication des droits

### 7.1 Groupe `*`

Le groupe `*` correspond aux visiteurs anonymes.

Configuration appliquée :

```php
$wgGroupPermissions['*']['read'] = false;
$wgGroupPermissions['*']['edit'] = false;
$wgGroupPermissions['*']['createaccount'] = false;
```

Cela signifie que les visiteurs anonymes ne peuvent pas lire tout le wiki, ne peuvent pas modifier les pages et ne peuvent pas créer de compte.

### 7.2 Groupe `user`

Le groupe `user` correspond aux utilisateurs connectés.

Dans le cadre de ResioClage, il s’agit principalement des adhérents.

Configuration appliquée :

```php
$wgGroupPermissions['user']['read'] = true;
$wgGroupPermissions['user']['edit'] = false;
$wgGroupPermissions['user']['createpage'] = false;
$wgGroupPermissions['user']['createtalk'] = false;
```

Cela signifie que les adhérents peuvent lire les pages privées, mais ne peuvent pas modifier ou créer des pages.

### 7.3 Groupe `sysop`

Le groupe `sysop` correspond aux administrateurs.

Configuration appliquée :

```php
$wgGroupPermissions['sysop']['read'] = true;
$wgGroupPermissions['sysop']['edit'] = true;
$wgGroupPermissions['sysop']['createpage'] = true;
$wgGroupPermissions['sysop']['createtalk'] = true;
$wgGroupPermissions['sysop']['createaccount'] = true;
```

Cela signifie que les administrateurs peuvent lire, modifier, créer des pages et créer des comptes utilisateurs.

### 7.4 Groupe `bureaucrat`

Le groupe `bureaucrat` peut gérer certains droits utilisateurs.

Configuration appliquée :

```php
$wgGroupPermissions['bureaucrat']['createaccount'] = true;
$wgGroupPermissions['bureaucrat']['userrights'] = true;
```

Cela permet à un bureaucrate de créer des comptes et de gérer les groupes utilisateurs.

## 8. Accéder aux pages spéciales

MediaWiki ne possède pas forcément un tableau de bord administrateur visible comme WordPress.

La majorité des actions importantes se trouvent dans les **pages spéciales**.

Pour y accéder :

```text
http://127.0.0.1/mediawiki/index.php/Spécial:Pages_spéciales
```

ou en anglais :

```text
http://127.0.0.1/mediawiki/index.php/Special:SpecialPages
```

Depuis cette page, l’administrateur peut accéder aux outils de gestion des utilisateurs, des pages et du wiki. 
## 9. Pages spéciales utiles

| Fonction | Page spéciale française | Page spéciale anglaise |
|---|---|---|
| Créer un compte | `Spécial:Créer_un_compte` | `Special:CreateAccount` |
| Liste des utilisateurs | `Spécial:Liste_des_utilisateurs` | `Special:ListUsers` |
| Gestion des droits | `Spécial:Permissions` ou `Spécial:Groupes_utilisateur` selon version | `Special:UserRights` |
| Pages spéciales | `Spécial:Pages_spéciales` | `Special:SpecialPages` |
| Modifications récentes | `Spécial:Modifications_récentes` | `Special:RecentChanges` |
| Toutes les pages | `Spécial:Toutes_les_pages` | `Special:AllPages` |

Les noms peuvent varier selon la version et la langue de MediaWiki.

## 10. Créer un utilisateur adhérent

### 10.1 Situation

Un adhérent est une personne dont la candidature a été acceptée par l’association.

Il doit recevoir un compte lui permettant d’accéder à l’espace privé du wiki.

### 10.2 Étapes de création

1. Se connecter au MediaWiki avec un compte administrateur.
2. Ouvrir la page spéciale de création de compte :

```text
http://127.0.0.1/mediawiki/index.php/Spécial:Créer_un_compte
```

ou :

```text
http://127.0.0.1/mediawiki/index.php/Special:CreateAccount
```

3. Renseigner le nom d’utilisateur.
4. Renseigner un mot de passe temporaire.
5. Confirmer le mot de passe.
6. Valider la création du compte.
7. Vérifier que l’utilisateur est bien créé.
8. Envoyer les identifiants à l’adhérent par e-mail en réponse à sa candidature.

## 11. Convention de nommage des comptes adhérents

Pour conserver une gestion propre, il est recommandé d’utiliser une convention de nommage simple.

Exemples :

```text
prenom.nom
```

ou :

```text
p.nom
```

Exemples concrets :

```text
dream.team
d.team
```

Éviter les identifiants imprécis comme :

```text
user1
test
adhérent1
comptewiki
```

## 12. Droits d’un adhérent

Par défaut, un utilisateur créé dans MediaWiki appartient au groupe `user`.

Dans la configuration ResioClage, ce groupe permet uniquement de lire les pages privées.

L’adhérent ne doit pas être ajouté aux groupes :

- `sysop`
- `bureaucrat`

Ces groupes sont réservés aux administrateurs.

## 13. Vérification d’un compte adhérent

Après création du compte, vérifier que :

- l’utilisateur peut se connecter ;
- l’utilisateur peut consulter les pages privées ;
- l’utilisateur ne peut pas modifier les pages ;
- l’utilisateur ne peut pas créer de pages ;
- l’utilisateur ne peut pas créer d’autres comptes ;
- l’utilisateur ne possède pas de droits administrateur.

## 14. E-mail d’envoi des identifiants à un adhérent

Exemple de message :

```text
Objet : Accès à l’espace adhérent ResioClage

Bonjour,

Votre candidature d’adhésion à l’association ResioClage a été acceptée.

Un compte utilisateur a été créé afin de vous permettre d’accéder à l’espace adhérent du wiki.

Identifiant : [IDENTIFIANT]
Mot de passe temporaire : [MOT_DE_PASSE]

Lien d’accès au wiki :
http://127.0.0.1/mediawiki

Vos identifiants sont personnels et ne doivent pas être partagés.

Pour des raisons de sécurité, nous vous recommandons de modifier votre mot de passe lors de votre première connexion si cette fonctionnalité est disponible.

Cordialement,

L’association ResioClage
```

## 15. Créer un utilisateur administrateur

### 15.1 Situation

Un utilisateur administrateur est un compte destiné à gérer le wiki.

Il peut modifier les pages, créer des pages, gérer l’organisation du wiki et créer des comptes utilisateurs.

### 15.2 Étapes générales

1. Se connecter avec un compte déjà administrateur.
2. Créer un compte utilisateur classique via :

```text
Spécial:Créer_un_compte
```

ou :

```text
Special:CreateAccount
```

3. Aller dans la page de gestion des droits utilisateurs.
4. Rechercher le compte nouvellement créé.
5. Ajouter le compte au groupe `sysop`.
6. Ajouter le compte au groupe `bureaucrat` si nécessaire.
7. Enregistrer les modifications.
8. Tester la connexion et les droits.

## 16. Accéder à la gestion des droits utilisateurs

La gestion des droits se fait via une page spéciale.

URL possible :

```text
http://127.0.0.1/mediawiki/index.php/Spécial:Permissions
```

ou :

```text
http://127.0.0.1/mediawiki/index.php/Special:UserRights
```

Selon la langue ou la version de MediaWiki, le nom affiché peut être différent.

Depuis cette page :

1. Saisir le nom d’utilisateur.
2. Cliquer sur le bouton de recherche ou de modification.
3. Cocher les groupes à ajouter.
4. Enregistrer.

## 17. Groupes à attribuer à un administrateur

Pour un administrateur classique :

```text
sysop
```

Pour un administrateur chargé aussi de gérer les droits :

```text
sysop
bureaucrat
```

Le groupe `bureaucrat` doit être attribué avec prudence, car il permet de gérer certains droits utilisateurs.

## 18. Vérification d’un compte administrateur

Après attribution des droits, vérifier que l’administrateur peut :

- modifier une page ;
- créer une page ;
- accéder aux pages spéciales ;
- créer un utilisateur ;
- consulter la liste des utilisateurs ;
- gérer les droits si le groupe `bureaucrat` est attribué.

## 19. Retirer les droits d’un utilisateur

Si un utilisateur ne doit plus être administrateur :

1. Se connecter avec un compte possédant les droits nécessaires.
2. Ouvrir la page `Special:UserRights`.
3. Rechercher l’utilisateur.
4. Retirer le groupe `sysop`.
5. Retirer le groupe `bureaucrat` si présent.
6. Enregistrer.
7. Vérifier que l’utilisateur n’a plus les droits administrateur.

## 20. Désactiver ou limiter un compte adhérent

MediaWiki ne fonctionne pas toujours avec un bouton simple “désactiver le compte”.

Selon le besoin, plusieurs solutions sont possibles :

- changer le mot de passe ;
- retirer les droits spécifiques si l’utilisateur en avait ;
- bloquer l’utilisateur via la page spéciale de blocage ;
- supprimer l’accès au compte selon la politique interne ;
- conserver le compte pour l’historique, mais empêcher son utilisation.

Page possible :

```text
Spécial:Bloquer
```

ou :

```text
Special:Block
```

Le blocage est utile si un compte ne doit plus pouvoir agir sur le wiki.

## 21. Créer une page privée d’administration du wiki

Pour éviter de retenir toutes les URL, il est conseillé de créer une page privée appelée :

```text
Administration du wiki
```

Contenu recommandé :

```wiki
= Administration du wiki =

Cette page regroupe les liens utiles pour administrer le MediaWiki ResioClage.

== Gestion des utilisateurs ==

* [[Spécial:Créer_un_compte|Créer un compte utilisateur]]
* [[Spécial:Liste_des_utilisateurs|Liste des utilisateurs]]
* [[Special:UserRights|Gérer les droits utilisateurs]]
* [[Special:Block|Bloquer un utilisateur]]

== Gestion du contenu ==

* [[Spécial:Pages_spéciales|Pages spéciales]]
* [[Spécial:Toutes_les_pages|Toutes les pages]]
* [[Spécial:Modifications_récentes|Modifications récentes]]
* [[Spécial:Pages_demandées|Pages demandées]]
* [[Spécial:Pages_orphelines|Pages orphelines]]

[[Accueil]]
```

Cette page ne doit pas être ajoutée dans la liste des pages publiques.

## 22. Processus complet de création d’un adhérent

```text
1. Réception de la candidature par e-mail
2. Vérification du CV
3. Étude du profil
4. Validation de la candidature
5. Création du compte via Spécial:Créer_un_compte
6. Vérification du compte
7. Envoi des identifiants par e-mail
8. Archivage de la candidature
9. Suivi du compte adhérent
```

La procédure détaillée de traitement de la candidature est décrite dans le livrable :

```text
Procédure de gestion des adhésions
```

## 23. Processus complet de création d’un administrateur

```text
1. Définir le besoin d’un nouvel administrateur
2. Créer un compte utilisateur via Spécial:Créer_un_compte
3. Aller dans Special:UserRights
4. Ajouter le groupe sysop
5. Ajouter le groupe bureaucrat uniquement si nécessaire
6. Tester les droits
7. Documenter la création du compte administrateur
```

## 24. Bonnes pratiques de gestion des comptes

Il est recommandé de :

- créer un compte nominatif par utilisateur ;
- éviter les comptes partagés ;
- utiliser des mots de passe temporaires lors de la création ;
- demander un changement de mot de passe à la première connexion si possible ;
- ne pas envoyer de mot de passe définitif en clair ;
- limiter le nombre d’administrateurs ;
- retirer les droits inutiles ;
- vérifier régulièrement la liste des utilisateurs ;
- documenter les créations et modifications de comptes.

## 25. Bonnes pratiques de sécurité

Pour sécuriser la gestion des utilisateurs :

- réserver la création de compte aux administrateurs ;
- bloquer la création de compte publique ;
- utiliser des mots de passe robustes ;
- ne pas réutiliser le même mot de passe pour plusieurs comptes ;
- vérifier les droits après chaque création ;
- ne pas attribuer `sysop` à un adhérent ;
- ne pas attribuer `bureaucrat` sans raison ;
- conserver une trace des comptes créés ;
- supprimer ou bloquer les comptes inutilisés.

## 26. Différence entre interface MediaWiki et phpMyAdmin

La création de compte via l’interface MediaWiki est recommandée car MediaWiki gère automatiquement :

- la création correcte du compte ;
- le hachage du mot de passe ;
- les tables internes ;
- la cohérence avec le système d’utilisateurs ;
- les journaux liés aux actions.

phpMyAdmin doit être réservé aux cas exceptionnels :

- perte d’accès administrateur ;
- correction d’une erreur en base ;
- dépannage avancé ;
- récupération d’un compte.

Pour une gestion normale, il faut utiliser :

```text
Spécial:Créer_un_compte
Special:UserRights
Spécial:Liste_des_utilisateurs
```

## 27. Tableau récapitulatif

| Action | Outil recommandé | Réservé à |
|---|---|---|
| Créer un adhérent | `Spécial:Créer_un_compte` | Admin |
| Créer un administrateur | `Spécial:Créer_un_compte` + `Special:UserRights` | Admin / Bureaucrat |
| Ajouter le rôle admin | `Special:UserRights` | Bureaucrat |
| Voir les utilisateurs | `Spécial:Liste_des_utilisateurs` | Admin |
| Bloquer un compte | `Special:Block` | Admin |
| Modifier les pages | Interface MediaWiki | Admin |
| Modifier la base | phpMyAdmin | Dépannage uniquement |

## 28. À retenir

La gestion des utilisateurs ResioClage doit se faire depuis l’interface MediaWiki.

Les adhérents sont créés via la page spéciale de création de compte et disposent d’un accès en lecture au contenu privé.

Les administrateurs sont créés comme des utilisateurs classiques, puis reçoivent les droits `sysop` et éventuellement `bureaucrat`.

phpMyAdmin n’est pas la méthode normale de gestion des comptes. Il doit rester une solution de secours.
