---
marp: true
theme: socrates
author: Pierre-Antoine Bouttier
paginate: true
footer: Git(Lab)@CNRS - pierre-antoine.bouttier@univ-grenoble-alpes.fr
---

<!-- _class: titlepage -->


![bg left:33% fit](fig/logo-git.png)
# Miscellanées

### GitLab@CNRS - 09-10/01/2024
#### [Pierre-Antoine Bouttier](mailto:pierre-antoine.bouttier@univ-grenoble-alpes.fr)


---
# `git log`, maoins austère

```
$ git log --graph --oneline --branches --decorate
```

---

# Le fichier `.gitignore`

---
## Le fichier `gitignore`

Parfois, nous voulons que certains types fichiers ne soient jamais considérés/vus par git : binaires, `*.pyc`, fichiers temporaires, etc.

Le fichier nommé `.gitignore` permet d'indiquer les fichiers à ne pas considérer.

**Attention** : si des fichiers décrits dans le .gitignore sont déjà suivis par git, ils le resteront.

---
## Le fichier `.gitignore`

On peut avoir un `.gitignore` valable pour tous nos dépôts locaux 
```shell
$ git config --get core.excludesfile <chemin vers le .gitgnore>
$ ... # éditer et sauver le .gitignore au chemin indiqué au-dessus
```
Ou à la racine de chaque dépôt. Il sera valable pour tous les sous-dossiers du dépôt.

Dans chaque sous-dossier, vous pouvez également sauvegarder un `.gitignore` qui s'appliquera à ce sous-dossier et aux répertoires enfants.

[Quelques modèles de `.gitignore`](https://github.com/github/gitignore)


---

# Les alias Git

---
## Toujours les mêmes commandes à taper...

Si vous utilisez git régulièrement, vous verrez que vous ferez sans cesse les mêmes commandes. 

On peut définir des alias aux commandes que vous utilisez le plus : ce sont des commandes raccourcies. Par exemple : 
```shell
$ git cm "..." # alias de git commit -m "...
```

---
## Définition des alias

Pour définir, vos alias, vous pouvez éditer le fichier `~/.gitconfig`, en ajoutant, par exemple : 
```git
[alias]
  a = add --all
  b = branch
  c = commit
  ca = commit -a
  cm = commit -m
  cam = commit -am
  o = checkout
  ob = checkout -b
```
---

# La commande `git diff`

---
# La commande `git diff`

La commande git diff permet de comparer n’importe quelle version d’un fichier avec n’importe quelle autre.

Différence entre la version indexée et la version modifiée :
```shell
git diff monFichier
```
Différence entre la version indexée et la version de référence
```shell
git diff --staged nomFichier
```
Différence entre la version courante et celle d'un commit précédent
```shell
git diff d18e05e80fe11c6206e2cb5a0624b477a6c31226 monFichier
```


---

# Les licences (logicielles)

---
## Les licences (logicielles)

Si vous publiez/diffusez votre code/un rapport/site web, il est important d'établir un **contrat** entre **vous**, son créateur, le **propriétaire** du code (souvent l'employeur, dans le cadre professionnel) et les **utilisateurs**. C'est le rôle de la **licence**.

Il y a les licences logicielles (GPL, MIT, BSD, CeCiLL, etc.) et les licences plus larges (Creative Commons). Chaque licence peut avoir (et a souvent) des déclinaisons. Il y a des libres, open-source, propriétaires...

Bref, c'est compliqué. Dans la recherche, les instituts fournissent des directives à appliquer. Rapprochez-vous des services juridiques pour en savoir plus. 

---
## Les licences (logicielles) - En pratique

Autant il peut être compliqué de savoir quelle licence utiliser, autant il est très facile d'ajouter un fichier de licence à votre projet Gitlab.

**Voir démo**

[Un document sur les licences logicielles qui peut être très utile](fig/INRIA_recueil_fiches_licences_libres_vf-4.pdf)

---

# Les issues

---
## Les issues Gitlab, gestionnaire de problème et d'échanges

Le gestionnaire de problème est un outil standard des forges qui permet le suivi efficace de l’évolution des bugs (du signalement à la résolution), mais aussi de proposer, de discuter de nouvelles idées, de suivre l’ajout de nouvelles fonctionnalités, leur évolution (dans des branches).

Bref, c’est un outil indispensable à la vie du projet, qu’il faut exploiter. Il est accessible via le menu Issues, avec en particulier :
- List et Board pour visualiser l’ensemble des problèmes déja déposés, les classer, les commenter ... 
- Milestones : définition d’étapes de développement, planification du projet

---
## Conseils et fonctionnalités utiles sur les issues

- Créez des labels explicites pour classer vos issues (documentation, bugs, newideas...).
- Utilisez le markdown pour rédiger vos issues. 
- Vous pouvez mentionner dans les issues ou dans les messages de commit les autres participants au projet via la chaine @username. Cela entrainera l’envoi d’un mail à la personne concernée, l’ajout d’une tâche dans sa todo-list
- faire référence à une issue dans une autre issue ou un message de commit via `#id`, id étant le numéro de l’issue
- clore automatiquement une issue via un message de commit. Il suffit qu’il contienne la chaîne `Fix #id` (ou un des autres mots-clés mentionnés ici :
https://docs.gitlab.com/ee/user/project/issues/automatic_issue_closing.html).

---
# Fork et merge-requests - Divergence et demande de fusion

Jusqu’à présent, **nous avons tous travaillé dans le même dépôt (sandbox)**, et à la toute fin, sur la même branche `master`.

En principe, **il vaut mieux réserver la branche master pour la version principale et stable du projet**, et **isoler** les essais/implémentations de nouvelles fonctionnalités, corrections d’un bug etc., dans des **branches**. 

**Si vous n'êtes pas dans le projet**, mais que vous souhaitez proposer des améliorations, des corrections, bref, contribuer, Gitlab propose un mécanisme pour gérer proprement ces développements parallèles : **les merge-requests ou demandes de fusion (pull requests sous github).**

---
## Comment procéder pour faire une merge-request ? - le fork

Si vous voulez contribuer à un projet mais que vous n'en êtes pas membre : 
- vous pouvez le cloner localement
- Faire des modifs dans votre dépôt local
- `git pull`
- `git push` : erreur, vous n'avez pas les droits de contribuer. 
- Il faut donc passer par **un fork**

---
## Comment procéder pour faire une merge-request ? - le fork

Le fork est **une copie d'un projet gitlab** dans un espace de noms où vous avez les droits pour contribuer.

**Voir démo**

---
## Comment procéder pour faire une merge-request ? - le workflow

### De votre côté 

1. On forke le projet visé
2. on clone localement le dépôt associé au fork, on fait nos modifs et nos commits, que l'on push dans le projet forké. 
3. Quand on a un commit qui nous semble OK pour contirbuer au projet visé, on va faire une **demande de fusion** (menu gauche du projet forké)

### Côté projet principal

1. On fait une revue de la demande de fusion
2. On la rejette ou on l'intègre


---
# `git stash`, pour mettre de côté des changements non-validés

Parfois, pour des opérations de merge, par exemple, il faut une copie de travail sans modifications. 

Si jamais ce n'est pas le cas, `git stash` vous aide. Il peut mettre de côté les modifications en cours dans l'espace de travail. 

---

# `git stash`

```
$ git status
On branch development
Your branch is up to date with 'origin/development'.

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   README.md

no changes added to commit (use "git add" and/or "git commit -a")
$ git stash
$ git status
On branch development
Your branch is up to date with 'origin/development'.

nothing to commit, working tree clean
```

---
# `git stash`

- `git stash` sauvegarde vos modifications actuelles de votre copie de travail et les met de côté
- `git stash list` : liste l'ensemble de vos jeux de modif.
- `git stash pop` les réapplique à votre copie de travail et supprime ce jeu de modif. dans l'espace de stash
- `git stash apply` fait la même chose mais les garde dans l'espace de stash
- `git stash save "message"` permet d'ajouter un message à ce jeu de modification
- `git stash pop stash@{2}` : applique un jeu de modif précis

---
# `git rebase`, pour linéariser l'historique

[Thanks to minimum.fr](https://www.miximum.fr/blog/git-rebase/)

Une situation classique : 
```
A---B---C---D ← main
     \
      E---F---G ← discussion
```

Après un `git merge` :
```
$ git switch main
$ git merge discussion

A---B---C---D---H ← main
     \         /
      E---F---G ← discussion
```

---
# `git rebase`, pour linéariser l'historique

`git rebase` va transplanter la branche discussion sur la branche `main` (à partir du commit D) : 
```
$ git rebase main discussion
A---B---C---D ← main
             \
              E---F---G ← discussion
```

La fusion dans `main` est maintenant triviale : 
```
$ git switch main
$ git merge discussion

A---B---C---D---E---F---G ← master
                         \
                          discussion
```

---
# À quoi sert `git rebase` ? 

 - conserver un historique propre ;
- corriger des erreurs de fusion ;
- faciliter le travail collaboratif ;
- faciliter les fusions sur les branches qui nécessitent un très long développement.

---
# Cas pratique 

```
          F---G ← bug2
         /
A---B---E---H---I ← main
     \
      C---D ← bug1
```

Avec un `rebase` avant chaque fusion, on obtient : 

```
A---B---E---H---I---C---D---F---G ← main
```

---
# Détails des commandes 

```
$ git rebase main bug1
$ git switch master
$ git merge bug1
$ git branch -d bug1
$ git rebase main bug2
$ git switch main
$ git merge bug2
$ git branch -d bug2
```

---
# Remarque

**Soyez très vigilants en rebasant des commits qui ont déjà été poussés sur un dépôt public.**

Quand vous rebasez des données, vous abandonnez les commits existants et vous en créez de nouveaux qui sont similaires mais différents (e.g. hash différents).
