# 📘 Dictionnaire de données

## Table : `Project_plans`

| Champ                     | Type   | Contrainte   | Description                                                                                                |
|---------------------------|--------|--------------|------------------------------------------------------------------------------------------------------------|
| `Project_ID`              | STRING |              | Identifiant unique pour chaque projet. Il y a plusieurs lignes pour chaque ID projet car plusieurs phases  |
| `Phase`                   | STRING |              | La phase du projet                                                                                         |
| `Start Date`              | DATE   |              | Date de départ d'un projet                                                                                 |
| `Planned_Duration`        | FLOAT  |              | Nombre de jours prévus par phase. Estimation renseignée par l'équipe en charge                             |
| `Planned_Cost`            | FLOAT  |              | Coûts prévus (en $) pour achever la phase de projet associée. Estimation renseignée par l'équipe en charge |

## Table : `Project type`

| Champ                     | Type   | Contrainte   | Description                                                                                                |
|---------------------------|--------|--------------|------------------------------------------------------------------------------------------------------------|
| `Project_ID`              | STRING | Clé Primaire | Identifiant unique pour chaque projet.                                                                     |
| `Project Type`            | STRING |              | Type de projet : IT ou Marketing                                                                           |

## Table : `Actual_Costs`

| Champ                     | Type   | Contrainte   | Description                                                                                               |
|---------------------------|--------|--------------|-----------------------------------------------------------------------------------------------------------|
| `Project_ID`              | STRING |              | Identifiant unique pour chaque projet. Il y a plusieurs lignes pour chaque ID projet car plusieurs phases |
| `Phase`                   | STRING |              | La phase du projet                                                                                        |
| `Actual_Cost              | FLOAT  |              | Coût réel (en $) constaté de la phase du projet. Donnée resneignéee à la fin de la phase                  |

## Table : `Actual_Duration`

| Champ                     | Type   | Contrainte   | Description                                                                                               |
|---------------------------|--------|--------------|-----------------------------------------------------------------------------------------------------------|
| `Project_ID`              | STRING |              | Identifiant unique pour chaque projet. Il y a plusieurs lignes pour chaque ID projet car plusieurs phases |
| `Phase`                   | STRING |              | La phase du projet                                                                                        |
| `Actual_Duration`         | FLOAT  |              | Nombre de jours nécessaires pour achever la phase du projet. Constaté à la fin de chaque phase            |

## Table : `Deliverables_status`

| Champ                     | Type   | Contrainte   | Description                                                                                               |
|---------------------------|--------|--------------|-----------------------------------------------------------------------------------------------------------|
| `Project_ID`              | STRING |              | Identifiant unique pour chaque projet. Il y a plusieurs lignes pour chaque ID projet car plusieurs phases |
| `Phase`                   | STRING |              | La phase du projet                                                                                        |
| `Var_Deliverables`        | FLOAT  |              | Différence (en %) entre le nombre cible de livrables et le nombre réel livré pour chaque phase de projet. Donnée renseignée à des fins de suivi de performance. Elle permet de tracer la quantité d'éléments qui auraient dûs être livrés mais qui ne l'ont pas été       |

## Table : `Projects_Locations`

| Champ                     | Type   | Contrainte   | Description                                                                                                |
|---------------------------|--------|--------------|------------------------------------------------------------------------------------------------------------|
| `Project_ID`              | STRING | Clé Primaire | Identifiant unique pour chaque projet.                                                                     |
| `Country`                 | STRING |              | Pays dans lequel le projet est en cours de réalisation                                                     |

## Table : `Country_Profiles`

| Champ                     | Type   | Contrainte   | Description                                                                                                |
|---------------------------|--------|--------------|------------------------------------------------------------------------------------------------------------|
| `Country`                 | STRING | Clé Primaire | Pays dans lequel le projet est en cours de réalisation                                                     |
| `Region`                  | STRING |        | Région du pays (Asie Pacifique, Wester Europe, Central/Eastern Europe-Middle East-Africa, North & Latin America) | 
| `Type`                    | STRING |              | Classification du pays en fonction du type de partenariat (affilié ou distributeur)                        |

## Architecture des données 

![Schéma des données](../images/schema_donnees.png)

