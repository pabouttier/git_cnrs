+++
template = "page.html"
title = "Exercices de la formation"
insert_anchor_links = "right"
+++

# Exercices

Cette page regroupe les mises en pratique proposées durant la formation sur la plateforme [GRICAD-GitLab](https://gricad-gitlab.univ-grenoble-alpes.fr). Elle est découpée en deux parties qui suivent le déroulé du stage :

- **Partie A — Git** : les exercices se déroulent **en local, sur votre machine**, dans la console. On y manipule les concepts fondamentaux de Git (commits, historique, branches).
- **Partie B — GitLab** : les exercices se déroulent **sur la forge** et avec les **dépôts distants**. On y manipule la plateforme web et le travail collaboratif.

Les exercices sont progressifs (niveau 1 : seul, niveau 2 : en binôme, niveau 3 : en groupe). Cochez les objectifs au fur et à mesure pour suivre votre progression.

> **Prérequis** : un compte sur la plateforme et les outils installés (voir la page [Prérequis](/git_cnrs/prerequis/)).

---

# Partie A — Git en pratique (local, ligne de commande)

Cette partie correspond au support [Introduction à Git](/git_cnrs/slides/git.html). Le but est de manipuler Git **sans dépôt distant**, dans un dossier de votre machine. Vous travaillerez dans un dépôt `formation` que vous créez vous-même.

> **Astuce** : à chaque étape, après chaque commande, lancez `git status` pour observer ce qui change. C'est « votre nouvel ami ».

---

## Niveau 1 — Se familiariser avec Git

### Exercice A.1 — Configurer Git

Avant de commencer, renseignez votre identité. Utilisez de préférence le **même nom et la même adresse e-mail que votre compte GitLab** (important pour associer correctement vos commits lors de la partie B).

```shell
$ git config --global user.name "Prénom Nom"
$ git config --global user.email prenom.nom@exemple.fr
$ git config --global core.editor code   # votre éditeur favori : code, gedit, vim...
```

1. Vérifiez ce qui est enregistré : `git config --global --list`.
2. Retrouvez le fichier de configuration dans `~/.gitconfig` grâce à `git config --global --edit`.
3. Notez que ces réglages sont **globaux** : ils seront prioritairement utilisés pour tous vos dépôts (ils peuvent être surchargés par projet, ce n'est pas l'objet ici).

**Objectif validé** : savoir configurer son identité Git une fois pour toutes.

### Exercice A.2 — Créer son premier dépôt

Git agit dans un **dépôt** (un dossier suivi par Git). Créez le vôtre :

```shell
$ cd                        # retour dans votre répertoire personnel
$ mkdir formation           # le dossier qui contiendra nos fichiers
$ cd formation
$ git init .                # git init sans le '.' fait la même chose
```

1. Vérifiez qu'un dossier `.git` est apparu : `ls -a`. Que contient-il ?
2. Créez un premier fichier `README.md` (avec un titre) et observez l'état du dépôt avec `git status`.
3. **Testez aussi** : placez-vous dans un dossier qui contient déjà des fichiers et lancez-y `git init`. Git accepte d'initialiser un dépôt dans un dossier non vide.

> **À retenir** : Git reconnaît qu'il est dans un dépôt si un dossier `.git` existe dans le dossier courant ou dans un **dossier parent**. Git ne versionne que des fichiers : un dossier vide ne sera pas suivi.

**Objectif validé** : savoir créer un dépôt local avec `git init`.

### Exercice A.3 — Les trois états d'un fichier (status, add, commit)

Ce premier cycle vous fait vivre les **trois états** d'un fichier sous Git : *modifié* → *indexé (staged)* → *committé*.

1. Créez le fichier `README.md` et ajoutez-y quelques lignes.
2. `git status` : le fichier est-il suivi ? Dans quel état ?
3. Dites à Git de surveiller ce fichier : `git add README.md`, puis `git status`.
4. Sauvegardez une première version : `git commit -m "Ajout du README"`, puis `git status`.
5. Modifiez le fichier, constatez qu'il repasse en état *modifié*, ré-indexez et committez.
6. Consultez l'historique construit : `git log`.

**Objectif validé** : faire un cycle complet *modifier → indexer → committer* et lire l'historique.

### Exercice A.4 — Comprendre la zone de staging

La **zone d'index/staging** regroupe ce qui fera partie du **prochain commit**. Testez son fonctionnement :

1. Créez deux fichiers `a.md` et `b.md`.
2. Indexez-en un seul : `git add a.md`.
3. `git commit -m "Un seul fichier"`, puis observez : seul `a.md` a été committé, `b.md` est toujours *non suivi*.
4. Indexez maintenant les deux et committez.

Puis jouez avec les commandes d'annulation :
5. Annulez la mise à l'index d'un fichier (le « dé-stage ») : `git reset fichier.md` (puis re-indexez-le).
6. Annulez les modifications d'un fichier en revenant au dernier commit : modifiez-le puis `git restore fichier.md` (ou `git checkout -- fichier.md`).

> **À retenir** : ce n'est pas parce qu'un fichier est modifié qu'il sera committé. Avant chaque commit, il faut **dire à Git** quels changements sauvegarder : c'est le rôle de `git add`.

**Objectif validé** : maîtriser le rôle de la zone de staging et savoir annuler un `add`.

### Exercice A.5 — Construire un historique propre

Un **bon commit** est *atomique* : il ne doit contenir qu'un ensemble de modifications qui *font sens* ensemble. Les messages doivent **donner du sens** à l'historique.

1. Créez trois fichiers correspondant à trois sujets distincts (ex. `intro.md`, `conclusion.md`, `bib.md`).
2. Committez-les **séparément**, en indexant fichier par fichier, avec des messages explicites :
   ```shell
   $ git add intro.md
   $ git commit -m "Ajout de l'introduction"
   $ git add conclusion.md
   $ git commit -m "Ajout de la conclusion"
   ```
3. Pour les fichiers déjà connus de Git, indexez et committez en une fois : `git commit -am "message"`.
4. Pour indexer tous les changements en une fois (à utiliser **avec précaution**) : `git add -A`.
5. **Bonus — corriger le dernier commit** : modifiez un fichier, indexez-le, puis `git commit --amend` pour l'intégrer au commit précédent (et éventuellement en changer le message).

**Objectif validé** : produire des commits atomiques avec des messages explicites, et corriger son dernier commit.

### Exercice A.6 — Naviguer dans l'historique

Maintenant que vous avez un historique, apprenez à le parcourir et à « voyager dans le temps ».

1. Parcourez l'historique : `git log`. Repérez les **hash** de commits, le `HEAD` et la branche `main`.
2. Utilisez des vues plus lisibles : `git log --oneline`, `git log --graph --oneline`.
3. **Revenir en arrière proprement — `git revert`** : inversez les changements d'un intervalle de commits sans toucher à l'historique :
   ```shell
   $ git revert HEAD~3..HEAD        # inverse les 3 derniers commits
   ```
   Observez : un **nouveau commit** est créé, l'historique existant n'est pas détruit.
4. **Annuler les modifications d'un fichier** (retour au dernier commit) : `git restore fichier.md`.
5. **Bonus — se placer sur un ancien commit** : `git checkout <hash>` (attention, `HEAD` devient *détaché*), puis repartez proprement en créant une branche : `git switch -c nouveau_depart`.

> **À retenir** : `git revert` est, dans la plupart des cas, la **bonne façon** d'annuler des modifications : on enrichit l'historique au lieu de le réécrire.

**Objectif validé** : lire un historique et annuler proprement des modifications.

### Exercice A.7 — Les fichiers supprimés et les exclus

1. Supprimez un fichier suivi **du dépôt et du disque** :
   ```shell
   $ git rm fichier_a_supprimer
   $ git commit -m "Suppression du fichier"
   ```
2. Supprimez un fichier **du dépôt mais pas du disque** (`git rm --cached`) — utile pour arrêter de suivre un fichier tout en le gardant. Committez.
3. **Bonus — `.gitignore`** : créez un fichier `.gitignore` à la racine et ajoutez-y des motifs (ex. `*.log`, `dossier_temporaire/`). Vérifiez que ces fichiers n'apparaissent plus dans `git status`.

**Objectif validé** : supprimer des fichiers du dépôt et exclure des fichiers du suivi.

### Exercice A.8 — Branches et fusion

Une **branche** est un pointeur vers un commit. Elle permet de développer en parallèle **sans modifier** la version principale.

1. Créez la branche `newtest` et placez-vous dessus : `git branch newtest` puis `git switch newtest` (ou directement `git switch -c newtest`).
2. Faites quelques commits dessus. Vérifiez où pointe `HEAD` : `git branch`.
3. Revenez sur `main` : `git switch main`. Observez que le contenu des fichiers a changé (ils sont revenus à l'état de `main`).
4. Intégrez les développements de `newtest` dans `main`. **On se place d'abord sur la branche cible**, puis on fusionne :
   ```shell
   $ git switch main
   $ git merge newtest
   ```
   - Si rien ne diverge, la fusion se fait sans commit (fast-forward) ;
   - si les branches ont divergé, un *commit de fusion* est créé.
5. Supprimez ensuite la branche devenue inutile : `git branch -d newtest`.
6. Observez le résultat final : `git log --graph --oneline`.

**Objectif validé** : créer, fusionner et supprimer des branches.

### Exercice A.9 — Résoudre un conflit de fusion (forcé)

1. Depuis `main`, créez deux branches `a` et `b` (`git switch -c a`, puis revenez et créez `b`).
2. Sur chaque branche, modifiez **la même ligne** d'un fichier partagé (ex. la première ligne du `README.md`) et committez.
3. Placez-vous sur `a` et fusionnez `b` : un **conflit** doit être signalé :
   ```shell
   $ git switch a
   $ git merge b
   ```
4. Ouvrez le fichier en conflit. Repérez les marqueurs :
   ```
   <<<<<<< HEAD
   (contenu de la branche a)
   =======
   (contenu de la branche b)
   >>>>>>> b
   ```
5. Éditez le fichier à la main pour ne garder que le contenu souhaité (supprimez les marqueurs), puis :
   ```shell
   $ git add fichier_regle
   $ git commit
   ```
   Le conflit est résolu, la fusion est complète.

**Objectif validé** : détecter et résoudre un conflit en ligne de commande.

### Pour aller plus loin (Git)

Ces notions n'ont pas été présentées en détail dans le cours ; explorez-les par vous-même :
- `git diff` : visualiser les modifications avant de les committer (essayez aussi `git diff --cached`).
- `git tag` : poser une étiquette sur un commit (utile pour marquer une version).
- `git stash` : mettre de côté temporairement des modifications pour changer de contexte.
- `git rebase` et `git cherry-pick` : réorganiser l'historique (**avec précaution**, le cours ne les a pas abordés).
- Les **interfaces graphiques** à Git : [git-scm.com/downloads/guis](https://git-scm.com/downloads/guis).

---

# Partie B — GitLab en pratique (forge, distant, collaboratif)

Cette partie correspond aux supports [Introduction à GitLab](/git_cnrs/slides/gitlab.html) et [Git & GitLab — les dépôts distants](/git_cnrs/slides/git_gitlab.html). Elle se déroule sur la plateforme [GRICAD-GitLab](https://gricad-gitlab.univ-grenoble-alpes.fr) et alterne interface web et ligne de commande.

Pour cette partie, vous créerez un projet `sandbox_votre_login` (où `votre_login` est votre identifiant), que vous réutiliserez tout au long des exercices.

---

## Niveau 1 — Se familiariser avec la forge

### Exercice B.1 — Première connexion et profil

1. Connectez-vous à <https://gricad-gitlab.univ-grenoble-alpes.fr> avec vos identifiants.
   - **Communauté ESR Grenoble** (onglet *LDAP UGA*) : accès complet à tous les outils ;
   - les comptes **externes** (onglet *Standard*) ne peuvent pas créer de groupes/projets.
2. Rendez-vous dans votre profil (*Settings*), complétez les champs utiles et paramétrez vos **notifications**.
3. (Optionnel) Si vous êtes à l'aise avec les clés SSH, ajoutez votre clé publique dans *Settings → SSH Keys* (elle configurera l'authentification SSH des exercices suivants).
4. Explorez librement l'interface et la documentation en ligne : <https://gricad-gitlab.univ-grenoble-alpes.fr/help>.

**Objectif validé** : se connecter et régler son profil.

### Exercice B.2 — Groupes et projets

1. Demandez à rejoindre le groupe `git_cnrs` (ou le groupe indiqué par le formateur) via le lien *Request Access*.
2. Créez votre projet `sandbox_votre_login` dans ce groupe/sous-groupe.
3. Renseignez :
   - un **emplacement** (namespace) et un **nom** ;
   - une **description** ;
   - un **niveau de visibilité** (privé / interne / public) et laissez-vous **initialiser un `README.md`** pour créer un dépôt non vide.
4. Observez que le projet hérite de la visibilité du groupe.

> **Bonnes pratiques** : ne négligez pas le nommage et l'organisation (renommer/déplacer un projet a un impact sur son URL). Gérez proprement la liste des membres et leurs **rôles** / **droits**.

**Objectif validé** : savoir créer un projet dans un groupe et comprendre l'organisation des espaces.

### Exercice B.3 — Manipuler le dépôt via l'interface web

GitLab permet de gérer un dépôt Git entièrement depuis le navigateur.

1. Depuis votre projet `sandbox_votre_login`, créez une **branche** (menu *Branches*).
2. Dans cette branche, créez un fichier `votrelogin.md` (`+` → *Nouveau fichier*), insérez un contenu quelconque et faites un **commit** via l'interface.
3. Retrouvez votre commit dans *Commits* et l'historique de la branche.

**Objectif validé** : réaliser des opérations Git de base (branche, fichier, commit) via le web.

### Exercice B.4 — Cloner un dépôt distant

Récupérez votre projet GitLab sur votre machine.

1. Depuis votre projet, repérez l'adresse de clonage. Deux protocoles :
   - **HTTPS** (`https://...`) : authentification par vos identifiants de plateforme ;
   - **SSH** (`git@...`) : authentification par clé SSH.
2. Clonez : `git clone <adresse> sandbox` puis entrez dans le dossier.
3. Listez les **dépôts distants** connectés : `git remote`. Le nom par défaut du dépôt distant est `origin`.
4. Regardez l'état et l'historique : `git status`, `git log --oneline --graph` (vous y voyez les branches locales *et* distantes).

> **À retenir** : après un clone, le **tracking** entre votre branche locale `main` et `origin/main` est déjà actif. `git pull` et `git push` fonctionnent directement.

**Objectif validé** : cloner un dépôt distant (HTTPS ou SSH) et comprendre la notion de `remote`.

### Exercice B.5 — Synchroniser avec le dépôt distant (pull / push)

Mettez en place un cycle complet d'échanges avec GitLab.

1. *Fichier ajouté côté distant* : via le web, créez un fichier `notes.md` dans `main` et committez-le.
2. Récupérez ces changements **en deux temps** (pour comprendre le mécanisme) :
   ```shell
   $ git fetch origin          # collecte les données distantes (ne modifie pas vos branches)
   $ git merge origin/main     # fusionne dans votre branche courante
   ```
   puis **en une seule commande** : `git pull` (fetch + merge).
3. *Fichier ajouté côté local* : créez maintenant un fichier `atelier.md` en local, committez-le, puis **poussez** :
   ```shell
   $ git add atelier.md
   $ git commit -m "Ajout du fichier atelier"
   $ git push                  # main déjà trackée : pas besoin de préciser
   ```
4. Vérifiez sur GitLab que le fichier est bien en ligne.

**Objectif validé** : synchroniser dépôt local et distant (*fetch / merge / pull / push*).

### Exercice B.6 — Branches distantes

1. Créez localement une branche `fonctionnalite` et faites-y un commit.
2. Poussez-la vers le dépôt distant **en lui associant une branche upstream** au premier push :
   ```shell
   $ git switch -c fonctionnalite
   $ git push --set-upstream origin fonctionnalite
   ```
   (ou `git push -u origin fonctionnalite`). Sans `-u`, Git refuse le push faute de branche upstream.
3. Listez toutes les branches (locales **et** distantes) : `git branch -a`.
4. **Rapatrier une branche distante** : passez sur `main`, puis `git switch <autre_branche_distante>` : Git crée la branche locale et la rattache directement à la branche distante (après un `fetch`).

**Objectif validé** : créer et pousser une branche locale, et rapatrier une branche distante.

### Exercice B.7 — Rédiger en Markdown (GitLab-flavored)

Dans votre projet, éditez le fichier `README.md` directement depuis l'interface web et démontrez les éléments suivants du **GitLab-flavored Markdown** :

- un titre et un sous-titre ;
- une liste à puces et une liste numérotée ;
- un **texte en gras** et un *texte en italique* (et du ~~texte barré~~) ;
- un [lien](https://docs.gitlab.com/ee/user/markdown.html) et une image ;
- un tableau ;
- une case à cocher (`- [ ] tâche à faire`) ;
- un extrait de **code** avec coloration syntaxique (triple accent + langage).

Faites un *commit* via l'interface et observez le rendu formatté.

> **À retenir** : le Markdown est très utilisé (issues, commentaires, README). Écrire un `README.md` de qualité est **très vivement conseillé** pour chaque projet.

**Objectif validé** : formater un document Markdown et le versionner.

---

## Niveau 2 — Travailler à plusieurs (en binôme)

> Avant de commencer : ouvrez une **session partagée avec un binôme** sur un projet commun (le formateur vous ajoutera comme *Developer* sur le projet `common`).

### Exercice B.8 — Pull/push synchronisés sur un même dépôt

1. Chacun clone le projet `common` : `git clone <adresse> common`.
2. **Les deux participants éditent et valident alors le même fichier** (ex. `README.md`).
3. Le premier fait `git push` avec succès ; le second doit d'abord faire `git pull` (ou `git fetch` + `git merge`) avant de pouvoir pousser.
4. En cas de **conflit**, résolvez-le à deux (voir exercice A.9, mais cette fois sur un dépôt partagé).
5. Vérifiez que tout le monde est à jour et que le dépôt distant reflète l'état final.

**Objectif validé** : synchroniser plusieurs contributeurs sur le même dépôt sans perte de travail.

### Exercice B.9 — Issues, labels et jalons (Board / Milestones)

1. Sur GitLab, ouvrez quelques **numéros d'issue** (*Issues*) dans le projet `common` (ex. « Améliorer la description du README »).
2. Rédigez-les **en Markdown** et ajoutez-leur des **labels explicites** (documentation, bug, idée...).
3. Classez-les avec le **Board** GitLab (colonnes *À faire / En cours / Fait*).
4. Rattachez-les à un **jalon** (*Milestones*) pour planifier une étape du projet.
5. **Clore automatiquement une issue via un commit** : créez une branche, modifiez le fichier concerné, et committez avec un message contenant `Fix #<numéro>`.
6. **Bonus** : dans une issue ou un commit, mentionnez un collègue avec `@username` (cela lui envoie une notification et l'ajoute à sa todo-list), et faites référence à une issue avec `#<id>`.

**Objectif validé** : utiliser les *issues*, les labels, le *Board* et les jalons pour organiser le travail.

### Exercice B.10 — Merge-request et revue de code

1. Chacun crée sa branche `votre_login` dans le projet `common` et y fait des modifications.
2. Ouvrez une **merge-request (MR)** de votre branche vers `main` : décrivez-la en Markdown, et si besoin liez-la à une issue (`Closes #id`).
3. Échangez vos rôles : chaque participant **reviewe** la MR d'un autre, laisse un commentaire (mention `@username`), puis l'**approuve** ou la **refuse**.
4. Corrigez la MR si nécessaire, puis **fusionnez**-la une fois approuvée.

> **Pour aller plus loin** : si `main` est *protégée* (*Paramètres → Référentiel → Branches protégées*, fusion uniquement via MR), cet exercice devient **obligatoire** : on ne pousse jamais directement dans `main`.

**Objectif validé** : collaborer proprement via les branches et les merge-requests.

---

## Niveau 3 — En groupe / contribution externe

### Exercice B.11 — Fork + merge-request

Participez à un projet dont vous **n'êtes pas membre** :

1. Sur GitLab, faites un **fork** d'un projet commun vers votre espace personnel (**attention au nommage du fork**).
2. Clonez votre fork, faites vos modifications (branche, commits), puis poussez-les vers votre fork.
3. Ouvrez une **merge-request** de votre fork vers le projet **source**.
4. Un formateur ou un autre participant **reviewe** votre MR, puis l'intègre.

**Objectif validé** : contribuer à un projet tiers via le fork.

---

## Niveau 4 — La CI/CD

### Exercice B.12 — Un premier pipeline

1. Dans votre projet (ou un dépôt `awesome-website` fourni), créez à la racine un fichier `.gitlab-ci.yml`.
2. Ajoutez un job qui affiche un message :
   ```yaml
   hello:
     script:
       - echo "Hello $GITLAB_USER_LOGIN !"
   ```
3. Poussez ce fichier : un **pipeline** doit se déclencher automatiquement (menu *CI/CD → Pipelines*).
4. Cliquez sur le job et lisez le contenu des **logs** : vous y verrez votre message, ainsi que les commandes exécutées.

**Objectif validé** : comprendre le déclenchement d'une CI sur *push*.

### Exercice B.13 — Publier un site avec GitLab Pages

1. Clonez le dépôt du site (construit avec Hugo).
2. Créez une branche à votre nom et modifiez le contenu (ex. `content/fr/home`).
3. Ouvrez une **merge-request** vers `main` ; la CI construit le site automatiquement.
4. Après fusion, consultez la page publiée (*Déploiements → Pages*).

**Objectif validé** : automatiser la construction et la publication d'un site web avec la CI/CD.

---

## Pour aller plus loin (GitLab)

- Le **Wiki** du projet : documenter votre projet directement sur la forge.
- Les **snippets** : partager de petits extraits de code.
- L'**intégration continue** avancée : caches, artefacts, tests parallèles, déploiement par environnements.

---

# Conditions de réussite

À la fin de la formation, vous êtes **autonomes** si vous savez :

**Git (local)**
- [ ] créer un dépôt avec `git init` et faire un cycle *modifier → `git add` → `git commit`* ;
- [ ] comprendre la zone de staging et consulter l'état avec `git status` ;
- [ ] lire et naviguer dans l'historique (`git log`) et annuler proprement (`git revert`) ;
- [ ] créer des branches, les fusionner (`git merge`) et résoudre un conflit ;
- [ ] exclure des fichiers du suivi (`.gitignore`, `git rm --cached`).

**GitLab (distants et collaboration)**
- [ ] créer des groupes et projets et en maîtriser la visibilité ;
- [ ] cloner, et synchroniser un dépôt local avec GitLab (*fetch / merge / pull / push*) ;
- [ ] créer et pousser une branche distante (`git push -u origin`) ;
- [ ] rédiger et versionner un document Markdown ;
- [ ] utiliser les *issues*, les labels, le *Board* et les jalons ;
- [ ] contribuer à un projet via une **merge-request** (avec ou sans fork) et effectuer une revue ;
- [ ] décrire et déclencher une tâche de **CI/CD** et publier avec **GitLab Pages**.
