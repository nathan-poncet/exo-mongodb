Voici la base de donnees qui permet d'effectuer la serie d'exercices :

```
db.salles.insertMany([
   {
       "_id": 1,
       "nom": "AJMI Jazz Club",
       "adresse": {
           "numero": 4,
           "voie": "Rue des Escaliers Sainte-Anne",
           "codePostal": "84000",
           "ville": "Avignon",
           "localisation": {
               "type": "Point",
               "coordinates": [43.951616, 4.808657]
           }
       },
       "styles": ["jazz", "soul", "funk", "blues"],
       "avis": [{
               "date": new Date('2019-11-01'),
               "note": NumberInt(8)
           },
           {
               "date": new Date('2019-11-30'),
               "note": NumberInt(9)
           }
       ],
       "capacite": NumberInt(300),
       "smac": true
   }, {
       "_id": 2,
       "nom": "Paloma",
       "adresse": {
           "numero": 250,
           "voie": "Chemin de l'Aérodrome",
           "codePostal": "30000",
           "ville": "Nîmes",
           "localisation": {
               "type": "Point",
               "coordinates": [43.856430, 4.405415]
           }
       },
       "avis": [{
               "date": new Date('2019-07-06'),
               "note": NumberInt(10)
           }
       ],
       "capacite": NumberInt(4000),
       "smac": true
   },
    {
       "_id": 3,
       "nom": "Sonograf",
       "adresse": {
           "voie": "D901",
           "codePostal": "84250",
           "ville": "Le Thor",
           "localisation": {
               "type": "Point",
               "coordinates": [43.923005, 5.020077]
           }
       },
       "capacite": NumberInt(200),
       "styles": ["blues", "rock"]
   }
])
```

Exercice 1

Affichez l’identifiant et le nom des salles qui sont des SMAC.

```
db.salles.find({ smac: true }, { _id: 1, nom: 1})
```

Exercice 2

Affichez le nom des salles qui possèdent une capacité d’accueil strictement supérieure à 1000 places.

```
db.salles.find({capacite: {$gt: 1000}}, { _id: 0, nom: 1 })
```

Exercice 3

Affichez l’identifiant des salles pour lesquelles le champ adresse ne comporte pas de numéro.

```
db.salles.find({ "adresse.numero": { $exists: false } }, { _id: 1 })
```

Exercice 4

Affichez l’identifiant puis le nom des salles qui ont exactement un avis.

```
db.salles.find({ avis: { $size: 1 } }, { _id: 1, nom: 1 })
```

Exercice 5

Affichez tous les styles musicaux des salles qui programment notamment du blues.

```
db.salles.find({ styles: "blues" }, { _id: 0, styles: 1 })
```

Exercice 6

Affichez tous les styles musicaux des salles qui ont le style « blues » en première position dans leur tableau styles.

```
db.salles.find({ $expr: { $eq: [{ $arrayElemAt: ["$styles", 0] }, "blues"] }}, { _id: 0, styles: 1 })
```

Exercice 7

Affichez la ville des salles dont le code postal commence par 84 et qui ont une capacité strictement inférieure à 500 places (pensez à utiliser une expression régulière).

```
db.salles.find({ "adresse.codePostal": /^84/, capacite: { $lt: 500 } }, { _id: 0, "adresse.ville": 1 })
```

Exercice 8

Affichez l’identifiant pour les salles dont l’identifiant est pair ou le champ avis est absent.

```
db.salles.find({ $or: [{ _id: { $mod: [2, 0] } }, { avis: { $exists: false } }] }, { _id: 1 })
```

Exercice 9

Affichez le nom des salles dont au moins un des avis comporte une note comprise entre 8 et 10 (tous deux inclus).

```
db.salles.find({ avis: { $elemMatch: { note: { $gte: 8, $lte: 10 } } } }, { _id: 0, nom: 1 })
```

Exercice 10

Affichez le nom des salles dont au moins un des avis comporte une date postérieure au 15/11/2019 (pensez à utiliser le type JavaScript Date).

```
db.salles.find({ avis: { $elemMatch: { date: { $gt: new Date('2019-11-15') } } } }, { _id: 0, nom: 1 })
```

Exercice 11

Affichez le nom ainsi que la capacité des salles dont le produit de la valeur de l’identifiant par 100 est strictement supérieur à la capacité.

```
db.salles.find({ $expr: { $gt: [{ $multiply: ["$_id", 100] }, "$capacite"] } }, { _id: 0, nom: 1, capacite: 1 })
```

Exercice 12

Affichez le nom des salles de type SMAC programmant plus de deux styles de musiques différents en utilisant l’opérateur $where qui permet de faire usage de JavaScript.

```
db.salles.find({ smac: true, $where: function() { return new Set(this.styles).size > 2; }}, { _id: 1, nom: 1})
```

Exercice 13

Affichez les différents codes postaux présents dans les documents de la collection salles.

```
db.salles.distinct("adresse.codePostal")
```

Exercice 14

Mettez à jour tous les documents de la collection salles en rajoutant 100 personnes à leur capacité actuelle.

```
db.salles.updateMany({}, { $inc: {capacite: 100} })
```

Exercice 15

Ajoutez le style « jazz » à toutes les salles qui n’en programment pas.

```
db.salles.updateMany({ styles: {$ne: "jazz"} }, { $push: { styles: "jazz" } })
```

Exercice 16

Retirez le style «funk» à toutes les salles dont l’identifiant n’est égal ni à 2, ni à 3.

```
db.salles.updateMany({ _id: { $nin: [2, 3] } }, { $pull: { styles: "funk" } })
```

Exercice 17

Ajoutez un tableau composé des styles «techno» et « reggae » à la salle dont l’identifiant est 3.

```
db.salles.updateOne({ _id: 3 }, { $push: { styles: { $each: ["techno", "reggae"] } } })
```

Exercice 18

Pour les salles dont le nom commence par la lettre P (majuscule ou minuscule), augmentez la capacité de 150 places et rajoutez un champ de type tableau nommé contact dans lequel se trouvera un document comportant un champ nommé telephone dont la valeur sera « 04 11 94 00 10 ».

```
db.salles.updateMany({ nom: /^p/i }, { $inc: { capacite: 150 }, $push: { contact: { telephone: "04 11 94 00 10" } } })
```

Exercice 19

Pour les salles dont le nom commence par une voyelle (peu importe la casse, là aussi), rajoutez dans le tableau avis un document composé du champ date valant la date courante et du champ note valant 10 (double ou entier). L’expression régulière pour chercher une chaîne de caractères débutant par une voyelle suivie de n’importe quoi d’autre est [^aeiou]+$.

```
db.salles.updateMany({ nom: /^[aeiou]/i }, { $push: { avis: { date: new Date(), note: 10 } } })
```

Exercice 20

En mode upsert, vous mettrez à jour tous les documents dont le nom commence par un z ou un Z en leur affectant comme nom « Pub Z », comme valeur du champ capacite 50 personnes (type entier et non décimal) et en positionnant le champ booléen smac à la valeur « false ».

```
db.salles.updateMany({ nom: /^[z]/i }, { $set: { nom: "Pub Z", capacite: 50, smac: false } }, { upsert: true })
```

Exercice 21

Affichez le décompte des documents pour lesquels le champ \_id est de type « objectId ».

```
db.salles.count({ _id: { $type: "objectId" } })
```

Exercice 22

Pour les documents dont le champ \_id n’est pas de type « objectId », affichez le nom de la salle ayant la plus grande capacité. Pour y parvenir, vous effectuerez un tri dans l’ordre qui convient tout en limitant le nombre de documents affichés pour ne retourner que celui qui comporte la capacité maximale.

```
db.salles.find({ _id: { $not: { $type: "objectId" } } }, { _id: 0, nom: 1, capacite: 1 }, { sort: { capacite: -1 }, limit: 1 })
```

Exercice 23

Remplacez, sur la base de la valeur de son champ \_id, le document créé à l’exercice 20 par un document contenant seulement le nom préexistant et la capacité, que vous monterez à 60 personnes.

```
db.salles.replaceOne({ _id: ObjectId("6495c3625d9b8adab396a30f") }, { nom: "Pub Z", capacite: 60 })
```

Exercice 24

Effectuez la suppression d’un seul document avec les critères suivants : le champ \_id est de type « objectId » et la capacité de la salle est inférieure ou égale à 60 personnes.

```
db.salles.deleteOne({ _id: { $type: "objectId" }, capacite: { $lte: 60 } })
```

Exercice 25

À l’aide de la méthode permettant de trouver un seul document et de le mettre à jour en même temps, réduisez de 15 personnes la capacité de la salle située à Nîmes.

```
db.salles.findOneAndUpdate({ "adresse.ville": "Nîmes" }, { $inc: { capacite: -15 } })
```
