# Réponses aux questions - Volumes Docker

## Exercice 1 - Perte des données sans volume

**Quel résultat dans le premier conteneur, puis dans le second ?**
Le premier affiche `Donnee-temporaire`, le second affiche `ABSENT`.

**Pourquoi la même image Alpine ne suffit-elle pas à retrouver le fichier ?**
L'image est en lecture seule. Le fichier était écrit dans la couche du conteneur, supprimée avec `docker rm`.

**Dans quels cas la perte de ces données poserait-elle problème ?**
Pour une base de données, des fichiers envoyés par les utilisateurs ou des logs.

## Exercice 2 - Volume partagé entre conteneurs

**Les deux conteneurs voient-ils les mêmes fichiers ?**
Oui, ils utilisent le même volume : une modification est visible tout de suite par l'autre.

**Pourquoi le lecteur utilise-t-il `readonly` ?**
Il n'a besoin que de lire : on l'empêche de modifier ou supprimer les données (sécurité).

**Quels risques si deux applications modifient les mêmes fichiers en même temps ?**
Données écrasées, fichiers corrompus ou lus à moitié écrits.

## Exercice 3 - Bind Mount

**Pourquoi la page change-t-elle sans reconstruire l'image ?**
Le dossier du PC est branché dans le conteneur : nginx lit directement le fichier modifié.

**Quelle est la valeur de `Type` dans `docker inspect` ?**
`bind`

**Que signifie `readonly` ?**
Le conteneur peut lire les fichiers mais pas les modifier.

**Les fichiers locaux sont-ils supprimés avec le conteneur ?**
Non, ils appartiennent au PC et restent intacts.

## Exercice 4 - Choisir le bon stockage

| Situation | Choix | Justification |
|---|---|---|
| Faire persister des fichiers de base de données | Volume nommé | Docker gère le stockage, les données survivent au conteneur |
| Voir instantanément les modifications d'un site HTML local | Bind Mount | Le dossier du PC est branché dans le conteneur |
| Exécuter une application temporaire sans données à conserver | Aucun montage | Rien à garder |
| Conserver des données lors de la recréation d'un conteneur | Volume nommé | Le volume reste et se rebranche sur le nouveau conteneur |

**Différence entre un chemin géré par Docker et un chemin choisi sur l'hôte ?**
Volume nommé : Docker choisit l'emplacement. Bind mount : je choisis le dossier sur mon PC.

**Volume nommé et Bind Mount permettent-ils tous les deux la persistance ?**
Oui, les données sont hors du conteneur et survivent à sa suppression.

**Pourquoi ne pas confondre persistance et sauvegarde ?**
La persistance garde les données à un seul endroit. Une sauvegarde est une copie stockée ailleurs.

## Exercice 5 - Nettoyage des volumes

**Le fichier `important.txt` est-il encore présent après recréation du volume ?**
Non, supprimer un volume efface ses données. Le nouveau volume est vide.

**Pourquoi `docker volume prune --all` peut-il être risqué ?**
Il supprime tous les volumes non utilisés, y compris ceux d'autres projets qui contiennent des données.

**Que faire avant de supprimer le volume d'une application de production ?**
Faire une sauvegarde, la tester, et vérifier qu'on supprime le bon volume.

## Atelier PostgreSQL

**Les données sont-elles toujours présentes après remplacement du conteneur ?**
Oui, les 3 lignes sont toujours là car elles sont stockées dans le volume `tp-postgres-data`.

**Limites de cette persistance et besoin de sauvegarde ?**
Le volume protège contre la suppression du conteneur, pas contre la perte du disque ou une suppression du volume. Il faut des sauvegardes régulières stockées ailleurs, par exemple avec `pg_dump`.
