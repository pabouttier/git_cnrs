+++
template = "page.html"
title = "Exercices — le carnet de bord"
insert_anchor_links = "right"
+++

# Exercices : un carnet de bord, du local à la forge {#exercices}

Cette page n'est **pas un résumé du cours**. Chaque fiche est une **mission** à réaliser dans un dépôt qui grandit au fil de la formation.

**Le fil rouge.** Vous tenez le carnet de bord d'un projet : un `README.md`, des notes datées, des annexes. En partie A, vous le versionnez en local. En partie B, vous le publiez sur GitLab, vous le faites vivre à plusieurs, puis vous le mettez en ligne avec GitLab Pages.

**Comment travailler**

1. Lisez la mission et essayez **sans ouvrir les aides**.
2. Vérifiez la **preuve de réussite** : c'est un résultat observable, pas une impression.
3. Si vous bloquez, ouvrez les **indices** un par un, puis la **solution**.
4. Si quelque chose casse, consultez le [dépannage](#depannage) avant d'appeler le formateur.

**Légende** · 🛠️ mission · ✅ preuve de réussite · 💡 indices · 🔧 solution · 🚀 défi

| Git en local | GitLab |
|---|---|
| [A.1 — Identité](#a1) | [B.1 — Connexion et profil](#b1) |
| [A.2 — Premier dépôt](#a2) | [B.2 — Projet d'accueil](#b2) |
| [A.3 — Trois états, trois commits](#a3) | [B.3 — Commit depuis le web](#b3) |
| [A.4 — Committer juste](#a4) | [B.4 — Publier et cloner](#b4) |
| [A.5 — Historique lisible](#a5) | [B.5 — Synchroniser](#b5) |
| [A.6 — Voyager et réparer](#a6) | [B.6 — Branches distantes](#b6) |
| [A.7 — Nettoyer](#a7) | [B.7 — README utile](#b7) |
| [A.8 — Branches](#a8) | [B.8 — À deux sur un dépôt](#b8) |
| [A.9 — Conflit](#a9) | [B.9 — Backlog](#b9) |
| [Défis Git](#pour-aller-plus-loin) | [B.10 — Merge-request](#b10) |
| [Dépannage](#depannage) | [B.11 — Fork](#b11) |
| [Auto-évaluation](#autoevaluation) | [B.12 — Pipeline](#b12) |
| | [B.13 — Pages](#b13) |

> **Prérequis** : un compte sur la plateforme et les outils installés (voir la page [Prérequis](/git_cnrs/prerequis/)).

---

# Partie A — Git en local {#partie-a}

Vous travaillez dans `~/carnet-de-bord`, **sans dépôt distant**. L'objectif n'est pas d'apprendre des commandes par cœur, mais de produire un historique que vous saurez relire, réparer et fusionner.

À la fin de cette partie, votre dépôt ressemblera à ceci (les dates et les contenus seront les vôtres) :

```text
carnet-de-bord/
├── .gitignore
├── README.md
├── annexes/
│   ├── lectures.md
│   └── ressources.md
├── brouillons/          # non suivi, conservé sur le disque
│   └── idees.md
└── notes/
    ├── 2026-06-01.md
    ├── 2026-06-03.md
    ├── 2026-06-04.md
    └── 2026-06-05.md
```

---

## Étape 1 — Démarrer le carnet (A.1 à A.3) {#etape-1}

### A.1 — Prendre possession de son identité {#a1}

> **Mission** — Vos commits doivent être signés avec la même identité que votre compte GitLab. Configurez votre poste une fois pour toutes, puis vérifiez que Git l'a bien enregistré.
>
> **Durée** 10 min · **Mode** seul · **Prérequis** aucun

**À faire**

1. Dans GitLab, ouvrez votre avatar → *Edit profile* → *Main settings*. Notez le **nom** et l'**e-mail** affichés.
2. Dans un terminal, configurez Git avec ces valeurs, votre éditeur et le nom de branche par défaut :
   - `git config --global user.name "Prénom Nom"`
   - `git config --global user.email "l'e-mail de votre compte"`
   - `git config --global core.editor "code --wait"` (ou `nano`, `vim`, `gedit`...)
   - `git config --global init.defaultBranch main`
3. Ouvrez le fichier de configuration avec `git config --global --edit` et repérez les quatre réglages.
4. Vérifiez chaque valeur avec `git config --global --get <clé>`.

**✅ Preuve de réussite**

```shell
$ git config --global user.name
$ git config --global user.email
$ git config --global init.defaultBranch
```

Les deux premières valeurs correspondent à votre profil GitLab ; la troisième affiche `main`.

<details>
<summary>💡 Indices</summary>

- La clé de l'éditeur est `core.editor`.
- `git config --global --list` affiche tous les réglages d'un coup.
- Avec VS Code, `code --wait` évite que Git rende la main avant la fermeture de l'éditeur.

</details>

<details>
<summary>🔧 Solution</summary>

```shell
$ git config --global user.name "Camille Martin"
$ git config --global user.email "camille.martin@univ-grenoble-alpes.fr"
$ git config --global core.editor "code --wait"
$ git config --global init.defaultBranch main
$ git config --global --list
user.name=Camille Martin
user.email=camille.martin@univ-grenoble-alpes.fr
core.editor=code --wait
init.defaultbranch=main
```

`git config --global --edit` ouvre `~/.gitconfig` : c'est ce fichier que Git relit pour tous vos dépôts.

</details>

**🚀 Défi** — Dans un dépôt de test, surchargez l'e-mail avec `git config user.email "autre@exemple.fr"` (sans `--global`), puis comparez `git config user.email` et `git config --global user.email`. Supprimez le réglage local avec `git config --unset user.email`.

---

### A.2 — Donner un toit au carnet {#a2}

> **Mission** — Créez le dépôt `carnet-de-bord` dans votre dossier personnel, écrivez un README qui dit ce que c'est, et enregistrez ce premier état.
>
> **Durée** 15 min · **Mode** seul · **Prérequis** A.1

**À faire**

1. Créez le dossier et initialisez le dépôt :

   ```shell
   $ cd
   $ mkdir carnet-de-bord
   $ cd carnet-de-bord
   $ git init
   ```

2. Créez `README.md` avec :
   - un titre de niveau 1 (le nom du carnet) ;
   - une phrase qui dit à quoi il sert ;
   - une ligne `Démarré le <date>`.
3. Avant d'indexer, lancez `git status`. Que répond Git ? Pourquoi ?
4. Indexez le fichier, regardez à nouveau `git status`, puis committez avec un message qui décrit l'état initial.
5. Vérifiez le contenu du commit avec `git show --stat HEAD`, puis l'historique avec `git log --oneline`.

**✅ Preuve de réussite**

```shell
$ git status
On branch main
nothing to commit, working tree clean
$ git log --oneline
<sha> Initialise le carnet de bord
$ git show --stat HEAD
 README.md | ...
```

La branche courante est `main` (grâce à A.1) et le commit ne contient que `README.md`.

<details>
<summary>💡 Indices</summary>

- `git init` crée le dossier caché `.git` ; vérifiez avec `ls -a`.
- Le cycle est : `git status` → `git add README.md` → `git status` → `git commit -m "..."`.
- Un message de commit réussi se lit sans ouvrir le diff.

</details>

<details>
<summary>🔧 Solution</summary>

```shell
$ cd
$ mkdir carnet-de-bord
$ cd carnet-de-bord
$ git init
$ git status
$ git add README.md
$ git status
$ git commit -m "Initialise le carnet de bord"
$ git show --stat HEAD
$ git log --oneline
```

Exemple de `README.md` :

```markdown
# Carnet de bord

Notes de suivi de mon projet : décisions, manipulations, prochaines étapes.

Démarré le 1er juin 2026.
```

</details>

**🚀 Défi** — Créez un dossier `test-init/` contenant un fichier, lancez-y `git init`, puis `git status`. Git accepte donc un dossier non vide. Supprimez ensuite le dépôt de test avec `rm -rf test-init/.git` : le dossier et son fichier restent intacts.

---

### A.3 — Trois états, trois commits {#a3}

> **Mission** — Faites vivre le carnet : ajoutez deux notes datées et une précision dans le README, en produisant des commits séparés et lisibles.
>
> **Durée** 20 min · **Mode** seul · **Prérequis** A.2

**À faire**

1. Créez `notes/2026-06-01.md` avec trois puces : ce que vous avez fait, ce qui vous a bloqué, la prochaine étape.
2. `git status` : le dossier `notes/` est non suivi. Indexez la note, vérifiez `git status`, committez.
3. Modifiez le README pour ajouter une phrase sur l'objectif du carnet. Indexez et committez **séparément**.
4. Créez `notes/2026-06-02.md` sur le même modèle, puis committez-la.
5. Affichez l'historique avec `git log --oneline --stat` : chaque commit ne doit contenir que les fichiers annoncés par son message.

**✅ Preuve de réussite**

- `git log --oneline` montre quatre commits (le commit initial + trois).
- `git log --oneline --stat` : chaque commit ne touche que le fichier attendu.
- `git status` affiche `nothing to commit, working tree clean`.

<details>
<summary>💡 Indices</summary>

- Pour indexer un seul fichier : `git add notes/2026-06-01.md`.
- `git log --oneline --stat` affiche le message, le hash et les fichiers modifiés.
- Un commit peut ne contenir qu'un seul fichier : c'est normal et souvent souhaitable.

</details>

<details>
<summary>🔧 Solution</summary>

```shell
$ git add notes/2026-06-01.md
$ git commit -m "Ajoute la note du 1er juin"
$ git add README.md
$ git commit -m "Précise l'objectif du carnet"
$ git add notes/2026-06-02.md
$ git commit -m "Ajoute la note du 2 juin"
$ git log --oneline --stat
```

</details>

**🚀 Défi** — Utilisez `git commit -am "..."` pour modifier le README (fichier déjà suivi), puis créez une nouvelle note et essayez la même commande. Que se passe-t-il ? Pourquoi la note n'est-elle pas incluse ?

---

## Étape 2 — Tenir un historique propre (A.4 à A.7) {#etape-2}

### A.4 — Committer juste ce qu'il faut {#a4}

> **Mission** — Trois changements sont en cours ; deux seulement appartiennent au prochain commit. Préparez le bon instantané, puis corrigez deux erreurs d'indexation.
>
> **Durée** 20 min · **Mode** seul · **Prérequis** A.3

**À faire**

1. Modifiez le README (ajoutez une section « Journal »), créez `notes/2026-06-03.md`, et créez un fichier temporaire `brouillon.tmp` à la racine.
2. Indexez **uniquement** le README et la note. `git status` doit montrer `brouillon.tmp` comme non suivi.
3. Committez avec un message qui dit pourquoi ces deux changements vont ensemble.
4. Indexez tout par erreur avec `git add .`, puis retirez `brouillon.tmp` de l'index avec `git restore --staged brouillon.tmp`. Vérifiez qu'il est redevenu non suivi.
5. Dans le README, ajoutez une ligne `À SUPPRIMER`, puis abandonnez cette modification avec `git restore README.md`. Vérifiez que la ligne a disparu.

**✅ Preuve de réussite**

```shell
$ git show --stat HEAD
 README.md
 notes/2026-06-03.md
$ git status --short
?? brouillon.tmp
$ grep "À SUPPRIMER" README.md
# aucune sortie
```

Le commit contient exactement deux fichiers, le fichier temporaire est non suivi, et la mauvaise ligne a disparu.

<details>
<summary>💡 Indices</summary>

- `git add <fichier>` indexe un fichier précis ; `git add .` indexe tout le dossier courant.
- `git restore --staged <fichier>` retire un fichier de l'index **sans** toucher au disque.
- `git restore <fichier>` restaure le contenu du dernier commit et **efface** les modifications non indexées.

</details>

<details>
<summary>🔧 Solution</summary>

```shell
$ git add README.md notes/2026-06-03.md
$ git status
$ git commit -m "Documente la séance du 3 juin"
$ git add .
$ git restore --staged brouillon.tmp
$ git status --short
$ git restore README.md
$ git status
```

</details>

**🚀 Défi** — Testez `git restore brouillon.tmp` après l'avoir indexé : que se passe-t-il si le fichier n'existe pas dans le dernier commit ? Comparez avec `git restore --staged`. Retenez la différence entre « annuler une indexation » et « annuler une modification ».

---

### A.5 — Un historique qui se relit {#a5}

> **Mission** — Transformez un chantier en trois commits atomiques, puis réparez le dernier commit qui a oublié un fichier et dont le message est vague.
>
> **Durée** 25 min · **Mode** seul · **Prérequis** A.4

**À faire**

1. Préparez trois changements indépendants :
   - corrigez une faute dans `notes/2026-06-01.md` ;
   - créez `annexes/ressources.md` avec trois liens utiles ;
   - ajoutez une section « Organisation » au README.
2. Committez-les **un par un**, dans l'ordre de votre choix, avec des messages qui expliquent le *pourquoi* (pas seulement le nom du fichier).
3. Ajoutez une ligne à la section « Organisation » du README, indexez-la, puis intégrez-la au dernier commit avec `git commit --amend` (sans changer le message).
4. Trouvez le message du dernier commit trop vague : réécrivez-le avec `git commit --amend -m "..."`.
5. Vérifiez avec `git log --oneline` et `git show --stat HEAD`.

**✅ Preuve de réussite**

- Trois commits (hors commit initial) avec des messages distincts et lisibles.
- `git show --stat HEAD` contient la ligne ajoutée à l'étape 3.
- `git status` est propre.
- Le dernier message décrit l'intention, pas la liste des fichiers.

<details>
<summary>💡 Indices</summary>

- `git add` puis `git commit -m "..."` pour chaque sujet.
- `git commit --amend` remplace le **dernier** commit ; il ne crée pas un nouveau commit dans l'historique.
- `git commit --amend -m "..."` remplace le dernier commit **et** son message.

</details>

<details>
<summary>🔧 Solution</summary>

```shell
$ git add notes/2026-06-01.md
$ git commit -m "Corrige une faute dans la note du 1er juin"
$ git add annexes/ressources.md
$ git commit -m "Ajoute une première liste de ressources"
$ git add README.md
$ git commit -m "Explique l'organisation du carnet"
$ git add README.md
$ git commit --amend --no-edit
$ git commit --amend -m "Explique l'organisation et le point d'entrée du carnet"
$ git log --oneline
```

</details>

**🚀 Défi** — Que se passe-t-il si vous faites `git commit --amend` sur un commit déjà poussé ? Pourquoi est-ce particulièrement risqué sur une branche `main` partagée ? Ne poussez pas ce commit : raisonnez sur le graphe.

---

### A.6 — Voyager dans l'historique et réparer {#a6}

> **Mission** — Une note contient une erreur déjà commitée. Retrouvez le commit fautif, annulez-le proprement, puis visitez un état ancien et repartez de là avec une branche.
>
> **Durée** 25 min · **Mode** seul · **Prérequis** A.5

**À faire**

1. Créez `notes/2026-06-04.md` avec la ligne `TODO: vérifier les chiffres`, puis committez.
2. Retrouvez le commit avec `git log --oneline`, inspectez-le avec `git show <sha>`, puis regardez l'historique du fichier avec `git log -p -- notes/2026-06-04.md`.
3. Annulez ce commit avec `git revert <sha>`. Observez : un nouveau commit est créé et le fichier disparaît. Pourquoi l'historique n'est-il pas réécrit ?
4. Recréez une version corrigée de `notes/2026-06-04.md` (sans `TODO`) et committez-la.
5. Placez-vous sur un ancien commit avec `git checkout <sha>` : Git affiche `HEAD detached`. Créez une branche `sauvetage` avec `git switch -c sauvetage`, ajoutez `notes/2026-06-05.md`, committez.
6. Revenez sur `main` (`git switch main`), fusionnez `sauvetage`, puis vérifiez le graphe avec `git log --oneline --graph --all`.

**✅ Preuve de réussite**

```shell
$ grep -R "TODO" notes/
# aucune sortie
$ git status
On branch main
nothing to commit, working tree clean
$ git log --oneline --graph
*   <sha> Merge branch 'sauvetage'
...
```

La note du 4 juin est corrigée, la note du 5 juin existe, le commit de revert est visible, et vous n'êtes plus en `HEAD` détaché.

<details>
<summary>💡 Indices</summary>

- `git revert <sha>` crée un commit qui **inverse** les changements du commit ciblé.
- `git checkout <sha>` déplace `HEAD` sur un commit ; les fichiers reviennent à cet état.
- `git switch -c <branche>` crée une branche depuis l'endroit où vous êtes, ce qui « sauve » la visite.
- `git switch main` vous ramène sur la branche principale.

</details>

<details>
<summary>🔧 Solution</summary>

```shell
$ git add notes/2026-06-04.md
$ git commit -m "Ajoute la note du 4 juin"
$ git log --oneline
$ git show <sha>
$ git revert <sha>
$ git add notes/2026-06-04.md
$ git commit -m "Corrige la note du 4 juin"
$ git checkout <sha-ancien>
$ git switch -c sauvetage
$ git add notes/2026-06-05.md
$ git commit -m "Ajoute la note du 5 juin"
$ git switch main
$ git merge sauvetage
$ git log --oneline --graph --all
```

</details>

**🚀 Défi** — `git revert` d'un commit de fusion nécessite l'option `-m 1`. À quoi sert le `1` ? Testez sur une copie du dépôt, jamais sur le carnet principal.

---

### A.7 — Nettoyer sans perdre {#a7}

> **Mission** — Le carnet contient une note obsolète et un dossier de brouillons qui n'a rien à faire dans l'historique. Supprimez ce qui doit l'être, arrêtez de suivre le reste, et empêchez les fichiers temporaires de revenir.
>
> **Durée** 20 min · **Mode** seul · **Prérequis** A.5

**À faire**

1. Supprimez `notes/2026-06-02.md` **du dépôt et du disque** avec `git rm`, puis committez.
2. Créez `brouillons/idees.md`, indexez-le et committez-le (simulez un ajout accidentel).
3. Retirez-le du suivi **sans l'effacer du disque** : `git rm --cached -r brouillons`.
4. Créez un `.gitignore` à la racine contenant au moins `brouillons/` et `*.tmp`. Committez le `.gitignore` et le retrait du dossier.
5. Vérifiez que `brouillons/idees.md` et `brouillon.tmp` existent toujours sur le disque, mais n'apparaissent plus dans `git status`.

**✅ Preuve de réussite**

```shell
$ git ls-files | grep -E "brouillons|\.tmp"
# aucune sortie
$ ls brouillons/
idees.md
$ git status --short
# aucune sortie (ou seulement les fichiers voulus)
$ git check-ignore -v brouillons/idees.md
.gitignore:1:brouillons/    brouillons/idees.md
```

<details>
<summary>💡 Indices</summary>

- `git rm <fichier>` supprime le fichier du disque **et** de l'index.
- `git rm --cached <fichier>` retire seulement le fichier de l'index ; le fichier reste sur le disque.
- `.gitignore` ne s'applique qu'aux fichiers **non suivis**.
- `git check-ignore -v` indique quelle règle ignore un fichier.

</details>

<details>
<summary>🔧 Solution</summary>

```shell
$ git rm notes/2026-06-02.md
$ git commit -m "Retire la note obsolète du 2 juin"
$ git add brouillons/idees.md
$ git commit -m "Ajoute un brouillon (à retirer)"
$ git rm --cached -r brouillons
$ printf 'brouillons/\n*.tmp\n' > .gitignore
$ git add .gitignore
$ git commit -m "Ignore les brouillons et les fichiers temporaires"
$ git status
$ git check-ignore -v brouillons/idees.md
```

</details>

**🚀 Défi** — Prouvez que `.gitignore` ne s'applique pas à un fichier déjà suivi : ajoutez `notes/2026-06-01.md` au `.gitignore`, puis regardez `git status`. Retirez la ligne ensuite. Que faudrait-il faire pour arrêter de suivre ce fichier malgré tout ?

---

## Étape 3 — Branches et conflits (A.8 et A.9) {#etape-3}

### A.8 — Branches : préparer sans bloquer {#a8}

> **Mission** — Préparez une annexe sur une branche pendant que `main` reste stable, fusionnez-la, puis provoquez une fusion divergente pour voir la différence.
>
> **Durée** 25 min · **Mode** seul · **Prérequis** A.5

**À faire**

1. Créez et basculez sur une branche `annexe` : `git switch -c annexe`.
2. Créez `annexes/lectures.md` avec trois références, committez.
3. Ajoutez dans le README un lien vers cette annexe, committez.
4. Revenez sur `main` : `git switch main`. Vérifiez que `annexes/lectures.md` n'existe pas. Fusionnez `annexe` : `git merge annexe`. Observez le message *fast-forward*.
5. Créez `corrections` depuis `main`, modifiez une ligne du README, committez. Revenez sur `main`, modifiez une **autre** ligne, committez. Fusionnez `corrections` : cette fois un commit de fusion est créé.
6. Affichez `git log --graph --oneline --all`, puis supprimez `annexe` avec `git branch -d annexe`.

**✅ Preuve de réussite**

- Le graphe montre d'abord une ligne droite (fast-forward), puis une bifurcation et un commit de fusion.
- Les deux modifications du README sont présentes.
- `git branch` ne liste plus `annexe`.

<details>
<summary>💡 Indices</summary>

- `git switch -c annexe` crée la branche **et** vous place dessus.
- On fusionne toujours une branche **source** dans la branche **cible** où l'on se trouve.
- `git log --graph --oneline --all` dessine l'historique de toutes les branches.
- `git branch -d` refuse de supprimer une branche dont les commits ne sont pas fusionnés ; c'est une sécurité.

</details>

<details>
<summary>🔧 Solution</summary>

```shell
$ git switch -c annexe
$ git add annexes/lectures.md
$ git commit -m "Ajoute une annexe de lectures"
$ git add README.md
$ git commit -m "Renvoie vers l'annexe de lectures"
$ git switch main
$ ls annexes/lectures.md   # absent
$ git merge annexe
$ git log --oneline --graph
$ git switch -c corrections
$ git add README.md
$ git commit -m "Précise la date de démarrage"
$ git switch main
$ git add README.md
$ git commit -m "Précise le public visé"
$ git merge corrections
$ git log --oneline --graph
$ git branch -d annexe
```

</details>

**🚀 Défi** — Refaites la fusion de `annexe` avec `git merge --no-ff annexe` sur une copie : quel commit supplémentaire apparaît ? Dans quel cas peut-on préférer garder une trace explicite de la fusion ?

---

### A.9 — Conflit : le provoquer, le lire, le résoudre {#a9}

> **Mission** — Deux branches modifient le même titre du README. Produisez le conflit, lisez-le, choisissez un texte final, puis apprenez à annuler une fusion conflictuelle.
>
> **Durée** 30 min · **Mode** seul · **Prérequis** A.8

**À faire**

1. Depuis `main`, créez deux branches `titre-court` et `titre-long`. Sur chacune, remplacez le titre H1 du README par une formulation différente, et committez.
2. Placez-vous sur `titre-court`, puis fusionnez `titre-long` : `git merge titre-long`. Git signale un conflit.
3. Lisez `git status`, ouvrez le README et repérez les marqueurs `<<<<<<<`, `=======`, `>>>>>>>`.
4. Écrivez un titre final qui n'est **ni exactement** celui de `titre-court`, ni celui de `titre-long`. Supprimez tous les marqueurs.
5. Indexez le fichier (`git add README.md`) et terminez la fusion (`git commit`). Vérifiez le graphe.
6. Revenez sur `main`, ajoutez une ligne à `notes/2026-06-01.md` (fichier différent), committez : `main` a divergé sans toucher au titre. Fusionnez `titre-court` : un commit de fusion est créé et le titre résolu arrive.
7. **Casser puis annuler** : depuis `main`, créez `titre-annule`, modifiez le H1, committez. Depuis `main`, créez `titre-conflit`, modifiez le H1 autrement, committez. Sur `titre-conflit`, `git merge titre-annule` → conflit, puis `git merge --abort`. Vérifiez que `git status` est propre et que le README ne contient plus de marqueurs. Revenez sur `main` et supprimez les branches de travail.

**✅ Preuve de réussite**

```shell
$ grep -R "<<<<<<<\|=======\|>>>>>>>" .
# aucune sortie
$ git status
On branch main
nothing to commit, working tree clean
$ git log --oneline --graph
*   <sha> Merge branch 'titre-court'
...
```

Après `--abort`, la fusion interrompue a disparu et `main` n'a pas bougé.

<details>
<summary>💡 Indices</summary>

- Un conflit n'est pas une erreur : Git vous demande de choisir le contenu final.
- Les marqueurs délimitent la version de la branche courante (`HEAD`), la version de l'autre branche, et éventuellement un ancêtre commun.
- Après édition, il faut `git add` pour dire que le conflit est résolu, puis `git commit`.
- `git merge --abort` annule une fusion en cours et restaure l'état d'avant.

</details>

<details>
<summary>🔧 Solution</summary>

```shell
$ git switch -c titre-court
$ # remplacer le H1 par : # Carnet de bord — version courte
$ git commit -am "Raccourcit le titre"
$ git switch main
$ git switch -c titre-long
$ # remplacer le H1 par : # Carnet de bord de suivi de projet
$ git commit -am "Décrit mieux le contenu"
$ git switch titre-court
$ git merge titre-long
# ... conflit ...
$ git status
$ # éditer README.md et choisir :
# # Carnet de bord — suivi de projet
$ git add README.md
$ git commit
$ git log --oneline --graph
$ git switch main
$ # ajouter une ligne à notes/2026-06-01.md
$ git commit -am "Complète la note du 1er juin"
$ git merge titre-court
$ git log --oneline --graph
$ git switch -c titre-annule
$ # modifier le H1
$ git commit -am "Essai de titre"
$ git switch main
$ git switch -c titre-conflit
$ # modifier le H1 autrement
$ git commit -am "Autre essai de titre"
$ git merge titre-annule
$ git merge --abort
$ git status
$ git switch main
$ git branch -D titre-conflit titre-annule
$ git branch -d titre-court titre-long
```

</details>

**🚀 Défi** — Pendant un conflit, `git checkout --ours README.md` garde la version de la branche courante et `--theirs` celle de l'autre branche. Dans la fusion `titre-long` → `titre-court`, qui est « ours » ? Testez sur une copie, puis expliquez pourquoi ce n'est pas toujours la branche que l'on croit.

---

## Le carnet est prêt {#bilan-a}

Avant de passer à GitLab, vérifiez que vous savez refaire **sans aide** :

- créer un dépôt et produire trois commits atomiques ;
- annuler une modification indexée, puis une modification non indexée ;
- retrouver un commit fautif et l'annuler avec `git revert` ;
- fusionner une branche et résoudre un conflit ;
- expliquer la différence entre `git rm` et `git rm --cached`.

Si un point résiste, refaites l'exercice correspondant : la partie B s'appuie sur ce dépôt.

---

# Partie B — Le carnet sur GitLab {#partie-b}

Cette partie se déroule sur [GRICAD-GitLab](https://gricad-gitlab.univ-grenoble-alpes.fr). Le formateur vous communique le **groupe de formation** et le projet `common` utilisé pour le travail à plusieurs.

Vous allez publier le dépôt `carnet-de-bord` créé en partie A, le faire évoluer avec d'autres personnes, puis le mettre en ligne. **Conservez le même dépôt** : chaque exercice ajoute une brique au précédent.

> **Attention** — Les exercices B.8 à B.11 se font à deux ou en groupe. Les autres se font seul, mais vous pouvez comparer vos résultats avec un voisin.

---

## Étape 4 — Publier le carnet (B.1 à B.6) {#etape-4}

### B.1 — Entrer dans la forge {#b1}

> **Mission** — Connectez-vous, complétez votre profil et réglez vos notifications. Si vous avez une clé SSH, ajoutez-la maintenant.
>
> **Durée** 15 min · **Mode** seul · **Prérequis** un compte sur la plateforme

**À faire**

1. Connectez-vous à <https://gricad-gitlab.univ-grenoble-alpes.fr> :
   - **communauté ESR Grenoble** : onglet *LDAP UGA* ;
   - **comptes externes** : onglet *Standard*.
2. Ouvrez votre avatar → *Edit profile* et renseignez au minimum votre nom complet, votre organisation et votre poste. Enregistrez.
3. Dans *Settings → Notifications*, choisissez un niveau de notification pour le groupe de formation. Expliquez en une phrase ce que vous attendez comme notifications.
4. **Optionnel — clé SSH** :
   ```shell
   $ ssh-keygen -t ed25519 -C "votre e-mail"
   $ cat ~/.ssh/id_ed25519.pub
   ```
   Copiez la clé publique dans *Settings → SSH Keys*, puis testez :
   ```shell
   $ ssh -T git@gricad-gitlab.univ-grenoble-alpes.fr
   ```
5. Notez votre **nom d'utilisateur** (le *username*, pas le nom affiché) : il servira dans les URLs et les mentions.

**✅ Preuve de réussite**

- Votre page de profil est accessible à l'adresse `.../<votre_username>`.
- Le menu de l'avatar affiche votre nom complet.
- Si vous avez configuré SSH, `ssh -T` renvoie un message de bienvenue avec votre username.
- Vous savez dire quel niveau de notification vous avez choisi et pourquoi.

<details>
<summary>💡 Indices</summary>

- Le username est visible dans l'URL de votre profil et dans le menu de l'avatar.
- Une clé SSH se compose d'une clé **publique** (`.pub`, à copier) et d'une clé **privée** (à ne jamais partager).
- Si `ssh -T` affiche `Permission denied (publickey)`, la clé n'est pas reconnue : vérifiez que vous avez copié la bonne clé et que vous utilisez le bon compte.

</details>

<details>
<summary>🔧 Solution</summary>

1. Connexion par l'onglet correspondant à votre compte.
2. Profil complété et enregistré.
3. Notifications : pour un projet suivi, « Watch » envoie toutes les activités ; « Participate » se limite aux discussions où vous êtes impliqué. Pour la formation, « Participate » suffit généralement.
4. Clé SSH ajoutée ; `ssh -T` répond `Welcome to GitLab, @votre_username!`.
5. Username noté.

</details>

**🚀 Défi** — Fixez un statut (« En formation Git », par exemple), puis cherchez dans le menu *Help* la page consacrée aux clés SSH. Ajoutez-la à vos favoris.

---

### B.2 — Créer le projet d'accueil {#b2}

> **Mission** — Créez un projet **vide** `carnet-de-bord` dans le groupe de formation, en visibilité privée. Il accueillera le dépôt local de la partie A.
>
> **Durée** 15 min · **Mode** seul · **Prérequis** B.1

**À faire**

1. Demandez à rejoindre le groupe de formation via le lien *Request Access* (ou faites-vous ajouter par le formateur).
2. Créez un projet : *New project → Create blank project*.
3. Renseignez :
   - **nom** : `carnet-de-bord` ;
   - **namespace** : le groupe de formation (ou votre espace sandbox dans ce groupe) ;
   - **visibilité** : privée ;
   - **décochez** « Initialize repository with a README ».
4. Créez le projet, puis notez les deux URLs de clonage (HTTPS et SSH).
5. Vérifiez dans *Settings → General* la visibilité, et dans *Members* votre rôle.

**✅ Preuve de réussite**

- La page du projet affiche « The repository for this project is empty » et les commandes de clonage.
- L'URL du projet se termine par `/carnet-de-bord`.
- Le dépôt est **vide** : aucun commit, aucun fichier.

<details>
<summary>💡 Indices</summary>

- Le namespace se choisit dans un menu déroulant, au moment de la création.
- « Initialize repository with a README » crée un premier commit : ici, cela nous gênerait pour pousser l'historique local.
- L'URL SSH commence par `git@` ; l'URL HTTPS par `https://`.

</details>

<details>
<summary>🔧 Solution</summary>

1. Groupe rejoint.
2. *New project → Create blank project*.
3. Nom `carnet-de-bord`, namespace de formation, visibilité privée, README décoché.
4. URLs notées.
5. Le projet est vide et vous êtes au moins *Developer* (nécessaire pour pousser).

</details>

**🚀 Défi** — Dans *Settings → General → Advanced*, regardez l'URL du projet, renommez-le temporairement, observez la nouvelle URL, puis remettez le nom d'origine. Que faudrait-il mettre à jour dans un dépôt local si l'URL change ?

---

### B.3 — Un commit depuis le navigateur {#b3}

> **Mission** — Créez la branche `notes-web` et ajoutez `notes/2026-06-06.md` depuis l'interface web, sans toucher à votre dépôt local.
>
> **Durée** 15 min · **Mode** seul · **Prérequis** B.2

**À faire**

1. Dans le projet, ouvrez *Code → Branches → New branch*. Nom : `notes-web`, depuis `main`.
2. Basculez sur `notes-web`, puis `+ → New file`.
3. Donnez le chemin `notes/2026-06-06.md` et un contenu de trois lignes. Committez avec un message explicite.
4. Ouvrez *Commits* et *Branches* : retrouvez votre commit et la branche.
5. Dans votre dépôt local, lancez `git log --all --oneline` **sans** `git fetch`. Le commit web est-il visible ? Pourquoi ?

**✅ Preuve de réussite**

- La branche `notes-web` existe sur GitLab.
- Le commit apparaît dans l'historique de la branche.
- En local, `git log --all` ne montre pas encore ce commit : il n'a pas été récupéré.

<details>
<summary>💡 Indices</summary>

- L'interface web travaille directement sur le dépôt distant ; votre dépôt local ne le sait pas tant que vous ne faites pas `git fetch`.
- Le bouton `+` se trouve dans la barre de fichiers du projet.
- Un fichier créé dans une branche n'existe pas dans `main` tant que la branche n'est pas fusionnée.

</details>

<details>
<summary>🔧 Solution</summary>

1. Branche `notes-web` créée depuis `main`.
2. Fichier `notes/2026-06-06.md` créé avec trois lignes.
3. Commit « Ajoute la note du 6 juin depuis le web ».
4. Commit et branche visibles dans GitLab.
5. En local, `git log --all --oneline` ne montre que les commits connus ; `git fetch` les récupérera.

</details>

**🚀 Défi** — Modifiez deux fois le fichier depuis le web. Combien de commits sont créés ? L'interface permet-elle d'amender un commit ? Si non, comment faire autrement ?

---

### B.4 — Publier le carnet, puis le récupérer ailleurs {#b4}

> **Mission** — Reliez votre dépôt local au projet vide, poussez `main`, puis clonez le projet dans un second dossier pour vérifier que l'historique est identique.
>
> **Durée** 25 min · **Mode** seul · **Prérequis** A.9 et B.2

**À faire**

1. Dans `~/carnet-de-bord`, ajoutez le dépôt distant :
   ```shell
   $ git remote add origin <URL du projet>
   $ git remote -v
   ```
2. Poussez la branche principale :
   ```shell
   $ git push -u origin main
   ```
3. Sur GitLab, vérifiez que les fichiers et les commits sont arrivés. Si la branche `notes-web` créée en B.3 est devenue la branche par défaut, choisissez `main` dans *Settings → Repository → Default branch*.
4. Dans le dossier parent, clonez le projet dans un second dossier :
   ```shell
   $ cd ..
   $ git clone <URL> carnet-de-bord-copie
   $ cd carnet-de-bord-copie
   $ git log --oneline main
   ```
5. Comparez avec l'historique de `~/carnet-de-bord`. Vérifiez aussi `git remote -v` et `git status -sb` dans la copie.

**✅ Preuve de réussite**

```shell
$ git log --oneline main   # dans le dépôt d'origine
$ git log --oneline main   # dans la copie
# les deux listes sont identiques
$ git status -sb
## main...origin/main
```

GitLab affiche le README et les notes ; la copie connaît `origin` et suit `origin/main`.

<details>
<summary>💡 Indices</summary>

- `git remote add origin <url>` associe un nom (`origin`) à une URL.
- `git push -u origin main` pousse **et** configure le tracking pour les prochains `push`/`pull`.
- `git clone <url> <dossier>` crée un nouveau dépôt avec un remote `origin` déjà configuré.
- Si le push est rejeté parce que le projet distant contient déjà un README, deux options : recréer un projet vide (le plus propre) ou fusionner les historiques avec `git pull --allow-unrelated-histories`.

</details>

<details>
<summary>🔧 Solution</summary>

```shell
$ cd ~/carnet-de-bord
$ git remote add origin git@gricad-gitlab.univ-grenoble-alpes.fr:VOTRE_NAMESPACE/carnet-de-bord.git
$ git remote -v
$ git push -u origin main
$ cd ..
$ git clone git@gricad-gitlab.univ-grenoble-alpes.fr:VOTRE_NAMESPACE/carnet-de-bord.git carnet-de-bord-copie
$ cd carnet-de-bord-copie
$ git log --oneline main
$ git status -sb
```

Si vous avez initialisé le projet avec un README par erreur :

```shell
$ git pull --allow-unrelated-histories origin main
# résoudre l'éventuel conflit sur README.md, puis :
$ git push -u origin main
```

</details>

**🚀 Défi** — Clonez une fois en HTTPS et une fois en SSH. Quelles différences d'authentification observez-vous ? Pourquoi ne faut-il jamais mettre son mot de passe dans l'URL d'un remote ?

---

### B.5 — Synchroniser dans les deux sens {#b5}

> **Mission** — Faites une modification sur le web et une en local, récupérez-les sans rien perdre, puis provoquez un push refusé et réparez la situation.
>
> **Durée** 30 min · **Mode** seul · **Prérequis** B.4

**À faire**

1. **Côté web** : sur `main`, éditez le README et ajoutez une section « Journal » avec une ligne. Committez.
2. **Côté local** : récupérez les données distantes sans modifier votre branche :
   ```shell
   $ git fetch origin
   $ git status
   $ git log --oneline main..origin/main
   $ git merge origin/main
   ```
3. **Côté web** : ajoutez une deuxième ligne au « Journal ». **Côté local** : récupérez-la en une seule commande avec `git pull`.
4. **Côté local** : créez `notes/2026-06-07.md`, committez, puis `git push`. Vérifiez sur GitLab.
5. **Divergence** : modifiez la première ligne du « Journal » sur le web. En local, modifiez la **deuxième** ligne sans avoir tiré les changements. Committez, puis `git push` : Git refuse. Faites `git pull`, résolvez l'éventuel conflit, puis poussez à nouveau.

**✅ Preuve de réussite**

- `git status -sb` indique que `main` est à jour avec `origin/main`.
- `git log --oneline --graph --all` montre les commits web et locaux.
- GitLab affiche les deux modifications.
- Aucun travail n'a été perdu.

<details>
<summary>💡 Indices</summary>

- `git fetch` télécharge les objets et met à jour `origin/main`, sans toucher à votre branche de travail.
- `git merge origin/main` fusionne la branche distante dans votre branche courante.
- `git pull` = `git fetch` + `git merge`.
- Un push refusé signifie que le distant a avancé : tirez, fusionnez, puis poussez.

</details>

<details>
<summary>🔧 Solution</summary>

```shell
# après la modification web
$ git fetch origin
$ git log --oneline main..origin/main
$ git merge origin/main

# après la deuxième modification web
$ git pull

# modification locale
$ git add notes/2026-06-07.md
$ git commit -m "Ajoute la note du 7 juin"
$ git push

# divergence
$ git push
# ! [rejected] main -> main (fetch first)
$ git pull
# résoudre le conflit si nécessaire, puis :
$ git add README.md
$ git commit
$ git push
```

</details>

**🚀 Défi** — Comparez `git pull` et `git pull --rebase` sur une copie du dépôt, avec un historique divergé. Quel graphe obtenez-vous dans chaque cas ? Le cours n'utilise pas `rebase` : contentez-vous d'observer.

---

### B.6 — Branches distantes {#b6}

> **Mission** — Publiez la branche `annexe-web`, listez les branches locales et distantes, supprimez la branche locale, puis faites-la revenir depuis le dépôt distant.
>
> **Durée** 20 min · **Mode** seul · **Prérequis** B.5

**À faire**

1. Créez la branche `annexe-web`, ajoutez une référence dans `annexes/lectures.md`, committez, puis publiez-la :
   ```shell
   $ git switch -c annexe-web
   $ git push -u origin annexe-web
   ```
2. Listez toutes les branches : `git branch -a`. Repérez `remotes/origin/annexe-web`.
3. Revenez sur `main`, supprimez la branche locale. Elle n'est pas fusionnée : `-D` force la suppression, mais le distant la conserve.
   ```shell
   $ git switch main
   $ git branch -D annexe-web
   $ git branch -a
   ```
4. Récupérez les données distantes, puis recréez la branche locale depuis la branche distante :
   ```shell
   $ git fetch
   $ git switch annexe-web
   $ git status -sb
   $ git branch -vv
   ```

**✅ Preuve de réussite**

- `git branch -a` montre encore `remotes/origin/annexe-web` après la suppression locale.
- La branche locale recréée suit `origin/annexe-web` : `git branch -vv` affiche `[origin/annexe-web]`.
- `git status -sb` affiche `## annexe-web...origin/annexe-web`.

<details>
<summary>💡 Indices</summary>

- `git push -u origin <branche>` publie la branche et configure le tracking.
- `git branch -a` liste les branches locales (`main`, `annexe-web`) et distantes (`remotes/origin/...`).
- `git switch <branche>` crée une branche locale à partir de la branche distante si elle n'existe pas encore localement.

</details>

<details>
<summary>🔧 Solution</summary>

```shell
$ git switch -c annexe-web
$ git add annexes/lectures.md
$ git commit -m "Ajoute une lecture à l'annexe"
$ git push -u origin annexe-web
$ git branch -a
$ git switch main
$ git branch -D annexe-web
$ git branch -a
$ git fetch
$ git switch annexe-web
$ git status -sb
$ git branch -vv
```

</details>

**🚀 Défi** — Poussez une nouvelle branche **sans** `-u`, lisez le message d'erreur, puis corrigez avec `git push -u origin <branche>`. Que configure exactement `-u` ?

---

## Étape 5 — Travailler à plusieurs (B.7 à B.11) {#etape-5}

### B.7 — Un README qui sert vraiment {#b7}

> **Mission** — Réécrivez le `README.md` pour qu'un nouveau venu comprenne le carnet en deux minutes. Utilisez le Markdown de GitLab de manière **utile**, pas comme une vitrine de syntaxe.
>
> **Durée** 30 min · **Mode** seul · **Prérequis** B.4

**À faire**

Travaillez sur une branche `readme`, puis intégrez le résultat dans `main`.

Le README doit contenir :

| Élément | Attendu |
|---|---|
| Titre et pitch | Un H1, puis une phrase qui dit à quoi sert le carnet |
| Sommaire | Une liste de liens vers les sections |
| Table des notes | Date, sujet, statut |
| Prochaines étapes | Une liste de tâches à cocher |
| Cloner | Un bloc de code avec la commande et l'URL |
| Image | Une capture d'écran ou un schéma, avec un texte alternatif |
| Lien | Un lien vers la documentation Markdown de GitLab |

1. Créez la branche `readme`, éditez le README, ajoutez `images/capture.png` (une capture de `git log --oneline --graph`, par exemple).
2. Committez, poussez, puis ouvrez le README sur GitLab pour vérifier le rendu.
3. Testez le sommaire : chaque lien doit amener à la bonne section.
4. Vérifiez que l'image s'affiche et que son texte alternatif est présent.
5. Fusionnez `readme` dans `main` (`git switch main`, `git merge readme`), puis poussez.

**✅ Preuve de réussite**

- Le README affiche un tableau, une liste de tâches et une image.
- Les liens du sommaire fonctionnent.
- Un voisin peut dire en une phrase à quoi sert le carnet et par où commencer.

<details>
<summary>💡 Indices</summary>

- Sommaire : `[Les notes](#les-notes)` pointe vers un titre `## Les notes`.
- Tableau : `| Date | Sujet | Statut |` puis `|---|---|---|`.
- Tâche : `- [ ]` (à faire) et `- [x]` (fait).
- Image : `![Capture du graphe Git](images/capture.png)`.
- Bloc de code : trois accents graves, puis `shell`.

</details>

<details>
<summary>🔧 Solution</summary>

````markdown
# Carnet de bord

Notes de suivi de mon projet : décisions, manipulations, prochaines étapes.

## Sommaire

- [Les notes](#les-notes)
- [Prochaines étapes](#prochaines-etapes)
- [Cloner le carnet](#cloner-le-carnet)

## Les notes

| Date | Sujet | Statut |
|---|---|---|
| 2026-06-01 | Mise en route | terminé |
| 2026-06-03 | Premier prototype | en cours |
| 2026-06-07 | Retours de l'équipe | à faire |

## Prochaines étapes

- [x] Créer le dépôt
- [x] Publier sur GitLab
- [ ] Relire les notes avec l'équipe
- [ ] Publier le carnet avec GitLab Pages

## Cloner le carnet

```shell
git clone git@gricad-gitlab.univ-grenoble-alpes.fr:VOTRE_NAMESPACE/carnet-de-bord.git
```

![Capture du graphe Git](images/capture.png)

[Syntaxe Markdown de GitLab](https://docs.gitlab.com/ee/user/markdown.html)
````

</details>

**🚀 Défi** — Ajoutez une section repliable pour l'historique :

```html
<details>
<summary>Historique</summary>

Contenu replié.

</details>
```

Vérifiez le rendu sur GitLab.

---

### B.8 — À deux sur le même dépôt {#b8}

> **Mission** — À deux, ajoutez chacun votre fiche et votre ligne dans le tableau des participants du projet `common`, puis synchronisez sans perdre la ligne de l'autre.
>
> **Durée** 30 min · **Mode** binôme · **Prérequis** B.5

**À faire**

1. Les deux participants clonent le projet `common` fourni par le formateur.
2. Chacun crée `participants/<login>.md` avec : nom, rôle, une attente pour la formation.
3. Chacun ajoute sa ligne dans le tableau des participants du `README.md`.
4. Le premier pousse. Le second pousse : Git refuse. Le second fait `git pull`, résout le conflit dans le tableau **en gardant les deux lignes**, puis pousse.
5. Vérifiez sur GitLab que les deux fichiers et les deux lignes sont présents.

**✅ Preuve de réussite**

- Le tableau du README contient les deux lignes.
- Les deux fichiers `participants/<login>.md` existent.
- `git log --oneline --graph --all` montre les contributions des deux participants.
- Aucune ligne n'a été écrasée.

<details>
<summary>💡 Indices</summary>

- Travaillez dans des fichiers différents quand c'est possible ; le conflit n'arrive que sur les lignes modifiées par les deux.
- Après résolution : `git add README.md`, `git commit`, `git push`.
- Pour vérifier que vous n'avez rien perdu, comparez avec `git log -p -- README.md`.

</details>

<details>
<summary>🔧 Solution</summary>

```shell
# participant 1
$ git clone <url-common>
$ cd common
$ # créer participants/alice.md et modifier README.md
$ git add participants/alice.md README.md
$ git commit -m "Ajoute la fiche d'Alice"
$ git push origin main

# participant 2, dans son propre clone
$ git clone <url-common>
$ cd common
$ # créer participants/bob.md et modifier README.md
$ git add participants/bob.md README.md
$ git commit -m "Ajoute la fiche de Bob"
$ git push origin main
# → refusé : le distant a avancé
$ git pull
# → conflit dans README.md : garder les deux lignes
$ git add README.md
$ git commit -m "Fusionne les fiches d'Alice et de Bob"
$ git push origin main
```

Si `main` est protégée, créez tous les deux la branche `participants`, poussez-la, et ouvrez une MR à la fin : le conflit se produira de la même façon au second push.

</details>

**🚀 Défi** — Comparez `git pull` et `git pull --rebase` sur une copie. Lequel crée un commit de fusion ? Lequel donne un historique linéaire ? Lequel est le plus simple pour un tableau partagé ?

---

### B.9 — Un backlog utilisable {#b9}

> **Mission** — Transformez trois remarques sur le projet `common` en issues actionnables, triez-les, puis fermez-en une par un commit.
>
> **Durée** 30 min · **Mode** binôme · **Prérequis** B.8

**À faire**

1. Créez trois issues avec un gabarit simple : **Contexte / Attendu / Étapes**. Exemples : un lien cassé dans le README, un participant manquant, une idée de section.
2. Ajoutez des labels explicites (`bug`, `documentation`, `idée`), assignez-vous une issue et rattachez-la à un jalon (*Milestone*) `Atelier GitLab`.
3. Ouvrez le *Board* et créez les colonnes `À faire / En cours / Fait`. Déplacez les cartes.
4. Créez une branche `fix-lien`, corrigez le lien, puis committez avec un message contenant `Fix #<numéro>`. Poussez.
5. Si `main` est protégée, ouvrez une merge-request : l'issue se fermera à la fusion. Sinon, vérifiez qu'elle est fermée après le push.
6. Dans un commentaire d'issue, mentionnez un collègue avec `@username`. Vérifiez qu'il reçoit une notification.

**✅ Preuve de réussite**

- L'issue est fermée automatiquement (ou le sera à la fusion de la MR).
- Le Board montre les cartes dans les bonnes colonnes.
- Les labels et le jalon sont visibles sur les issues.
- Le collègue confirme avoir reçu la notification.

<details>
<summary>💡 Indices</summary>

- Une issue actionnable commence par un verbe et décrit un résultat vérifiable.
- Les labels se créent dans *Manage → Labels*.
- `Fix #12` dans un message de commit ferme l'issue 12 lors de la fusion dans la branche par défaut.
- Les quick actions (`/label ~bug`, `/assign @user`) fonctionnent dans la description d'une issue.

</details>

<details>
<summary>🔧 Solution</summary>

1. Trois issues créées avec le gabarit.
2. Labels `bug`, `documentation`, `idée` ; jalon `Atelier GitLab`.
3. Board avec trois colonnes ; cartes déplacées.
4. Branche `fix-lien`, correction, commit `Corrige le lien du README\n\nFix #12`, push.
5. MR ou push direct selon la protection de `main`.
6. Commentaire `@bob peux-tu vérifier ce lien ?`.

</details>

**🚀 Défi** — Utilisez les quick actions dans une nouvelle issue : `/label ~documentation`, `/assign @username`, `/milestone %Atelier GitLab`. Observez le résultat sans passer par les menus.

---

### B.10 — Merge-request et revue {#b10}

> **Mission** — Proposez votre fiche via une merge-request, faites une revue argumentée de celle de votre binôme, corrigez la vôtre, puis fusionnez.
>
> **Durée** 40 min · **Mode** binôme · **Prérequis** B.9

**À faire**

1. Créez la branche `fiche-<login>` depuis `main`, ajoutez ou modifiez `participants/<login>.md`, puis poussez :
   ```shell
   $ git push -u origin fiche-<login>
   ```
2. Ouvrez une **merge-request** vers `main`. Renseignez :
   - un titre explicite ;
   - une description avec « Ce que fait cette MR », « Comment vérifier », et `Closes #<numéro>` si une issue est concernée ;
   - un reviewer (votre binôme).
3. **Relisez la MR de votre binôme** : lisez le diff, laissez au moins deux commentaires (une question ou suggestion, un point bloquant ou une approbation), puis approuvez ou demandez des changements.
4. Corrigez votre MR selon les retours, poussez, puis demandez une nouvelle revue. Fusionnez après approbation et supprimez la branche.
5. Protégez `main` : *Settings → Repository → Protected branches*. Interdisez le push direct et autorisez la fusion via MR.

**✅ Preuve de réussite**

- La MR est fusionnée et contient des commentaires de revue.
- `main` est protégée : un push direct est refusé.
- `git log --oneline --graph` montre la fusion.
- L'issue liée est fermée.

<details>
<summary>💡 Indices</summary>

- Une revue utile ne dit pas seulement « OK » : elle explique **ce qui** pourrait poser problème et **pourquoi**.
- « Request changes » bloque la fusion tant que les changements ne sont pas faits (selon les réglages).
- Après un push de correction, la MR se met à jour automatiquement.
- La protection de `main` se configure au niveau du projet, pas du dépôt local.

</details>

<details>
<summary>🔧 Solution</summary>

```shell
$ git switch -c fiche-alice
$ git add participants/alice.md
$ git commit -m "Ajoute la fiche d'Alice"
$ git push -u origin fiche-alice
```

Puis, dans GitLab :

1. *Merge requests → New merge request* : `fiche-alice` → `main`.
2. Description :

```markdown
## Ce que fait cette MR

Ajoute la fiche d'Alice et sa ligne dans le tableau des participants.

## Comment vérifier

- Ouvrir `participants/alice.md`.
- Vérifier que le tableau du README contient la ligne d'Alice.

Closes #12
```

3. Reviewer : votre binôme.
4. Revue : un commentaire sur le contenu, un sur la forme ; approbation.
5. Fusion, puis suppression de la branche.

</details>

**🚀 Défi** — Ouvrez une MR en **Draft**, utilisez `/assign_reviewer @username`, puis `/ready` quand elle est prête. Quelle différence de comportement observez-vous ?

---

### B.11 — Fork : contribuer sans être membre {#b11}

> **Mission** — Corrigez un vrai défaut dans le projet d'une autre équipe via un fork, puis proposez la correction en merge-request au projet source.
>
> **Durée** 30 min · **Mode** groupe · **Prérequis** B.10

**À faire**

1. Trouvez un projet dont vous n'êtes pas membre (le `common` d'une autre équipe, ou le dépôt [`awesome-website`](https://gricad-gitlab.univ-grenoble-alpes.fr/git_cnrs/awesome-website) fourni par le formateur).
2. Faites un **fork** vers votre espace personnel. Notez le nom du fork.
3. Clonez votre fork, créez une branche `fix-<sujet>`, corrigez un vrai défaut (faute, lien cassé, phrase incompréhensible), committez et poussez.
4. Ouvrez une merge-request **de votre fork** vers la branche `main` du projet source. Décrivez le problème et la correction.
5. Demandez à un membre du projet source (ou au formateur) de relire et de fusionner.

**✅ Preuve de réussite**

- La MR est visible dans le projet **source**, avec une source du type `votre_namespace:fix-...` et une cible `source:main`.
- La correction est fusionnée et apparaît dans l'historique du projet source.
- Votre fork n'est pas le projet source : vous avez contribué sans y être membre.

<details>
<summary>💡 Indices</summary>

- Le bouton *Fork* se trouve en haut à droite de la page du projet.
- Après le fork, clonez **votre** fork, pas le projet source.
- Dans la MR, la branche source est celle de votre fork ; la branche cible est celle du projet source.
- Si votre fork a divergé, synchronisez-le avec le projet source avant de proposer la MR.

</details>

<details>
<summary>🔧 Solution</summary>

```shell
$ git clone git@gricad-gitlab.univ-grenoble-alpes.fr:<votre_namespace>/common.git
$ cd common
$ git remote add upstream git@gricad-gitlab.univ-grenoble-alpes.fr:<namespace_source>/common.git
$ git switch -c fix-lien
$ # correction
$ git add .
$ git commit -m "Corrige le lien vers la documentation"
$ git push -u origin fix-lien
```

Puis dans GitLab : *Merge requests → New merge request*, source = votre fork, cible = projet source.

</details>

**🚀 Défi** — Après la fusion, synchronisez votre fork : `git fetch upstream`, `git merge upstream/main`, `git push origin main`. Que se passe-t-il si vous aviez d'autres branches dans votre fork ?

---

## Étape 6 — Automatiser et publier (B.12 et B.13) {#etape-6}

### B.12 — Un pipeline qui sert à quelque chose {#b12}

> **Mission** — Ajoutez au carnet un job de CI qui échoue sur un vrai problème, constatez l'échec dans les logs, corrigez, puis vérifiez que le pipeline passe.
>
> **Durée** 30 min · **Mode** seul · **Prérequis** B.5

**À faire**

1. Sur une branche `ci`, créez `.gitlab-ci.yml` avec un job qui vérifie le README :

   ```yaml
   stages:
     - verifier

   verifier-readme:
     stage: verifier
     image: alpine:3.20
     script:
       - test -f README.md
       - if grep -q "TODO" README.md; then echo "README contient encore TODO"; exit 1; fi
       - echo "README OK"
   ```

2. Ajoutez volontairement une ligne `TODO` dans le README, committez et poussez.
3. Ouvrez *CI/CD → Pipelines*, cliquez sur le pipeline rouge, puis sur le job. Lisez le log et repérez la ligne exacte qui a fait échouer le job.
4. Retirez le `TODO`, committez et poussez. Le pipeline doit passer au vert.
5. Ouvrez une merge-request et fusionnez la branche `ci` dans `main`.

**✅ Preuve de réussite**

- Le premier pipeline est rouge ; le log contient `README contient encore TODO`.
- Le second pipeline est vert ; le job affiche `README OK`.
- `.gitlab-ci.yml` est valide et le job se déclenche à chaque push.

<details>
<summary>💡 Indices</summary>

- Un job est une suite de commandes shell exécutées dans l'image indiquée.
- `grep -q` ne produit rien et renvoie 0 si le motif est trouvé, 1 sinon.
- `exit 1` fait échouer le job ; c'est ce que l'on veut pour signaler un problème.
- Si le pipeline reste *pending*, aucun runner n'est disponible : vérifiez *Settings → CI/CD → Runners* ou demandez au formateur.

</details>

<details>
<summary>🔧 Solution</summary>

```yaml
stages:
  - verifier

verifier-readme:
  stage: verifier
  image: alpine:3.20
  script:
    - test -f README.md
    - if grep -q "TODO" README.md; then echo "README contient encore TODO"; exit 1; fi
    - echo "README OK"
```

```shell
$ git switch -c ci
$ git add .gitlab-ci.yml
$ git commit -m "Ajoute une vérification du README"
$ git push -u origin ci
# ajouter TODO dans README.md, commit, push, observer l'échec
# retirer TODO, commit, push, observer le succès
```

</details>

**🚀 Défi** — Ajoutez un second job `verifier-liens` qui échoue si le README contient `example.com`, et faites-le tourner dans le même stage. Les deux jobs s'exécutent-ils en parallèle ? Que se passe-t-il si l'un échoue ?

---

### B.13 — Donner une URL au carnet {#b13}

> **Mission** — Publiez le carnet avec GitLab Pages. Vérifiez qu'un push sur `main` déploie, et qu'un push sur une branche ne déploie pas.
>
> **Durée** 40 min · **Mode** seul · **Prérequis** B.12

**À faire**

1. Créez une branche `pages` (`git switch -c pages`), puis `site/index.html` et `site/style.css` (un modèle est fourni dans la solution). Personnalisez le titre, l'introduction et deux liens vers vos notes. Remplacez `VOTRE_NAMESPACE` par le vôtre.
2. Complétez `.gitlab-ci.yml` avec un job `pages` :

   ```yaml
   stages:
     - verifier
     - deploy

   # ... job verifier-readme ...

   pages:
     stage: deploy
     image: alpine:3.20
     script:
       - mkdir -p public
       - cp -r site/. public/
     artifacts:
       paths:
         - public
     rules:
       - if: $CI_COMMIT_BRANCH == "main"
   ```

3. Committez, poussez la branche `pages`, et vérifiez que le job `pages` **ne s'exécute pas** (les règles le réservent à `main`).
4. Ouvrez une merge-request, fusionnez dans `main`, puis regardez le pipeline : le job `pages` doit s'exécuter.
5. Ouvrez *Déploiements → Pages* et notez l'URL. Vérifiez que la page s'affiche et que vos liens fonctionnent.
6. Modifiez le site sur une branche, poussez : aucun déploiement. Fusionnez : le site est mis à jour.

**✅ Preuve de réussite**

- L'URL Pages affiche le titre et les liens attendus.
- Le job `pages` ne s'exécute que sur `main`.
- Un push sur une branche ne modifie pas le site publié.
- Le job produit bien un artefact `public/`.

<details>
<summary>💡 Indices</summary>

- GitLab Pages publie le contenu du dossier `public/` produit par le job.
- `artifacts.paths` doit contenir `public` pour que GitLab récupère le dossier.
- `rules` remplace l'ancien `only` ; `$CI_COMMIT_BRANCH == "main"` limite le job à la branche principale.
- L'URL exacte est affichée dans *Déploiements → Pages* ; elle dépend de la plateforme.

</details>

<details>
<summary>🔧 Solution</summary>

`site/index.html` :

```html
<!doctype html>
<html lang="fr">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Carnet de bord</title>
  <link rel="stylesheet" href="style.css">
</head>
<body>
  <header>
    <h1>Carnet de bord</h1>
    <p>Notes de suivi de mon projet : décisions, manipulations, prochaines étapes.</p>
  </header>
  <main>
    <h2>Dernières notes</h2>
    <ul>
      <li><a href="https://gricad-gitlab.univ-grenoble-alpes.fr/VOTRE_NAMESPACE/carnet-de-bord/-/blob/main/notes/2026-06-01.md">Note du 1er juin</a></li>
      <li><a href="https://gricad-gitlab.univ-grenoble-alpes.fr/VOTRE_NAMESPACE/carnet-de-bord/-/blob/main/notes/2026-06-04.md">Note du 4 juin</a></li>
    </ul>
    <p><a href="https://gricad-gitlab.univ-grenoble-alpes.fr/VOTRE_NAMESPACE/carnet-de-bord">Voir le dépôt</a></p>
  </main>
</body>
</html>
```

`site/style.css` :

```css
:root {
  color-scheme: light dark;
  font-family: system-ui, sans-serif;
  line-height: 1.5;
}
body {
  max-width: 42rem;
  margin: 3rem auto;
  padding: 0 1rem;
}
h1 { border-bottom: 3px solid #f71c1c; padding-bottom: .3rem; }
a { color: #f71c1c; }
```

`.gitlab-ci.yml` complet :

```yaml
stages:
  - verifier
  - deploy

verifier-readme:
  stage: verifier
  image: alpine:3.20
  script:
    - test -f README.md
    - if grep -q "TODO" README.md; then echo "README contient encore TODO"; exit 1; fi
    - echo "README OK"

pages:
  stage: deploy
  image: alpine:3.20
  script:
    - mkdir -p public
    - cp -r site/. public/
  artifacts:
    paths:
      - public
  rules:
    - if: $CI_COMMIT_BRANCH == "main"
```

</details>

**🚀 Défi** — Ajoutez une seconde page `site/notes.html` reliée depuis l'index et vérifiez qu'elle est publiée. Si vous préférez un générateur de site statique, Zola fait le travail en une commande : `zola build` écrit le site dans `public/`, que le job publie tel quel. Le dépôt [`awesome-website`](https://gricad-gitlab.univ-grenoble-alpes.fr/git_cnrs/awesome-website) fourni par le formateur est un projet Zola et peut servir de variante.

---

# Dépannage {#depannage}

| Symptôme | Cause probable | À faire |
|---|---|---|
| `fatal: not a git repository` | Vous n'êtes pas dans le dépôt | `pwd`, `cd ~/carnet-de-bord`, `git status` |
| `Author identity unknown` | A.1 n'a pas été fait | `git config --global user.name` et `user.email` |
| `Updates were rejected because the remote contains work...` | Le distant a avancé | `git pull`, résoudre, `git push` |
| `fatal: refusing to merge unrelated histories` | Le projet distant a été initialisé avec un README | Recréer un projet vide, ou `git pull --allow-unrelated-histories` |
| `error: Your local changes would be overwritten by merge` | Des modifications non commitées gênent la fusion | Committer ou `git stash`, puis réessayer |
| Le README contient `<<<<<<<` | Un conflit n'a pas été résolu | A.9 : éditer, supprimer les marqueurs, `git add`, `git commit` |
| `Permission denied (publickey)` | Clé SSH absente ou mauvaise URL | Ajouter la clé dans *Settings → SSH Keys*, ou utiliser HTTPS |
| `You are not allowed to push code to this project` | Rôle insuffisant ou `main` protégée | Pousser une branche et ouvrir une MR |
| Le pipeline reste `pending` | Aucun runner disponible | *Settings → CI/CD → Runners* ; demander au formateur |
| Pages renvoie 404 | Artefact `public/` absent ou job non exécuté sur `main` | Vérifier `artifacts.paths` et `rules` ; relire le log du job |
| `git branch -d` refuse de supprimer | La branche n'est pas fusionnée | Fusionner, ou `git branch -D` en connaissance de cause |

---

# Auto-évaluation {#autoevaluation}

Cochez ce que vous savez faire **sans aide**. Pour chaque point, le numéro renvoie à l'exercice correspondant.

**Git en local**

- [ ] Je pars d'un dossier vide et j'arrive à un dépôt avec un historique lisible (A.2 à A.5).
- [ ] Je sais indexer seulement une partie de mes modifications, puis annuler une indexation (A.4).
- [ ] Je retrouve un commit fautif et je l'annule sans réécrire l'historique (A.6).
- [ ] Je nettoie un dépôt sans perdre les fichiers que je veux garder sur le disque (A.7).
- [ ] Je crée une branche, je la fusionne et je distingue un fast-forward d'un commit de fusion (A.8).
- [ ] Je provoque un conflit, je le lis et je le résous sans perdre le travail des deux branches (A.9).

**GitLab**

- [ ] Je crée un projet vide et j'y pousse un dépôt local existant (B.2 et B.4).
- [ ] Je clone un projet et je vérifie que l'historique est identique (B.4).
- [ ] Je synchronise deux dépôts qui ont divergé, sans perdre de commit (B.5).
- [ ] Je publie une branche et je la récupère depuis le dépôt distant (B.6).
- [ ] J'écris un README qui permet à un nouveau venu de démarrer seul (B.7).
- [ ] Je travaille à deux sur le même dépôt et je résous le conflit du tableau partagé (B.8).
- [ ] Je transforme une remarque en issue actionnable et je la ferme par un commit (B.9).
- [ ] Je fais une revue de MR qui aide réellement l'auteur, puis je fusionne (B.10).
- [ ] Je contribue à un projet dont je ne suis pas membre via un fork (B.11).
- [ ] Je diagnostique un pipeline rouge et je le rends vert (B.12).
- [ ] Je publie le carnet avec Pages et je vérifie qu'une branche ne déploie pas (B.13).

---

# Pour aller plus loin {#pour-aller-plus-loin}

Ces commandes n'ont pas été présentées en détail. Testez-les sur le carnet, puis notez ce qu'elles changent dans `git status` et `git log`.

| Commande | À essayer | Ce que vous devez observer |
|---|---|---|
| `git diff` | Modifiez un fichier sans l'indexer, puis `git diff` | La différence entre le dernier commit et le disque |
| `git diff --staged` | Indexez la modification, puis `git diff --staged` | La différence entre l'index et le dernier commit |
| `git stash` | Modifiez un fichier, `git stash`, changez de branche, `git stash pop` | Les modifications mises de côté puis restaurées |
| `git tag` | `git tag v1.0` sur un commit, puis `git tag` | Une étiquette posée sur un commit |
| `git rebase -i` | Sur une **copie** du dépôt, réorganisez trois commits | Un historique réécrit ; à ne pas faire sur une branche partagée |
| `git cherry-pick` | Sur une copie, rejouez un commit d'une branche sur une autre | Un nouveau commit avec les mêmes changements |
| `git log --graph --oneline --all --decorate` | Sur le carnet | Une vue synthétique de toutes les branches |

**Défi final** — Choisissez une commande que vous n'avez jamais utilisée, lisez sa page `man` ou la documentation Git, testez-la sur une copie du carnet, puis expliquez à votre voisin ce qu'elle fait et dans quel cas elle est utile.

---

# Ce que vous avez construit {#bilan}

À la fin de la formation, votre carnet contient :

```text
carnet-de-bord/
├── .gitlab-ci.yml
├── .gitignore
├── README.md
├── annexes/
│   ├── lectures.md
│   └── ressources.md
├── brouillons/          # non suivi
│   └── idees.md
├── images/
│   └── capture.png
├── notes/
│   ├── 2026-06-01.md
│   ├── ...
│   └── 2026-06-07.md
└── site/
    ├── index.html
    └── style.css
```

Et il est accessible :

- dans votre dépôt local ;
- sur GitLab, avec son historique, ses branches et ses merge-requests ;
- à l'URL affichée dans *Déploiements → Pages*.

Le plus important n'est pas la liste des commandes : c'est de savoir **où vous êtes** dans l'historique, **ce qui est indexé**, et **ce que le dépôt distant connaît**. Le reste s'apprend en pratiquant.
