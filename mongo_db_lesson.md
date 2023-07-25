# Mongo

## Base de données NoSQL :
MongoDB est une base de données orientée document, qui stocke les données sous une forme semi-structurée (BSON). Par exemple, au lieu de stocker les informations d'un utilisateur dans une table (comme SQL), MongoDB stockerait les informations de l'utilisateur dans un document BSON ressemblant à du JSON.

```json
{
  "nom": "Dupont",
  "prenom": "Jean",
  "age": 25,
  "adresse": {
    "rue": "123 rue de Paris",
    "ville": "Paris",
    "pays": "France"
  }
}
```

## Connaissance de la syntaxe MongoDB :

MongoDB utilise une syntaxe basée sur des documents JSON pour effectuer des opérations CRUD (Création, Lecture, Mise à jour et Suppression) sur les documents de la base de données.

Par exemple, pour insérer un nouveau document dans une collection appelée "users", vous pouvez utiliser la commande suivante :

```javascript
db.users.insert({
  name: "John",
  email: "john@example.com",
});
```

## Opérations de recherche :

Les méthodes comme find() et findOne() sont utilisées pour récupérer des documents de la base de données. Ces méthodes prennent en paramètre des critères de recherche qui sont définis dans un objet.

Par exemple, pour trouver un utilisateur avec le nom 'John', on peut utiliser la commande suivante :

```javascript
db.users.find({ name: "John" });
```

## Filtre et projection :

MongoDB permet de filtrer les documents à récupérer en utilisant divers opérateurs de comparaison comme $gt, $lt, $eq, $in, $ne, etc. On peut également utiliser la projection pour déterminer quels champs doivent être retournés par une opération de recherche.

Pour trouver tous les utilisateurs dont le nom commence par 'J' et afficher uniquement leurs emails, on peut utiliser la commande suivante :

```javascript
db.users.find({ name: { $regex: /^J/ } }, { email: 1 });
```

## Mise à jour des documents :

Les méthodes comme updateOne(), updateMany() et replaceOne() sont utilisées pour modifier les documents existants. Les modifications sont définies en utilisant des opérateurs de mise à jour comme $set, $unset, $inc, $push, $pop, etc.

Pour mettre à jour l'email de l'utilisateur 'John', on peut utiliser la commande suivante :

```javascript
db.users.updateOne(
  { name: "John" },
  { $set: { email: "john.new@example.com" } }
);
```

## Opérations d'agrégation :

MongoDB permet d'exécuter des opérations d'agrégation sur les documents de la base de données, comme le regroupement ($group), le comptage ($count), la somme ($sum), la moyenne ($avg), etc.

Si l'on souhait compter le nombre total d'utilisateurs dans une collection :

```javascript
db.users.count();
```

## Géolocalisation :

MongoDB fournit des opérateurs spécifiques pour les données géospatiales, comme $geoWithin, $nearSphere, etc. Cela permet de réaliser des opérations de recherche basées sur la localisation.

Ex:

```javascript
db.users.find({
  location: {
    $near: {
      $geometry: { type: "Point", coordinates: [-122, 37] },
      $maxDistance: 100000,
    },
  },
});
```

## Création d'index :

Pour optimiser les performances de la recherche, MongoDB permet de créer des index sur les champs d'un document. L'opérateur $index est utilisé à cet effet.

Ex:

```javascript
db.users.createIndex({ email: 1 });
```

## Expressions régulières :

Pour effectuer des recherches plus flexibles, MongoDB permet l'utilisation d'expressions régulières.

Ex:

```javascript
db.users.find({ name: { $regex: /^J/ } });
```

## Opérations sur les tableaux :

MongoDB permet de réaliser des opérations spécifiques sur les champs de type tableau, comme vérifier la taille d'un tableau, rechercher un élément spécifique dans un tableau, ajouter ou supprimer des éléments d'un tableau, etc.

Ex:

```javascript
// Recherche les utilisateurs ayant 3 tags
db.users.find({ tags: { $size: 3 } });

// Modifier l'utilisateur John en ajoutant un nouveau tag
db.users.updateOne({ name: "John" }, { $push: { tags: "reading" } });
```

## Upsert :

C'est une opération de mise à jour qui crée un nouveau document si aucun document ne correspond aux critères de recherche.

Ex:

```javascript
db.users.updateOne(
  { name: "John" },
  { $set: { email: "john.new@example.com" } },
  { upsert: true }
);
```

Nathan Poncet
