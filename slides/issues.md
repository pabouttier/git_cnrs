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
## Les issues Gitlab, gestionnaire de problème et d'échanges

Le gestionnaire de problème est un outil standard des forges qui permet le suivi efficace de l’évolution des bugs (du signalement à la résolution), mais aussi de proposer, de discuter de nouvelles idées, de suivre l’ajout de nouvelles fonctionnalités, leur évolution (dans des branches).

Bref, c’est un outil indispensable à la vie du projet, qu’il faut exploiter. Il est accessible via le menu Issues, avec en particulier :
* List et Board pour visualiser l’ensemble des problèmes déja déposés, les classer, les commenter ... 
* Milestones : définition d’étapes de développement, planification du projet

---
## Conseils et fonctionnalités utiles sur les issues

* Créez des labels explicites pour classer vos issues (documentation, bugs, newideas...).
* Utilisez le markdown pour rédiger vos issues. 
* Vous pouvez mentionner dans les issues ou dans les messages de commit les autres participants au projet via la chaine @username. Cela entrainera l’envoi d’un mail à la personne concernée, l’ajout d’une tâche dans sa todo-list
* faire référence à une issue dans une autre issue ou un message de commit via `#id`, id étant le numéro de l’issue
* clore automatiquement une issue via un message de commit. Il suffit qu’il contienne la chaîne `Fix #id` (ou un des autres mots-clés mentionnés ici :
https://docs.gitlab.com/ee/user/project/issues/automatic_issue_closing.html).

