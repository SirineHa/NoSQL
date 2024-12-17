# TP4 : MapReduce avec CouchDB
## Système de gestion de base de données documentaires : CouchDB

### Introduction
Apache CouchDB est un système de gestion de base de données documentaires open-source, simple à installer et à utiliser. Il est particulièrement adapté pour les applications utilisant des API REST et facilite la compréhension des concepts de bases de données NoSQL.

CouchDB expose des fonctionnalités sous forme d'API REST :
  - **GET** : Récupérer la représentation d'une ressource.
  - **PUT** : Créer ou mettre à jour une ressource.
  - **POST** : Envoyer des données à une ressource (ajout ou exécution d'un service).
  - **DELETE** : Supprimer une ressource.

### Installation
CouchDB peut être installé de deux manières :

1. **Installation de Apache CouchDB** via le site officiel : [CouchDB Apache](https://couchdb.apache.org/).
2. **Installation avec Docker** :
   ```bash
   docker run -d --name couchdbdemo -e COUCHDB_USER=youcef -e COUCHDB_PASSWORD=samir -p 5984:5984 couchdb
   ```

CouchDB propose un client graphique accessible via : `http://localhost:5984/_utils`.

---

### Manipulation de CouchDB

#### Lancer un client
Pour tester si CouchDB fonctionne correctement :
```bash
curl -X GET http://youcef:samir@localhost:5984
```

#### Créer une nouvelle base de données (films)
```bash
curl -X PUT http://youcef:samir@localhost:5984/films
```

#### Afficher la représentation de la base **films**
```bash
curl -X GET http://youcef:samir@localhost:5984/films
```

#### Insérer un nouveau document dans la base **films**
Comme MongoDB, CouchDB n'impose pas de schéma. Toutefois, contrairement à Cassandra, il attribue un identifiant automatique si aucun n'est spécifié.

```bash
curl -X PUT http://youcef:samir@localhost:5984/films/doc -d '{"cle":"valeur"}'
```

**Remarques :**
- Il n'est pas recommandé d'utiliser des identifiants fixes comme **doc**.
- Si un document sans identifiant est inséré, CouchDB lui attribuera un identifiant unique.
- Une tentative d'insertion d'un document avec un identifiant existant (comme **doc**) échouera.

#### Insérer un document à partir d'un fichier **movie:10098.json**
```bash
curl -X POST http://youcef:samir@localhost:5984/films -d @movie:10098.json -H "Content-Type: application/json"
```

#### Insérer une collection de documents à partir d'un fichier JSON
```bash
curl -X POST http://youcef:samir@localhost:5984/films/_bulk_docs -d @films_couchdb.json -H "Content-Type: application/json"
```

#### Récupérer un document par son identifiant (**_id**)
```bash
curl -X GET http://youcef:samir@localhost:5984/films/movie:10098
```

---

### Manipulation : MapReduce

CouchDB utilise MapReduce pour exécuter des requêtes complexes sur les documents de la base. Le processus se déroule en deux étapes :

1. **Map** : La fonction définit une vue qui extrait les informations souhaitées.
2. **Reduce** : La fonction agrège ou combine les résultats obtenus par la fonction Map.

#### Exemple de fonction Map
Voici un exemple d’une fonction Map qui extrait tous les titres de films :

```javascript
function (doc) {
  if (doc.title) {
    emit(doc._id, doc.title);
  }
}
```

#### Exemple de fonction Reduce
Une fonction Reduce pour compter le nombre de films dans la base :

```javascript
function (keys, values, rereduce) {
  return values.length;
}
```

---

### Conclusion
CouchDB est un excellent outil pour comprendre les bases de données documentaires et les architectures REST. Avec son système MapReduce, il permet de réaliser des traitements de données efficaces tout en étant simple à configurer.

---

### Références
- [Documentation officielle de CouchDB](https://docs.couchdb.org/en/stable/)
- [Tutoriel Docker pour CouchDB](https://hub.docker.com/_/couchdb/)
