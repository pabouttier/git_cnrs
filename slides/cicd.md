---
marp: true
theme: socrates
author: Pierre-Antoine Bouttier
paginate: true
footer: Git(Lab)@CNRS - pierre-antoine.bouttier@univ-grenoble-alpes.fr
---

<!-- _class: titlepage -->
<style scoped>
margin-left: 10%;

</style>

![bg left:33% fit](fig/logo-gitlab.png)
# L'intégration/déploiement continue
## Automatisation des tâches avec GitLab

### GitLab@CNRS - 11-12/12/2023
#### [Pierre-Antoine Bouttier](mailto:pierre-antoine.bouttier@univ-grenoble-alpes.fr)

---
# TOC

<!-- _class: cool-list -->

1. *La CI/CD, concepts et mise en place*
2. *Publier un site web avec GitLab*
   1. *Les générateurs de site statique*
   2. *GitLab Pages*
   3. *Mise en oeuvre de la CI/CD*

---
# TOC

<!-- _class: cool-list -->

1. ***La CI/CD, concepts et mise en place***
2. *Publier un site web avec GitLab*
   1. *Les générateurs de site statique*
   2. *GitLab Pages*
   3. *Mise en oeuvre de la CI/CD*

---
## Qu'est ce que c'est ?

**Concept**
* Continuous Integration : pratique consistant **à associer à chaque modification des sources** une séries d'opérations qui seront exécutées automatiquement.
* Continuous deployment : pratique consistant **à automatiser les actions de déploiement** d'applications

---
# Intérêts

* S’assurer que le résultat de nouvelles modifications n’introduit pas de régression du code
* Anticiper différents types d’utilisation du code.
* Faciliter les développements au quotidien, les collaborations
* Identifier et corriger les erreurs plus facilement et plus rapidement
* Anticiper, planifier, tester différents environnements (debug/release, OS, paramètres ...)
* Déployer le code, fournir des "images" prêtes à l'emploi (Docker, Singularity ...)
* Économiser du temps pour les développeurs, réduire le "time-to-release" ou le "time-to-new-features"

---
## Par quelle magie ?

À chaque push vers le serveur, des tâches pré-définies par les développeurs sont exécutées.

<center>

![w:900 center](fig/ci1.png)

</center>

---
## Quelques exemples

<center>

![w:900 center](fig/ci2.png)

</center>

Ici, il y a du **déploiement continu** aussi. Mais les mécanismes sont les mêmes que pour la CI. Seule la finalité change.

---
## Comment mettre la CI/CD en place ? 

* Il suffit de créer un fichier nommé `.gitlab-ci.yml`à la racine du dépôt du projet
* Dès que ce fichier est présent, l’intégration continue est activée. Ce fichier contient la liste des tâches à effectuer : quelles actions, sur quelles machines etc.
* Définir des ***runners*** : une ou plusieurs machines (éventuellement virtuelles) sur lesquelles seront exécutés les jobs d’intégration continue.
* https://docs.gitlab.com/ee/ci/quick_start/

---
# TOC

<!-- _class: cool-list -->

1. *La CI/CD, concepts et mise en place*
2. ***Publier un site web avec GitLab***
   1. ***Les générateurs de site statique***
   2. *GitLab Pages*
   3. *Mise en oeuvre de la CI/CD*

---
# Comment fait-on un site web ? 

* Il faut d'une part le construire... :
  * Avoir du contenu (texte, audio, vidéo, images, etc.)...
  * ...agencé suivant des standards qui permettent à tout le monde d'y accéder (via leurs navigateurs) : CSS (pour la forme), HTML (pour la structure)...
  * ...le tout agrémenté de fonctions supplémentaires, implémentées dans des langages de programmation : Javascript, PHP, Python, SQL, etc.  

* ...puis le mettre à disposition : 
  * À l'aide d'un serveur web : Apache, Nginx, etc
  * Qui tourne sur un ordinateur qu'il faut bien configurer...
  * ...y compris en terme de réseau

---
# Va-t-on apprendre tout ça en 6 heures ?

Non. 

Construire et publier un site web sur mesure est un métier (parfois, plusieurs).  

---
# SGC, SSG, kezako ?

Il existe des systèmes (logiciels) qui permettent de **construire** des sites webs (=pages HTML) *simplement*

* Les Systèmes de Gestion de Contenu (SGC ou CMS en anglais) : 
  * Ensemble d'outils pour créer et gérer des sites web complets et complexes
  * Souvent **dynamiques**, *i.e.* le rendu (=pages HTML) est généré lorsque le visiteur le consulte
  * Nécessitent, quasiment tout le temps, des bases de données
  * Exemples : wordpress, drupal

---
# SGC, SSG, kezako ?

* Les Générateurs de sites statiques (SSG en anglais) : 
  * Construit, en quelques commandes/scripts, un site web à partir de fichiers textes (contenu et templates/css) et medias
  * Le site construit est **statique**
  * Les données d'entrée sont stockées dans un système de fichiers
  * Exemple : **hugo**, jekyll, pelican, gatsby, eleventy

---
# Pourquoi nous intéressons-nous aux SSGs ? Pros

* Les avantages 
  * Légéreté et rapidité : Les pages HTML sont déjà écrites et stockées, il n'y a pas besoin de les construire à la volée
  * Sécurité : faible surface d'attaque
  * Contrôle de version du contenu : seuls des fichiers textes (et médias) sont nécessaires, donc on peut utiliser Git (et GitLab) pour gérer l'évolution du site. 
  * Maintenance légère : comparé à un CMS, la maintenance du SSG en lui-même est nulle. 
* Quand ne pas les utiliser : 
  * Contenu fortement dynamique
  * Traitement d'entrées utilisateurs (**e.g.** formulaires)
  * Nécessitent quelques compétences (mais les CMS aussi)
  * **Nota Bene** : Site statique ≠ [petit site](https://gricad-doc.univ-grenoble-alpes.fr/hpc/)

---
# Les différents SSGs

* Comme déjà mentionné, il existe pléthore de SSGs
* Ils peuvent être écrits dans différents langages : python, php, javascript, etc.
* Fonctionnement souvent semblable
* Nous utiliserons dorénavant [le SSG Hugo](https://gohugo.io/) : communauté active (plein d'avantages à ça), s'installe assez facilement sur toutes les plateformes, rapide, a pas mal de fonctionnalités, open source. 

---
<!-- _class: transition -->

Création du site

---
# En pratique




---
# TOC

<!-- _class: cool-list -->

1. *La CI/CD, concepts et mise en place*
2. ***Publier un site web avec GitLab***
   1. *Les générateurs de site statique*
   2. ***GitLab Pages***
   3. *Mise en oeuvre de la CI/CD*

---
# TOC

<!-- _class: cool-list -->

1. *La CI/CD, concepts et mise en place*
2. ***Publier un site web avec GitLab***
   1. *Les générateurs de site statique*
   2. *GitLab Pages*
   3. ***Mise en oeuvre de la CI/CD***