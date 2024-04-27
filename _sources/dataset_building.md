# La création du jeu de données de référence

Les technologies du traitement automatique du langage (TAL) sont des algorithmes d'apprentissage machine, ou *machine learning* en anglais. Elles ont besoin d'être *entraîner* à réaliser une tâche. Plus exactement, un modèle et son algorithme vont s'adapter à reproduire une tâche qui lui est présentée. Cette tâche doit lui être fournie via un certain nombre d'exemple. Ce nombre varie selon la complexité de la tâche. Plus la tâche est complexe plus le nombre d'exemple à fournir est important.

Dans le cadre du projet *Perspectiva*, la tâche est complexe. L'objectif est d'extraire les idées principales d'une contribution et de les évaluer. Ce tâche n'est pas toujours évidente pour un humain qui excelle dans ce domaine. En effet, selon la syntaxe, le vocabulaire, la grammaire, la ponctuation, le ton ou le style, il est possible de faire des erreurs d'interprétation. De fait, le modèle a besoin d'un grand nombre de données pour assurer sa capacité d'analyse. Dans le cas du projet de *Perspectiva*, le plus d'exemple possible est le mieux.

```{note}
Si vous êtes novice, David LOUAPRE a réalisé deux vidéos d'instruction qui résument très bien les principes. Vous pouvez y accéder via sa chaîne [Science étonnante](https://www.youtube.com/@ScienceEtonnante).

- [Le deep learning](https://www.youtube.com/watch?v=trWrEWfhTVg) : qu'est-ce que c'est ?
- [Ce qui se cache derrière le fonctionnement de ChatGPT](https://www.youtube.com/watch?v=7ell8KEbhJo) : comment fonctionnent les modèles du type GPT tels que ChatGPT ?
```

## La méthodologie : créer les données d'entrée et de sortie pour entraîner et évaluer.

Afin que la machine apprenne, il lui faut des exemples de ce qu'elle doit faire. Il faut donc que les praticiens, les ingénieurs et les développeurs de la solution soient au clair et s'accordent sur ce qu'ils souhaitent. Cette étape de réflexion est primordiale et peut être révisée au cours du projet. Mais il sera d'autant plus compliqué de créer une solution concrète qu'il y a de changement dans la définition de ce que la machine doit faire.

Un jeu de données ne sert pas uniquement à entraîner des modèles, mais aussi à évaluer ces derniers. En effet, il permet d'évaluer les performances des modèles dans un environnement le plus proche de la réalité, à savoir traiter des données qu'il n'a jamais vues. Il faut donc aussi que les parties prenantes du développement se mettent au clair avec une méthode d'évaluation.

## Le format de données basé sur JSON

Le choix actuel de *Perspectiva* est de réaliser à partir d'une contribution une extraction des idées principales. Cette extraction est réalisée sous le format JSON afin de pouvoir être facilement manipulable par les outils numériques.

```{note}
Le JSON est un format de données qui facilite la structuration et le transfert de ces données. Pour plus d'information : [Manipuler des données JSON - Mozilla](https://developer.mozilla.org/fr/docs/Learn/JavaScript/Objects/JSON)
```

Voici un exemple de document original, et son traitement au format JSON : 

Format original :
> Repartir les richesses. suppression de la taxe d habitation pour tous les français et de la csg reindexé les retraites . Ne plus diviser les français (les patrons ont besoin des plus modestes et vice versa. Les patrons ne réussisses que si les plus modestes sont aussi avec eux) . Les français ne veulent plus de l assistanat mais veulent vivre dignement de leur salaire

Format JSON :

``` json
{
    "content": [
    "Répartir les richesses de manière plus équitable",
    "Supprimer la taxe d'habitation pour tous les Français",
    "Supprimer la CSG",
    "Réindexer les retraites",
    "Unir les Français au lieu de les diviser",
    "Les patrons ont besoin des plus modestes et vice-versa",
    "Les Français veulent vivre dignement de leur salaire"
    ]
}
```

## Déterminer comment enrichir la données pour faciliter son traitement

La création de synthèse et de restitution nécessite plusieurs étape. Ces étapes sont majoritairement réalisées à la main par une ou plusieurs personnes :
1. récolter l'ensemble des contributions,
2. extraire les idées, les opinions, les propositions, les questions, etc...
1. créer une grille d'analyse et de catégories relatives aux thématiques partagées,
1. annoter et catégoriser les éléments extraits,
1. réaliser un ensemble de traitement statistique sur ces éléments,
1. réaliser une synthèse en vue de la restitution.

L'approche de *Perspectiva* est la suivante :
1. extraire les idées principales des contributions,
1. déterminer les thématiques et les regroupements possibles en analysant l'ensemble des idées partagées par les contributions,
1. venir annoter et catégoriser les idées selon les regroupements,
1. déterminer les poids relatifs entre les idées et leur relation.


Pour champs d'annotation actuelle géré par *Perspectiva* sont :
* `idea`: l'idée extraite,
* `semantic-cat`: la catégorie de l'idée au niveau sémantique, par exemple si elle est une proposition, une opinion ou une question.
* `semantic-neg`: si l'idée est positive, négative ou neutre du point du vue sémantique, du sens.
* `syntactic-neg` : si la syntaxe présente une négation (ne, ne ... pas, ne ... plus).
* `intensity` : l'intensité du propos ou du sens.
* `targets` : les sujets et objets de l'idée.

A cette première analyse, peut venir s'ajouter d'autres dimensions selon les besoins du praticien.

Exemple d'extraction partielle au format JSON :
``` json
{
  "extract": true,
  "content": [
    {
      "idea": "Répartir les richesses de manière plus équitable",
      "semantic-cat": "proposition",
      "semantic-neg": "neutre",
      "syntactic-neg": "positive",
      "intensity": "modéré",
      "targets": ["richesses", "équité"],
    },
    {
      "idea": "Supprimer la taxe d'habitation pour tous les Français",
      "category": "proposition",
      "semantic-neg": "négatif",
      "syntactic-neg": "positive",
      "intensity": "fort",
      "targets": ["taxe d'habitation", "Français"],
    }
  ]
}
```
