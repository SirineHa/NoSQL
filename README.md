# Introduction aux Bases de Données NoSQL

Les bases de données **NoSQL** sont des systèmes de gestion de bases de données qui ne suivent pas le modèle relationnel classique. Elles sont conçues pour gérer de grandes quantités de données non structurées ou semi-structurées et offrent une flexibilité, une évolutivité horizontale et des performances optimales pour certaines applications, notamment celles qui nécessitent une faible latence.

---

## Base de Données Orientées Clés-Valeurs

### Définition

Les bases de données orientées **clés-valeurs** sont l'une des catégories principales de bases de données NoSQL. Elles stockent les données sous forme de paires **clé-valeur**, où chaque clé unique est associée à une valeur. Ce modèle est simple mais très efficace pour des cas d'utilisation tels que :
- Les caches.
- La gestion de sessions.
- Les systèmes nécessitant des recherches rapides par clé.

- **Clé** : Un identifiant unique pour chaque entrée (souvent une chaîne ou un hachage).
- **Valeur** : Les données associées à la clé, qui peuvent être de n'importe quel type (chaîne, nombre, tableau, objet, etc.).

---

### Avantages

1. **Simplicité** : Modèle facile à comprendre et à utiliser.
2. **Performance élevée** : Accès rapide aux données grâce aux recherches basées sur les clés.
3. **Scalabilité horizontale** : Facilement extensible en ajoutant de nouveaux nœuds au cluster.

### Inconvénients

1. **Absence de relations** : Pas de support natif pour les relations complexes entre données.
2. **Limitation des requêtes** : Recherche uniquement possible par clé, pas de requêtes complexes.

---

## Exemple : Redis

**Redis** (Remote Dictionary Server) est une base de données clé-valeur open source extrêmement performante. Elle est souvent utilisée comme cache en mémoire ou comme file d'attente grâce à ses fonctionnalités avancées et à sa rapidité. Redis permet de faire la gestion de concurrence (exemple : plusieurs utilisateurs se connectant en même temps).

---

### Fonctionnalités clés

1. **Stockage en mémoire** : Redis conserve toutes les données en mémoire pour des performances optimales, avec des sauvegardes sur disque optionnelles.
2. **Structures de données avancées** :
    - Chaînes (**Strings**)
    - Listes (**Lists**)
    - Ensembles (**Sets**)
    - Tables de hachage (**Hashes**)
    - Ensembles ordonnés (**Sorted Sets**)
3. **Pub/Sub** : Fonctionnalité de messagerie entre applications.
4. **Transactions** : Prise en charge des commandes transactionnelles.
5. **TTL (Time to Live)** : Gestion automatique de l'expiration des clés.

---

### Cas d'utilisation

1. **Caching** : Mise en cache des résultats de calculs ou de requêtes.
2. **Gestion de sessions** : Stockage des données de session utilisateur.
3. **Systèmes de file d'attente** : Mise en œuvre de files d'attente distribuées.
4. **Tableau de classement** : Utilisation des ensembles ordonnés pour trier des scores.

---

### Exemple d'utilisation de Redis

#### Commandes CLI
```bash
# Lancer le serveur Redis
redis-server

# Créer une interface client-serveur pour se connecter à Redis
redis-cli

# Ajouter une clé-valeur
SET demo "Bonjour"
SET user:1 "Syrine"

# Récupérer une clé
GET user:1

# Supprimer une clé
DEL user:1

# Définir la durée de vie d'une clé
EXPIRE demo 120  # "demo" expire dans 120 secondes
TTL demo          # Vérifie la durée de vie restante de la clé
```

#### Exemple : Compter le nombre de visiteurs d'un site web
```bash
# Définir une clé : date, valeur initiale = 0
SET 1mars 0

# Incrémenter le nombre de visiteurs
INCR 1mars

# Décrémenter le nombre de visiteurs
DECR 1mars
```

#### Manipuler les listes
Une même valeur peut apparaître plusieurs fois
```bash
# Définir une liste et insérer des données
RPUSH mesCours "BDA"
RPUSH mesCours "Services Web"  # Retourne le nombre d'éléments dans la liste

# Afficher la liste LRANGE [nom_liste] [indice_premier_element] [indice_dernier_element]
LRANGE mesCours 0 -1  # Affiche tous les éléments

# Supprimer un élément d'une liste
LPOP mesCours  # Supprime l'élément à gauche
RPOP mesCours  # Supprime l'élément à droite
```

#### Manipuler les ensembles (Set) 
Les valeurs doivent être uniques, et l'ordre des éléments n'est pas important
```bash
# Définir un ensemble et insérer des données
SADD utilisateurs "Augustin"  # Retourne 1 pour indiquer que l'élément a été ajouté
SADD utilisateurs "Ines"
SADD utilisateurs "Samir"
SADD utilisateurs "Samir"  # Retourne 0 pour indiquer que l'élément existe déjà

# Afficher les éléments d'un ensemble
SMEMBERS utilisateurs

# Supprimer un élément d'un ensemble
SREM utilisateurs "MARC"

# Union de deux ensembles
SADD autresUtilisateurs "Antoine"
SADD autresUtilisateurs "Philippe"
SUNION utilisateurs autresUtilisateurs
```
#### Manipuler les set ordonnees
Utiliser dans les systemes de recommandations
Exemple: classement des scores des utilisateurs
```bash
# Définir un ensemble ordonnee et insérer des données
ZADD score4 19 "Augustin"
ZADD score4 18 "Ine"
ZADD score4 10 "Samir"
ZADD score4 8 "Philippe"

# Afficher la ensemble ordonnee ZRANGE [nom_set] [indice_premier_element] [indice_dernier_element]
ZRANGE score4 0 -1  # renvoie les elements dans l'ordre croissant
ZREVRANGE score4 0 -1  # renvoie les elements dans l'ordre decroissant

# Connetre l'indice d'un element (les elements sont triees dans l'ordre croissant et le 1er element est d'indice 0)
ZRANK score4 "Augustin" 
```

#### Manipuler les Hash
```bash
# Définir un hash et insérer des données
HSET user:11 username "hsyrine"
HSET user:11 age 31
HSET user:11 username "syrine@paris13.fr"
HMSET user:4 username "Augustin" age 5 email augustin@gmail.fr

# Recuperer les informations de hash
HGETALL user:11

# Recuperer les valeurs
HVALS user:4

# Incrementer un element
HINCRBY user:4 age 4 # incrementer l'age de 4 ans
```

#### Manipuler les PubSub
Tres utiliser dans les applications temps reels, par exemple l'echange des message
On a besoin de deux clients

```bash
# S'inscrire sur un canal : Client1
SUBSCRIBE mescours user:1 
# Publier un  message : Client2
PUBLISH mescours "Un nouveau cours sur MongoDB" # les clients inscit sur le canal mescours recoivent le messsage en temps reel
PUBLISH user:1 "Bonjour user1" # seulement le user1 recoit le message
```

```bash
# S'inscrire sur plusieurs canaux
PSUBSCRIBE mes*
```

```bash
# Afficher les clés sauvegardes
KEYS *
```
#### Autres
Redis met a disposition 16 bases de donnees par defaut (0->15)
Par defaut on est connecte a la base de donnee numero 0

```bash
# Changer de base de donnees SELECT [index]
SELECT 1
```
