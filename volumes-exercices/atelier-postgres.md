# Atelier - Persistance d'une base PostgreSQL

## Mission 1 - Préparer le stockage

    docker volume create tp-postgres-data
    docker volume inspect tp-postgres-data

Driver utilisé : `local`

## Mission 2 - Démarrer la base

    docker run -d --name tp-postgres \
      -e POSTGRES_USER=tpuser \
      -e POSTGRES_PASSWORD='TpDocker2026!' \
      -e POSTGRES_DB=tpdb \
      --mount type=volume,source=tp-postgres-data,target=/var/lib/postgresql/data \
      postgres:17

    docker exec tp-postgres pg_isready -U tpuser -d tpdb

Résultat : `accepting connections`. Le port 5432 n'est pas exposé vers l'hôte.

## Mission 3 - Créer et alimenter la table

    docker exec tp-postgres psql -U tpuser -d tpdb -c "CREATE TABLE apprenants (id INTEGER GENERATED ALWAYS AS IDENTITY PRIMARY KEY, nom VARCHAR(100) NOT NULL, formation VARCHAR(100) NOT NULL);"
    docker exec tp-postgres psql -U tpuser -d tpdb -c "INSERT INTO apprenants (nom, formation) VALUES ('Alice', 'Docker'), ('Bilal', 'DevOps'), ('Chloe', 'Cloud');"
    docker exec tp-postgres psql -U tpuser -d tpdb -c "SELECT * FROM apprenants ORDER BY id;"

## Mission 4 - Simuler une maintenance

    docker rm -f tp-postgres
    docker volume ls --filter name=tp-postgres-data
    # recréation avec la même commande qu'à la mission 2
    docker exec tp-postgres psql -U tpuser -d tpdb -c "SELECT * FROM apprenants ORDER BY id;"

Résultat après recréation du conteneur :

     id |  nom  | formation
    ----+-------+-----------
      1 | Alice | Docker
      2 | Bilal | DevOps
      3 | Chloe | Cloud
    (3 rows)

Les données sont conservées car elles sont stockées dans le volume, pas dans le conteneur.

## Limites et sauvegarde

Le volume protège les données lors du remplacement du conteneur, mais il reste à un seul endroit.
Si le volume est supprimé ou si le disque tombe en panne, les données sont perdues.
Il faut donc faire des sauvegardes régulières, stockées ailleurs :

    docker exec tp-postgres pg_dump -U tpuser -d tpdb > sauvegarde-tpdb.sql
