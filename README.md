# Cahier des charges du simulateur de prix d'adhésions à l'ATD16

## Problème actuel

Il est difficile, voire même impossible, actuellement de simuler certaines adhésions pour des structures déjà adhérentes, de plus il est impossible de simuler le prix d'une nouvelle adhésion pour une structure pas encore adhérente, c'est à dire une structure pas enregistrée en base de données.

### Projet

Le but de ce projet serait de développer un simulateur qui calculerait le tarif d'une, ou plusieurs, nouvelle(s) adhésion(s) en rentrant les informations nécessaires au calcul. Ce qui permettra aux agents de calculer le prix d'une adhésion pour une structure, c'est une fonctionnalité qui améliorerait la visibilité sur les prix des adhésions pour les agents.

### Contexte

Ce simulateur devra s'intégrer à l'application déjà existante : **Numérobis**, il sera accessible sur le site de cette application et utilisera la même API, le simulateur sera donc utilisé par les membres de l'atd16.

### Expression fonctionnelle du besoin

| Type Fonction | Fonction                                                  | Critères d'appréciation                         | Coefficient |
| :-----------: | --------------------------------------------------------- | ----------------------------------------------- | :---------: |
|      FP       | Calculer le prix final en fonction des adhésions simulées | Renvoie le prix correct en un temps raisonnable |      5      |
|      FC1      | Intégré sur le site interne                               | Lien facilement trouvable sur le site           |      5      |
|      FC2      | Utilisé l'API **Numérobis** déjà existante                | La/les nouvelles routes ne cassent pas l'API    |      5      |
|      FC3      | Formulaire intuitif et facilement completable             | Formulaire d'une taille raisonnable             |      3      |
|      FC4      | Pouvoir facilement supprimer une formule de la simulation | Facilement supprimable sans supprimé les autres |      3      |
|      FC5      | Faire attention aux prix planchers/plafonds               | Aucun résultat de dépasse le plancher/plafond   |      5      |

### Solutions proposées

#### Nouvelle Page

Je souhaite faire une page entière dédiée au simulateur, page contentant le formulaire ainsi que le choix de politique à simuler, et aussi le résultat et tout autre notification sur le calcul comme le plafond/plancher et augmentation/réduction

##### Wireframe

![Wireframe](images/1.png)
![WireframeModale](images/2.png)

##### Intégration dans le site

Cette page devrait être accessible depuis la barre de navigation a gauche afin qu'elle soit accessible de puis n'importe où (et peut être aussi depuis la déjà présente simulation pour une structures déjà présente ?)

#### Back-End

Il faudra certainement ajouté une route POST dans l'api qui attendrai dans le body une structure ainsi que les politiques (ou en tout cas des infos pour créer une structure factice dans le back-end) et donc renvoyé le prix que coûterait toutes ces adhésions mais aussi le prix unitaire de chaque politique.

##### Formatage des données

```json
{
  "adhesion": {
    "structures": {
      "name": "test",
      "typesOfStructure": "/api/types_of_structure/4",
      "adhesionVariables": [
        {
          "value": 10,
          "years": "/api/years/2025",
          "priceUnit": "/api/price_units/2"
        },
        {
          "value": 10,
          "years": "/api/years/2022",
          "priceUnit": "/api/price_units/2"
        },
        {
          "value": 1500,
          "years": "/api/years/2024",
          "priceUnit": "/api/price_units/1"
        },
        {
          "value": 7,
          "years": "/api/years/2025",
          "priceUnit": "/api/price_units/6"
        },
        {
          "value": 2,
          "years": "/api/years/2025",
          "priceUnit": "/api/price_units/10"
        },
        {
          "value": 2,
          "years": "/api/years/2025",
          "priceUnit": "/api/price_units/11"
        }
      ]
    },
    "formules": ["/api/formules/1"]
  },

  "years": 2025,
  "adheringRgpd": true
}
```

Retour :

```json
{
  "simulatedPrice": 0.0,
  "formulesPrices": [0, 0, 0]
}
```
