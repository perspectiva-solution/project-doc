# La création du jeu de données de référence

Les technologies du traitement automatique du langage (TAL) sont des algorithmes d'apprentissage machine, ou *machine learning* en anglais. Elles ont besoin d'être *entraînées* à réaliser une tâche. Plus exactement, ces technologies s'appuient sur des modèles qui stockent l'information et des algorithmes de paramétrage qui vont s'adapter à reproduire une tâche qui leur est présentée. Cette tâche doit leur être fournie via un certain nombre d'exemples variés. Ce nombre varie selon la complexité de la tâche. Plus la tâche est complexe plus le nombre d'exemple à fournir est important.

Dans le cadre du projet *Perspectiva*, la tâche est complexe. L'objectif est d'extraire les idées principales d'une contribution et de les évaluer. Cette tâche n'est pas toujours évidente même pour un humain qui excelle dans ce domaine. En effet, selon la syntaxe, le vocabulaire, la grammaire, la ponctuation, le ton ou le style, il est possible de faire des erreurs d'interprétation. De fait, le modèle a besoin d'un modèle pouvant stocker des règles complexes et d'un grand nombre de données pour se paramétrer correctement. Dans le cas du projet de *Perspectiva*, le plus d'exemple possible est le mieux.

```{note}
Si vous êtes novice, David LOUAPRE a réalisé deux vidéos d'instruction qui résument très bien les principes. Vous pouvez y accéder via sa chaîne [Science étonnante](https://www.youtube.com/@ScienceEtonnante).

- [Le deep learning](https://www.youtube.com/watch?v=trWrEWfhTVg) : qu'est-ce que c'est ?
- [Ce qui se cache derrière le fonctionnement de ChatGPT](https://www.youtube.com/watch?v=7ell8KEbhJo) : comment fonctionnent les modèles du type GPT tels que ChatGPT ?
```

## La méthodologie : créer les données d'entrée et de sortie pour entraîner et évaluer.

Afin que la machine apprenne, il lui faut des exemples de ce qu'elle doit faire. Il faut donc que les praticiens, les ingénieurs et les développeurs de la solution soient au clair et s'accordent sur ce qu'ils souhaitent que la machine fasse. Cette étape de réflexion est primordiale et peut être révisée au cours du projet. Mais il sera d'autant plus compliqué de créer une solution concrète qu'il y a de changement dans la définition de ce que la machine doit faire.

Un jeu de données ne sert pas uniquement à entraîner des modèles, mais aussi à évaluer ces derniers. En effet, il permet d'évaluer les performances des modèles dans un environnement le plus proche de la réalité, à savoir traiter des données qu'il n'a jamais vues. Il faut donc aussi que les parties prenantes du développement se mettent au clair avec une méthode d'évaluation et ce qu'ils jugent comme étant un bon fonctionnement. Si l'on peut aisément s'imaginer comment évaluer un modèle qui manipule des chiffres, par exemple en calculant la différence entre le résultat du modèle avec la valeur attendue, pour une phrase ou un texte, l'évaluation est plus délicate. Cet aspect sera développé dans un avenir proche.

Concernant les modèles du langage, l'état de l'art de l'apprentissage comprend trois étapes : le pré-entraînement, l'apprentissage de la tâche et l'alignement des préférences. Ces trois étapes nécessitent des données différentes :
* le **pré-entraînement** : grandes quantités de texte dans une ou plusieurs langues pour que le modèle apprennent à écrire les mots et les phrases,
* l'**apprentissage de la tâche** : jeu de données qui représentent les données d'entrée et de sortie de la tâche à réaliser.
* l'**alignement des préférences** : ensemble de résultat dont un des résultats est meilleure qu'un autre. L'idée est de présenter à la machine à minima deux sorties probables dont une est nettement meilleure qu'une autre selon l'évaluation humaine.

Dans la pratique, les praticiens utilisent des modèles déjà pré-entraînés par les *majors* du secteur, comme ChatGPT pour OpenAI, Mistral / Mixtral pour Mistral AI, Phi pour Microsoft ou Gemini pour Google. Ensuite, ils constituent un jeu de donnée comprenant à la fois la tâche à la réaliser et un jeu de donnée similaire mais légèrement dégradé qui fera l'objet de moins bon exemple pour la tâche d'alignement.

```{important}
**La création d'un jeu de données de référence est une étape cruciale qui est difficilement automatisable**. Elle est chronophage dans ce qu'elle nécessite d'être précautionneux. Si la machine apprend de mauvais exemples, elle réalisera de mauvaises productions. Si l'on peut réaliser certaine étape du traitement de façon automatique à l'aide de solution comme ChatGPT d'OpenAI, il est nécessaire de vérifier chacune des productions et de corriger à la main si nécessaire.
```

:::{seealso}
Il existe plusieurs algorithmes dont certains ne réalisent qu'une seule étape, comme l'algorithme *Supervised Fine-Tuning (SFT)* qui permet l'apprentissage d'une tâche, ou le *Direct Prefrence Optimization (DPO)* qui réalise l'alignement des préférences. Ces algorithmes sont l'objet d'une recherche actif. Les algorithmes et les pratiques ne cessent de s'améliorer. Pour plus d'information, et une revue à jour le avril 2024, lire les [huit billets dédiés sur le sujet d'Argilla (en anglais)](https://argilla.io/blog/mantisnlp-rlhf-part-1).
Dans tous les cas, si l’algorithme change, les données et leurs conditions sont les mêmes : en quantité et de qualité.
:::

## La création concrète du jeu de données

Comme vue précédemment, la constitution d'un jeu de données de référence s'appuie sur un travail préliminaire et des choix. Parmi ces choix :
* le format d'entrée et de sortie des données,
* la tâche de travail à réaliser par la machine.

### Le format : structure basée sur JSON

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

### La tâche : extraire et enrichir les contributions pour faciliter leurs analyses

La création de synthèse et de restitution nécessite plusieurs étape. Ces étapes sont majoritairement réalisées à la main par une ou plusieurs personnes :
1. récolter l'ensemble des contributions,
1. définir une grille d'analyse et de catégories relatives aux thématiques partagées ou à analyser,
1. lire les contributions et les annoter selon la grille d'analyse,
1. réaliser un ensemble de traitement statistique sur ces annotations,
1. réaliser une synthèse en vue de la restitution.

L'approche de *Perspectiva* est la suivante :
1. extraire les idées principales des contributions,
1. déterminer les thématiques et les regroupements possibles en analysant l'ensemble des idées partagées par les contributions,
1. venir annoter et catégoriser les idées selon les regroupements,
1. déterminer les poids relatifs entre les idées et leur relation,
1. le tout de façon semi-automatique : proposition automatique corrigeable par un praticien.

Les champs d'annotation actuels gérés par *Perspectiva* sont :
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
      "semantic-cat": "proposition",
      "semantic-neg": "négatif",
      "syntactic-neg": "positive",
      "intensity": "fort",
      "targets": ["taxe d'habitation", "Français"],
    }
  ]
}
```

```{important}
La taxonomie actuelle de *Perspectiva* n'est pas figée. Pour contribuer à son amélioration, n'hésitez pas à contacter l'équipe en charge du projet. Vous pouvez regarder et participer au ticket dédié à ce sujet sur Github : [ticket Github](https://github.com/perspectiva-solution/project-doc/issues/1)
```

### L'alignement de préférence à l'aide d'exemples dégradés

L'étape d'alignement de préférence permet d'obtenir de meilleures résultats. Il permet à la machine de déterminer ce qui est vraiment attendu et de produire le meilleur résultat dans les résultats les plus probables. D'ailleurs, il n'est pas rare que l'on apprenne, nous humain, aussi de cette façon.

Une idée simple et efficace pour générer un jeu de donnée de préférence est de prendre le jeu de données de la tâche à réaliser et de le dégrader. Par exemple, il est possible de détériorer une extraction, d'introduire une erreur dans la ou les catégories d'enrichissement. Voici un exemple concret : 

`````{tab-set}
````{tab-item} Correct
```json
{
  "idea": "Répartir les richesses de manière plus équitable",
  "semantic-cat": "proposition",
  "semantic-neg": "neutre",
  "syntactic-neg": "positive",
  "intensity": "modéré",
  "targets": ["richesses", "équité"],
}
```
````

````{tab-item} Dégradé
```json
{
  "idea": "Répartir les richesses de manière plus équitable",
  "semantic-cat": "opinion",
  "semantic-neg": "neutre",
  "syntactic-neg": "positive",
  "intensity": "modéré",
  "targets": ["richesses", "égalité"],
}
```
````
`````

Dans la version dégradée, la catégorie sémantique `opinion` est mauvaise, aussi le terme `égalité` est une mauvaise reformulation d'équitable, qui devrait être `équité`. Ces types d'erreur semblent possibles. Elles impacteront nécessairement le traitement et l'analyse si elles ne sont pas corrigées. L'étape de préférence vise à sensibiliser le modèle à ce type d'erreur et à les prévenir.

Ce travail peut être réalisé de différente façon, plus ou moins manuel. Il est possible d'utiliser un outil de traitement de langage qui génère des réponses proches et similaires, et de sélectionner la meilleure et une moins bonne réponse. Ce travail peut être aussi fait à la main. Il peut être pertinent de le faire réaliser par la machine dans le sens que les variabilités et les erreurs qui seront introduites par cette dernière seront fidèles au fonctionnement de la machine. Introduire ces erreurs dans l’entraînement est le meilleur moyen de les prévenir. En effet, quel serait l'effet de sensibiliser une machine à une erreur qu'elle ne ferait pas ? Cependant, cette approche est robuste lorsque l'on utilise un modèle en particulier. Or, du fait de l'évolution permanente des performances des modèles, il est très probable d'utiliser le jeu de données pour entraîner des modèles différents.

