Exercice 1

Écrivez le pipeline qui affichera dans un champ nommé ville le nom de celles abritant une salle de plus de 50 personnes ainsi qu’un booléen nommé grande qui sera positionné à la valeur « vrai » lorsque la salle dépasse une capacité de 1 000 personnes. Voici le squelette du code à utiliser dans le shell :

```
var pipeline = [
    {
        $match: {
            capacite: { $gt: 50 }
        }
    },
    {
        $project: {
            _id: 0,
            ville: "$adresse.ville",
            grande: {
                $cond: {
                    if: { $gt: ["$capacite", 1000] },
                    then: true,
                    else: false
                }
            }
        }
    }
];
```

Exercice 2

Écrivez le pipeline qui affichera dans un champ nommé apres_extension la capacité d’une salle augmentée de 100 places, dans un champ nommé avant_extension sa capacité originelle, ainsi que son nom.

```
var pipeline = [
    {
        $project: {
            _id: 0,
            nom: 1,
            avant_extension: "$capacite",
            apres_extension: { $add: ["$capacite", 100] }
        }
    }
];
```

Exercice 3

Écrivez le pipeline qui affichera, par numéro de département, la capacité totale des salles y résidant. Pour obtenir ce numéro, il vous faudra utiliser l’opérateur $substrBytes dont la syntaxe est la suivante :

```
var pipeline = [
    {
        $group: {
            _id: {
                $substrBytes: [ "$adresse.codePostal", 0, 2 ]
            },
            capacite_totale: {
                $sum: "$capacite"
            }
        }
    }
];
```

Exercice 4

Écrivez le pipeline qui affichera, pour chaque style musical, le nombre de salles le programmant. Ces styles seront classés par ordre alphabétique.

```
var pipeline = [
    {
        $unwind: "$styles"
    },
    {
        $group: {
            _id: "$styles",
            nombre: {
                $sum: 1
            }
        }
    },
    {
        $sort: {
            _id: 1
        }
    }
];
```

Exercice 5

À l’aide des buckets, comptez les salles en fonction de leur capacité :

celles de 100 à 500 places

celles de 500 à 5000 places

```
var pipeline = [
    {
        $bucket: {
            groupBy: "$capacite",
            boundaries: [100, 500, 5000],
            default: "autres",
            output: {
                _id: {
                    $cond: {
                        if: { $gt: ["$_id", 1000] },
                        then: true,
                        else: false
                    }
                }
                nombre: { $sum: 1 }
            }
        }
    }
];
```


Exercice 6

Écrivez le pipeline qui affichera le nom des salles ainsi qu’un tableau nommé avis_excellents qui contiendra uniquement les avis dont la note est de 10.

```
var pipeline = [
    {
        $project: {
            _id: 0,
            nom: 1,
            avis_excellents: {
                $filter: {
                    input: "$avis",
                    as: "avis",
                    cond: { $eq: ["$$avis.note", 10] }
                }
            }
        }
    }
];
```