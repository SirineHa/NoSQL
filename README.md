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

**Redis** (Remote Dictionary Server) est une base de données clé-valeur open source extrêmement performante. Elle est souvent utilisée comme cache en mémoire ou comme file d'attente grâce à ses fonctionnalités avancées et à sa rapidité.
Redis permet de faire la gestion de concurrence (Exemple si plusieur utilisateur se connecte au meme temps)

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

# Creer une interface client serveur qui permet de connecter à Redis
redis-cli

# Ajouter une clé-valeur
SET demo "Bonjour"
SET user:1 "Syrine"

# Récupérer une clé
GET user:1

# Supprimer une clé
DEL user:1

# Definir la duree de vie d'un cle
expire demo 120 # demo s'expire dans 120 s
ttl demo # il renvoie le nombre de la duree de vie restante du cle sinon -1 si la duree de la cle est indefini 
```

#### Exemple : Compter le nombre des visiteur d'un site web
```bash
# Definir une cle : date, valeur:nombre de visiteur initialement 0
set 1mars 0

# Incrementer le nombre de visiteur
incr 1mars

# Decrementer le nombre de visiteur
decr 1mars
```

#### Manipuler les listes : on peut avoir la meme valeur plusieur fois
```bash
# Definir une liste et inserer les données
RPUSH mesCours "BDA"
RPUSH mesCours "Services Web" # renvoie le nombre d'element dans la liste

# Afficher la liste LRANGE [nom_liste] [indice_premier_element] [indice_dernier_element]
LRANGE mesCours 0 -1 # Affiche tout les elements

# Supprimer un element d'une liste
LPOP mesCours # supprimer l'element à gauche
RPOP mesCours # supprimer l'element à droite
```

#### Manipuler les ensembles (Set) : les valeurs doivent etre unique + l'ordre des elements n'est pas important
```bash
# Definir un set et inserer les données
SADD utilisateurs "Augustin" # renvoie 1 pour dire que l'element est ajouté
SADD utilisateurs "Ines"
SADD utilisateurs "Samir"
SADD utilisateurs "Samir" # renvoie 0 pour dire que l'element n'a pas ete ajouté

# Afficher les elements d'un ensemble
SMEMBERS utilisateurs

# Supprimer un element d'un ensemble
SREM utilisateurs "MARC"

# Union de deux ensembles
SADD autresUtilisateurs "Antoine"
SADD autresUtilisateurs "Philippe"
SUNION Utilisateurs AutresUtilistauers

```
