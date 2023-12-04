---
marp: true
theme: socrates
author: Pierre-Antoine Bouttier
paginate: true
footer: Git(Lab)@CNRS - pierre-antoine.bouttier@univ-grenoble-alpes.fr
---

<!-- _class: titlepage -->


![bg left:33% fit](fig/logo-git.png)
# Introduction à Git
## Cours et mise en pratique

### GitLab@CNRS - 11-12/12/2023
#### [Pierre-Antoine Bouttier](mailto:pierre-antoine.bouttier@univ-grenoble-alpes.fr)


---
# Fork et merge-requests - Divergence et demande de fusion

Jusqu’à présent, **nous avons tous travaillé dans le même dépôt (sandbox)**, et à la toute fin, sur la même branche `master`.

En principe, **il vaut mieux réserver la branche master pour la version principale et stable du projet**, et **isoler** les essais/implémentations de nouvelles fonctionnalités, corrections d’un bug etc., dans des **branches**. 

**Si vous n'êtes pas dans le projet**, mais que vous souhaitez proposer des améliorations, des corrections, bref, contribuer, Gitlab propose un mécanisme pour gérer proprement ces développements parallèles : **les merge-requests ou demandes de fusion (pull requests sous github).**

---
## Comment procéder pour faire une merge-request ? - le fork

Si vous voulez contribuer à un projet mais que vous n'en êtes pas membre : 
* vous pouvez le cloner localement
* Faire des modifs dans votre dépôt local
* `git pull`
* `git push` : erreur, vous n'avez pas les droits de contribuer. 
* Il faut donc passer par **un fork**

---
## Comment procéder pour faire une merge-request ? - le fork

Le fork est **une copie d'un projet gitlab** dans un espace de noms où vous avez les droits pour contribuer.

**Voir démo**

---
## Comment procéder pour faire une merge-request ? - le workflow

### De votre côté 

1. On forke le projet visé
2. On fait nos modifs et nos commits, que l'on push dans le projet forké. 
3. Quand on a un commit qui nous semble OK pour contirbuer au projet visé, on va faire une **demande de fusion** (menu gauche du projet forké)

### Côté projet principal

1. On fait une revue de la demande de fusion
2. On la rejette ou on l'intègre
