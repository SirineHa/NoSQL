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
db.films.find({ genre: "Action", pays: "France" }, { grades: 0 })

```
Cette commande exclut le champ grades dans les documents renvoyés.
