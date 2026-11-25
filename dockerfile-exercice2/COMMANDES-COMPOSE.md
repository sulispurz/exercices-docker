# Exercice 2 - Commandes Docker Compose

| Commande | Rôle |
|---|---|
| `docker compose config` | vérifier la syntaxe et les variables du fichier |
| `docker compose up -d --build` | construire l'image `quiz-docker:secure` et démarrer le service |
| `docker compose ps` | voir l'état du service (healthy) |
| `docker compose logs -f` | suivre les logs |
| `docker compose stop` | arrêter le service sans le supprimer |
| `docker compose start` | redémarrer le service |
| `docker compose restart` | arrêter puis relancer |
| `docker compose down` | arrêter et supprimer le conteneur et le réseau |

## Vérifications de sécurité

| Test | Résultat |
|---|---|
| `docker compose exec quiz-docker whoami` | `node` (pas root) |
| `docker compose exec quiz-docker touch /app/test.txt` | `Read-only file system` |

Application accessible sur http://localhost:8082
