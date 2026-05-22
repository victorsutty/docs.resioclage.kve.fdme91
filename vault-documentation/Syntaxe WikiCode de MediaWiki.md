
## 1. Objectif de la documentation

Cette documentation présente les principaux éléments de **wikicode** utilisés dans le MediaWiki de l’association **ResioClage**.

Elle sert de mémo technique pour créer, modifier et structurer les pages du wiki de manière propre et cohérente.

Le wiki utilise principalement :

- des titres ;
- des paragraphes ;
- des listes ;
- des liens internes ;
- des tableaux ;
- des modèles ;
- des blocs de code ;
- des pages liées.

---

## 2. Créer un titre de page

Dans MediaWiki, le titre principal d’une page peut être écrit avec un seul signe égal de chaque côté.

### Exemple

```wiki
= Accueil =
```

Résultat : un grand titre de page.

### Exemple utilisé dans le wiki

```wiki
= Développement informatique =
```

---

## 3. Créer des sous-titres

Les sous-titres permettent d’organiser une page en plusieurs parties.

### Niveau 2

```wiki
== Introduction ==
```

### Niveau 3

```wiki
=== Développeur ===
```

### Niveau 4

```wiki
==== Missions principales ====
```

### Bonnes pratiques

Il faut éviter de mettre trop de niveaux de titres. Pour notre wiki, les niveaux les plus utilisés sont :

```wiki
= Titre de la page =
== Grande partie ==
=== Sous-partie ===
```

---

## 4. Écrire un paragraphe

Pour écrire un paragraphe, il suffit d’écrire du texte normalement dans la page.

### Exemple

```wiki
Le développement informatique regroupe les métiers liés à la création de logiciels, d’applications web, d’applications mobiles et de sites internet.
```

Pour créer un nouveau paragraphe, il faut laisser une ligne vide entre deux blocs de texte.

### Exemple

```wiki
L’informatique occupe une place essentielle dans les entreprises modernes.

Elle permet de gérer les données, de communiquer et de sécuriser les informations.
```

---

## 5. Créer une liste à puces

Les listes à puces sont utilisées pour présenter des éléments de manière claire.

### Syntaxe

```wiki
* Élément 1
* Élément 2
* Élément 3
```

### Exemple utilisé dans le wiki

```wiki
* [[Développement informatique]]
* [[Administration Réseau et système]]
* [[Parcours après un BTS SIO]]
```

---

## 6. Créer une liste numérotée

Les listes numérotées sont utiles pour décrire une procédure ou des étapes.

### Syntaxe

```wiki
# Première étape
# Deuxième étape
# Troisième étape
```

### Exemple

```wiki
# Le candidat rédige un e-mail de candidature.
# Il ajoute son CV en pièce jointe.
# Il envoie l’e-mail à l’adresse de réception.
# L’association étudie la candidature.
# Si la candidature est acceptée, un compte utilisateur est créé.
```

---

## 7. Créer un lien interne

Un lien interne permet de relier une page du wiki à une autre page.

### Syntaxe

```wiki
[[Nom de la page]]
```

### Exemple

```wiki
[[Sécurité informatique]]
```

Ce lien renvoie vers la page **Sécurité informatique**.

### Exemple avec phrase

```wiki
Voir aussi : [[Réglementation informatique]].
```

---

## 8. Créer un lien interne avec un texte différent

Il est possible d’afficher un texte différent du nom réel de la page.

### Syntaxe

```wiki
[[Nom de la page|Texte affiché]]
```

### Exemple

```wiki
Pour en savoir plus, consulter la page [[Réglementation informatique|réglementation liée au numérique]].
```

Résultat visible : **réglementation liée au numérique**.

---

## 9. Créer des liens entre les pages du projet

Dans notre MediaWiki, les liens internes sont importants pour faciliter la navigation.

### Exemples de liens utilisés

```wiki
[[Accueil]]
[[Objectifs de l'Association]]
[[Adhérer]]
[[Développement informatique]]
[[Administration Réseau et système]]
[[Parcours après un BTS SIO]]
[[Logiciels et licences]]
[[Open source et logiciel propriétaire]]
[[Sécurité informatique]]
[[Réglementation informatique]]
[[CNIL]]
[[Loi HADOPI]]
[[Informatique en entreprise]]
[[Rôle d’une DSI]]
[[Prestataire informatique]]
[[Grandes sociétés informatiques]]
```

### Bonnes pratiques

Il est recommandé d’ajouter une section **Pages liées** dans les pages importantes.

### Exemple

```wiki
== Pages liées ==

* [[CNIL]]
* [[Loi HADOPI]]
* [[Logiciels et licences]]
* [[Sécurité informatique]]
```

---

## 10. Créer un retour vers l’accueil

En bas de chaque page, on ajoute un lien vers la page d’accueil.

### Exemple

```wiki
[[Accueil]]
```

Cela permet de revenir facilement au sommaire principal du wiki.

---

## 11. Mettre du texte en gras

### Syntaxe

```wiki
'''Texte en gras'''
```

### Exemple

```wiki
'''La sécurité informatique''' est essentielle pour protéger les données.
```

---

## 12. Mettre du texte en italique

### Syntaxe

```wiki
''Texte en italique''
```

### Exemple

```wiki
''Le RGPD encadre l’utilisation des données personnelles.''
```

---

## 13. Afficher du code court dans une phrase

Pour afficher une commande, une adresse mail ou un chemin de fichier, on peut utiliser la balise `<code>`.

### Exemple

```wiki
Le candidat doit envoyer sa candidature à l’adresse <code>e.giraud@cybersec.fdme91.fr</code>.
```

### Exemple avec un chemin

```wiki
Le fichier de configuration se trouve dans <code>C:\wamp64\www\mediawiki\LocalSettings.php</code>.
```

---

## 14. Afficher un bloc de texte préformaté

La balise `<pre>` permet d’afficher un bloc de texte en conservant la mise en forme.

Elle est utile pour les exemples d’e-mails, les procédures ou les extraits de configuration.

### Exemple

```wiki
<pre>
Objet : Candidature d’adhésion à l’association ResioClage

Bonjour,

Je souhaite déposer ma candidature afin d’adhérer à l’association ResioClage.

Vous trouverez mon CV en pièce jointe.

Cordialement,
[Nom Prénom]
</pre>
```

---

## 15. Créer un tableau simple

Les tableaux permettent de présenter des comparaisons ou des informations structurées.

### Syntaxe de base

```wiki
{| class="wikitable" style="width:100%;"
|-
! Colonne 1
! Colonne 2
|-
| Élément A
| Élément B
|-
| Élément C
| Élément D
|}
```

### Exemple utilisé dans le wiki

```wiki
{| class="wikitable" style="width:100%;"
|-
! Critère
! Informatique personnelle
! Informatique d’entreprise
|-
| Nombre d’utilisateurs
| Une ou quelques personnes
| Plusieurs salariés ou services
|-
| Sécurité
| Protection individuelle
| Protection des données de l’organisation
|}
```

---

## 16. Utiliser le modèle `Page thématique`

Le wiki utilise un modèle pour uniformiser les pages générales.

### Nom du modèle

```wiki
Modèle:Page thématique
```

### Code du modèle

```wiki
{| class="wikitable" style="width:100%;"
|-
! colspan="2" style="text-align:center;" | Page thématique
|-
! Thème
| {{{theme}}}
|-
! Objectif
| {{{objectif}}}
|-
! Notions abordées
| {{{notions}}}
|}
```

### Exemple d’utilisation

```wiki
{{Page thématique
|theme=Sécurité informatique
|objectif=Comprendre les principaux risques informatiques et connaître les bonnes pratiques permettant de protéger les utilisateurs, les données et les équipements.
|notions=
* Confidentialité
* Intégrité
* Disponibilité
* Virus et logiciels malveillants
* Mots de passe
* Sauvegardes
}}
```

### Pages concernées

Ce modèle peut être utilisé pour :

```wiki
[[Logiciels et licences]]
[[Open source et logiciel propriétaire]]
[[Domaines d’utilisation de l’informatique]]
[[Sécurité informatique]]
[[Réglementation informatique]]
[[CNIL]]
[[Loi HADOPI]]
[[Informatique en entreprise]]
[[Rôle d’une DSI]]
[[Prestataire informatique]]
[[Grandes sociétés informatiques]]
[[Parcours après un BTS SIO]]
[[Objectifs de l'Association]]
[[Adhérer]]
```

---

## 17. Utiliser le modèle `Domaine métier`

Le wiki utilise aussi un modèle pour les pages qui présentent une grande famille de métiers.

### Nom du modèle

```wiki
Modèle:Domaine métier
```

### Code du modèle

```wiki
{| class="wikitable" style="width:100%;"
|-
! colspan="2" style="text-align:center;" | Domaine métier
|-
! Domaine
| {{{domaine}}}
|-
! Objectif de la page
| {{{objectif}}}
|-
! Compétences générales
| {{{competences}}}
|-
! Métiers présentés
| {{{metiers}}}
|}
```

### Exemple d’utilisation

```wiki
{{Domaine métier
|domaine=Développement informatique
|objectif=Présenter les principaux métiers liés à la création, au développement, à l’analyse et à la maintenance de logiciels, d’applications et de solutions numériques.
|competences=
* Programmation
* Logique et algorithmique
* Bases de données
* Gestion de projet
* Tests et maintenance
|metiers=
* Développeur
* Chef de projet informatique
* Ingénieur DevOps
* Data Scientist
* Data Analyst
* Responsable logiciel
}}
```

### Pages concernées

```wiki
[[Développement informatique]]
[[Administration Réseau et système]]
```

---

## 18. Structure recommandée pour une page thématique

Pour garder un wiki clair, les pages thématiques peuvent suivre cette structure.

### Exemple de structure

```wiki
= Nom de la page =

{{Page thématique
|theme=Nom du thème
|objectif=Objectif de la page.
|notions=
* Notion 1
* Notion 2
* Notion 3
}}

== Introduction ==

Texte d’introduction.

== Développement ==

Contenu principal de la page.

== Exemple dans l’association ResioClage ==

Exemple d’application dans le contexte de l’association.

== Pages liées ==

* [[Page liée 1]]
* [[Page liée 2]]

== À retenir ==

Résumé des points importants.

[[Accueil]]
```

---

## 19. Structure recommandée pour une page métier

Les pages métiers du wiki regroupent plusieurs métiers dans une seule page.

### Exemple de structure

```wiki
= Développement informatique =

{{Domaine métier
|domaine=Développement informatique
|objectif=Présenter les principaux métiers liés au développement informatique.
|competences=
* Programmation
* Bases de données
* Tests
|metiers=
* Développeur
* Chef de projet informatique
* Data Analyst
}}

== Présentation du domaine ==

Texte de présentation.

== Métiers du développement informatique ==

=== Développeur ===

Présentation du métier.

Ses missions principales sont :
* mission 1 ;
* mission 2 ;
* mission 3.

=== Chef de projet informatique ===

Présentation du métier.

== À retenir ==

Résumé.

[[Accueil]]
```

---

## 20. Créer une page d’accueil structurée

La page d’accueil sert de sommaire principal.

### Exemple

```wiki
= Accueil =

Bienvenue au sein de l'association ResioClage !

Ce wiki a pour objectif de présenter les principaux éléments liés aux métiers de l’informatique, à la sécurité, à la réglementation et à l’informatique en entreprise.

== ResioClage ==

Cette section présente l’association ResioClage, ses objectifs, son rôle et les modalités pour y adhérer.

* [[Objectifs de l'Association]]
* [[Adhérer]]

== Métiers de l’informatique ==

Cette section regroupe les principales informations sur les métiers de l’informatique.

* [[Développement informatique]]
* [[Administration Réseau et système]]
* [[Parcours après un BTS SIO]]
```

---

## 21. Gestion des pages rouges

Dans MediaWiki, un lien rouge indique que la page n’existe pas encore.

### Exemple

```wiki
[[Adhérer]]
```

Si la page n’existe pas, le lien apparaît en rouge. Il suffit de cliquer dessus pour créer la page.

---

## 22. Bonnes pratiques de rédaction

Pour garder un wiki professionnel, il est recommandé de :

- créer des titres clairs ;
- utiliser des paragraphes courts ;
- ajouter des listes pour les informations importantes ;
- créer des liens internes entre les pages ;
- ajouter une section **Pages liées** ;
- ajouter une section **À retenir** ;
- terminer par un lien vers `[[Accueil]]` ;
- utiliser les modèles pour garder une présentation cohérente ;
- éviter de copier-coller du contenu sans le reformuler ;
- citer les sources dans la page `[[Sources]]`.

---

## 23. Pages importantes du wiki ResioClage

Les principales pages du wiki sont :

```wiki
[[Accueil]]
[[Objectifs de l'Association]]
[[Adhérer]]
[[Développement informatique]]
[[Administration Réseau et système]]
[[Parcours après un BTS SIO]]
[[Logiciels et licences]]
[[Open source et logiciel propriétaire]]
[[Domaines d’utilisation de l’informatique]]
[[Sécurité informatique]]
[[Réglementation informatique]]
[[CNIL]]
[[Loi HADOPI]]
[[Informatique en entreprise]]
[[Rôle d’une DSI]]
[[Prestataire informatique]]
[[Grandes sociétés informatiques]]
[[Sources]]
[[Historique des modifications]]
```

---

## 24. Résumé

Le wikicode permet de structurer les pages du MediaWiki avec des titres, des listes, des liens internes, des tableaux et des modèles.

Dans le wiki ResioClage, les éléments les plus importants sont :

- les liens internes pour relier les pages ;
- les modèles pour uniformiser la présentation ;
- les sections `Introduction`, `Pages liées` et `À retenir` ;
- le lien `[[Accueil]]` en bas de page.

Cette organisation permet d’obtenir un wiki clair, cohérent, professionnel et facile à maintenir.
