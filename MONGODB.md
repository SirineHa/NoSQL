# Prise en main de MongoDB

MongoDB est un serveur de gestion de bases de données NoSQL documentaires et distribuées. 

---

## Étape 1 : Importer la collection de films

Utilisez la commande suivante pour importer une collection de films dans la base de données `lesfilms` :

```bash
mongoimport --db lesfilms --collection films --file films.json --jsonArray
```

---

## Étape 2 : Basculer vers la base de données lesfilms

Avant de commencer à travailler avec la base de données lesfilms, sélectionnez-la dans le client MongoDB :

```bash
use lesfilms
```
Cette commande permet de basculer sur la base de données lesfilms. Si elle n'existe pas, MongoDB la créera automatiquement lors de la première opération.

---

## Étape 3 : Vérifier les données importées

Pour vérifier que les données ont bien été importées, comptez les documents dans la collection :

```bash
db.films.count()
```
Cette commande renvoie le nombre total de documents dans la collection films.


---

## Étape 4 : Comprendre la structure des documents

Examinez la structure d'un document pour comprendre la disposition des champs et des données :

```bash
db.films.findOne()
```
La commande findOne() renvoie un seul document de la collection. Cela permet de visualiser les champs disponibles et leur structure.

---

## Étape 5 : Requêtes spécifiques

### 1. Afficher la liste des films d’action

```bash
db.films.find({ genre: "Action" })
```
Cette requête renvoie tous les films dont le champ genre contient "Action".

### 2. Afficher le nombre de films d’action

```bash
db.films.count({ genre: "Action" })
```
Compte le nombre total de documents ayant le champ genre égal à "Action".

### 3. Afficher les films d’action produits en France

```bash
db.films.find({ genre: "Action", country: "FR" })
```
Renvoie tous les films d'action produits en France.

### 4. Afficher les films d’action produits en France en 1963

```bash
db.films.find({ genre: "Action", country: "FR", year: 1963 })
```

---

## Étape 6 : Afficher certains champs spécifiques

### 1. Exclure certains champs comme grades

```bash
db.films.find({ genre: "Action", country: "FR" }, { grades: 0 })

```
Cette commande exclut le champ grades dans les documents renvoyés.


### 2. Exclure les identifiants (_id)

```bash
db.films.find({ genre: "Action", country: "FR" }, { _id: 0, grades: 0 })

```
Supprime les champs _id et grades dans les résultats.

### 3. Afficher uniquement les titres et grades

```bash
db.films.find({ genre: "Action", country: "FR" }, { _id: 0, title: 1, grades: 1 })
```
Renvoie uniquement les champs title et grades pour les films correspondants.

---

## Étape 7 : Requêtes avancées

### 1. Films avec des notes supérieures à 10

```bash
db.films.find({ genre: "Action", country: "FR", "grades.note": { $gt: 10 } }, { _id: 0, title: 1, grades: 1 })
```
Filtre les films dont une note est supérieure à 10 (on remarque que même si
des films ont obtenu certaines notes inférieures à 10 sont affichés).

### 2. Films où toutes les notes sont supérieures à 10

```bash
db.films.find({ genre: "Action", country: "FR", "grades.score": { $not: { $lte: 10 } } }, { _id: 0, title: 1, grades: 1 })
```
Filtre les films où toutes les notes sont strictement supérieures à 10.


### 3. Afficher les genres disponibles

```bash
db.films.distinct("genre")
```
Renvoie la liste unique des genres présents dans la collection.

### 4. Films sans résumé

```bash
db.films.find({ summary: { $exists: false } })
```
Filtre les films qui n'ont pas de champ summary.

### 5. Films avec Leonardo DiCaprio en 1997

```bash
db.films.find({ "actors.last_name": "DiCaprio", "actors.first_name":"Leonardo", year: 1997 })
```
Renvoie les films avec Leonardo DiCaprio sortis en 1997.


### 6. Films avec Leonardo DiCaprio ou tournés en 1997

```bash
db.films.find({$or :[ {"actors.last_name": "DiCaprio"}, {"actors.first_name":"Leonardo"}, {year: 1997} ]})
```
Renvoie tous les films avec Leonardo DiCaprio ou produits en 1997.


