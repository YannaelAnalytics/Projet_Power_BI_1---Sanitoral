# 🛠️ Méthodologie de préparation des données – Power Query & DAX

## 📂 Table Actual_Duration

### 1️⃣ Préparation dans Power Query

- 📌 **Promotion d’en-tête** → première ligne en en-tête.

- 🗑️ **Nettoyage** → suppression des lignes vides.

- 🔄 **Transformation de types :**

  - **Project_ID** → Nombre entier (meilleur affichage dans un segment de filtrage)

  - **Phase** → Texte

  - **Actual_Duration** → Durée

- 🔑 **Création d’une clé primaire :**

   - Duplication des colonnes Project_ID et Phase
  
   - Fusion pour créer Projet + Phase ID

- 🔗 **Relation** : liaison Actual_Duration ↔ Projects_plans via la clé Projet + Phase ID.

---

### 2️⃣ Colonnes calculées

<details>
<summary>📥 Récupération de la colonne "Planned_Duration" depuis la table "Projects_plans" </summary>

```dax
Planned_Duration = RELATED(Projects_plans[Planned_Duration])
```
</details>

<details>
<summary>📏 Calcul du taux de dépassement </summary>
  
```dax
Taux de dépassement durée = (Actual_Duration[Actual_Duration] - Actual_Duration[Planned_Duration]) / Actual_Duration[Planned_Duration]
```
</details>

<details>
<summary>🚦 Attribution du statut (OK / En Retard) </summary>
  
```dax
Statut durée par phase = IF(Actual_Duration[Taux de dépassement durée] >= 0.15, "En Retard", "OK")
```
</details>

---

### 3️⃣ Mesures pour les visualisations

<details>
<summary>📅 Durée prévue </summary>
  
```dax
Durée Prévue = SUM(Actual_Duration[Planned_Duration])
```
</details>

<details>
<summary> 📅 Durée réelle </summary>
  
```dax
Durée Réelle = SUM(Actual_Duration[Actual_Duration])
```
</details>

<details>
<summary>📊 Écart (jours)</summary>
  
```dax
Ecart Planned actual = [Durée Réelle] - [Durée Prévue]
```
</details>

<details>
<summary>⚠️ Alerte dépassement (+15%)</summary>
  
```dax
Alerte_Depassement_Durée = VAR DureePrevue = [Durée Prévue]
                           VAR DureeReelle = [Durée Réelle]
                           VAR Depassement = DureeReelle - DureePrevue
                           RETURN IF(Depassement >= DureePrevue * 0.15, "Retard de plus de 15%", "Durée Respectée")
```
</details>

---

### 4️⃣ Utilité dans le dashboard

Ces colonnes et mesures permettent :

- de calculer le retard et de retourner un statut pour chaque phase dans tous les projets.

- d'alimenter les graphiques de focus projet

- de classer les projets par taux de dépassement décroissant

- d’afficher automatiquement les alertes sur les projets en retard.

---

## Table `Actual_Costs` :

-	Promotion de la première ligne en en-tête.
  
-	Suppression des lignes vides

-	On crée ensuite la même clé primaire que dans la table précédente en fusionnant les 2 colonnes dupliquées en une colonne « Projet + Phase ID ».

-	On transforme ensuite le type de données en Texte pour « Projet + Phase ID » et « Phase », Nombre Entier pour « Project_ID » et Nombre Décimal pour « Actual_Cost ».




-	L’un des objectifs est d’alerter au-delà d’un dépassement de plus de 15%. J’ai donc créé une colonne qui calcule le taux de dépassement pour déterminer si chaque phase du projet dépasse la valeur seuil.

-	J’ai également créé une colonne conditionnelle qui attribue un statut en fonction du taux de dépassement : « OK » si en-dessous de 15% de dépassement et « Dépassé » au-delà.

-	Les 2 étapes précédentes servent à alimenter les graphiques de focus de projet en découpant par phase et alimente aussi les classements des projets par taux de dépassement décroissant (les mesures ne me permettent pas d’alimenter correctement les graphiques).

-	Cependant, j’ai utilisé des mesures pour créer mon alerte de coûts. Les mesures reprennent d’abord le [Budget prévu], puis le [Budget réel]. Enfin la mesure qui définit l’alerte stipule que si [Budget réel] – [Budget prévu] est supérieur ou égal à [Budget prévu] x 0,15, alors doit s’afficher « Budget dépassé de plus de 15% », sinon « Budget Respecté ».

-	Création de la mesure d’écart de coûts qui sert d’info-bulle aux graphiques : [Budget réel] – [Budget prévu].


---

## Table `Country_Profiles` :

-	Promotion de la première ligne en en-tête.

-	On transforme ensuite le type de données en Texte.

-	Suppression des lignes et colonnes vides.

---

## Table `Deliverables_status` :

-	Promotion de la première ligne en en-tête.

-	Suppression des lignes vides.

-	On crée ensuite la même clé primaire que dans la table précédente en fusionnant les 2 colonnes dupliquées en une colonne « Projet + Phase ID ».
  
-	On transforme ensuite le type de données en Texte pour « Projet + Phase ID » et « Phase », Nombre Entier pour « Project_ID » et Nombre Décimal pour « Var_Deliverables ».





-	J’ai utilisé une mesure pour créer mon alerte de taux de livrables manquants. La mesure calcule si la moyenne du taux de livrable manquants pour un projet est inférieure ou égale à -15%, alors doit s’afficher « Plus de 15% de livrables manquants », sinon « Projet Complété ».

---

## Table `Projects_Locations` :

-	Promotion de la première ligne en en-tête.

-	Suppression des lignes vides.

-	Fusion Externe Gauche sur la base du champ « Country » avec la Table « Country_Profiles » pour rassembler en une seule tables toutes les données géographiques, donc rajouter à notre tables « Projects_Locations » la « Région » et le « Type » de pays.

-	Transformation du type de données : « Project_ID » en Nombre Entier, puis « Country », « Region » et « Type » en Texte.

---	

## Table `Project type` :

-	Promotion de la première ligne en en-tête.

-	Suppression des lignes vides.

-	Transformation du type de données : « Project_ID » en Nombre Entier et « Project Type » en Texte.

---

## Table `Projects_plans` :

-	Promotion de la première ligne en en-tête.

-	Etant donné que la table ne comporte pas de clé primaire (le même Project_ID se retrouve autant de fois qu’il y’a de phases pour un projet), on duplique les colonnes « Project_ID » et « Phase ».

-	On crée ensuite une clé primaire en fusionnant ces 2 colonnes en une colonne « Projet + Phase ID ».

- Transformation du type de données : « Project_ID » en Nombre Entier, "Planned_Cost" en Nombre Décimal ainsi que "Projet Phase ID" et "Phase" en Texte. On transforme également "Start Date" en Date et "Planned_Duration" en Durée.

-	Suppression des lignes vides.

---

## Création de la table `PROJETS GLOBAL COUTS`

-	Je voulais que les directeurs aient accès aux KPIs de l’ensemble des projets et non au projet ou à la phase. J’ai donc décidé de créer cette nouvelle table dans laquelle l’ID Projet serait une clé primaire, sans les phases.

-	J’ai donc utilisé les données déjà entrées dans la table ‘Actual_Costs’, notamment la colonne ID Projet et ai calculé la moyenne du taux de dépassement des coûts par projet.

-	Enfin j’ai créé une colonne conditionnelle qui attribue un statut en fonction du taux de dépassement : « OK » si en-dessous de 15% de dépassement et « Budget dépassé » au-delà.

## Création de la table `PROJETS GLOBAL DUREE`

-	Dans la même idée que pour la table précédemment expliquée, j’ai fait de même pour la durée des projets.

-	J’ai donc utilisé les données déjà entrées dans la table ‘Actual_Duration’, notamment la colonne ID Projet et ai calculé la moyenne du taux de dépassement de durée par projet.

-	Enfin j’ai créé une colonne conditionnelle qui attribue un statut en fonction du taux de dépassement : « OK » si en-dessous de 15% de dépassement et « En Retard » au-delà.

# Architecture Finale







