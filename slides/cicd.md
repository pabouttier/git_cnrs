---
marp: true
theme: socrates
author: Pierre-Antoine Bouttier
paginate: true
footer: Git(Lab)@CNRS - pierre-antoine.bouttier@univ-grenoble-alpes.fr
---

<!-- _class: titlepage -->


![bg left:33% fit](fig/logo-git.png)
# L'intégration/déploiement continue
## Automatisation des tâches

### GitLab@CNRS - 11-12/12/2023
#### [Pierre-Antoine Bouttier](mailto:pierre-antoine.bouttier@univ-grenoble-alpes.fr)


---
## Qu'est ce que c'est ?

**Concept** : l’intégration continue (CI) est une pratique consistant à vérifier systématiquement l’impact de toute modification du code source sur le fonctionnement, les performances, etc. par la mise en place d’une chaine d’exécution automatique contenant par exemple des ”tests”.

* Permet de produire un code stable, robuste et portable. 
* Permet de s’assurer que le résultat de nouvelles modifications n’introduit pas de régression du code
* Permet d’anticiper différents types d’utilisation du code.
* Permet de faciliter les développements au quotidien.

* **Comment** : Des outils tels que github ou gitlab proposent un outil d’intégration continue simple à utiliser et très fonctionnel.

---
## Par quelle magie ?

À chaque push vers le serveur, des tâches pré-définies par les développeurs sont exécutées.

![w:900 center](fig/ci1.png)

---
## Quelques exemples

![w:900 center](fig/ci2.png)

Ici, il y a du **déploiement continu** aussi. Mais les mécanismes sont les mêmes que pour la CI. Seule la finalité change.

---
## Comment mettre la CI/CD en place ? 

* Il suffit de créer un fichier nommé `.gitlab-ci.yml`à la racine du dépôt du projet
* Dès que ce fichier est présent, l’intégration continue est activée. Ce fichier contient la liste des tâches à effectuer : quelles actions, sur quelles machines etc.
* Définir des ***runners*** : une ou plusieurs machines (éventuellement virtuelles) sur lesquelles seront exécutés les jobs d’intégration continue.
* https://docs.gitlab.com/ee/ci/quick_start/
* **Voir démo**