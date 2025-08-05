# Préparation des données par table

I)	Actual Duration

-	Promotion de la première ligne en en-tête.

-	Renommer le titre de la colonne « Projet » en « Project_ID » pour plus de cohérence dans la liaison des tables.
-	Suppression des lignes vides.

-	Transformation du type de données : « Project_ID » en Nombre Entier, « Phase » en Texte et « Actual_Duration » en Durée.

-	Etant donné que la table ne comporte pas de clé primaire (le même Project_ID se retrouve autant de fois qu’il y’a de phases pour un projet), on duplique les colonnes « Project_ID » et « Phase ».

-	On crée ensuite une clé primaire en fusionnant ces 2 colonnes en une colonne « Projet + Phase ID ».

-	L’un des objectifs est d’alerter au-delà d’un dépassement de plus de 15%. J’ai donc créé une colonne qui calcule le taux de dépassement pour déterminer si chaque phase du projet dépasse la valeur seuil.

-	J’ai également créé une colonne conditionnelle qui attribue un statut en fonction du taux de dépassement : « OK » si en-dessous de 15% de dépassement et « En Retard » au-delà.

-	Les 2 étapes précédentes servent à alimenter les graphiques de focus de projet en découpant par phase et alimente aussi les classements des projets par taux de dépassement décroissant (les mesures ne me permettent pas d’alimenter correctement les graphiques).

-	Cependant, j’ai utilisé des mesures pour créer mon alerte de durée. Les mesures reprennent d’abord la [durée prévue], puis la [durée réelle]. Enfin la mesure qui définit l’alerte stipule que si [durée réelle] – [durée prévue] est supérieur ou égal à [durée prévue] x 0,15, alors doit s’afficher « Retard de plus de 15% », sinon « Durée Respectée ».

-	Création de la mesure d’écarts de durée qui sert d’info-bulle aux graphiques : [durée réelle] – [durée prévue]

II)	Actual_Costs

-	Promotion de la première ligne en en-tête.

-	Renommer le titre de la colonne « Proj_ID » en « Project_ID » pour plus de cohérence dans la liaison des tables.
-	Suppression des lignes vides

-	On crée ensuite la même clé primaire que dans la table précédente en fusionnant les 2 colonnes dupliquées en une colonne « Projet + Phase ID ».

-	On transforme ensuite le type de données en Texte pour « Projet + Phase ID » et « Phase », Nombre Entier pour « Project_ID » et Nombre Décimal pour « Actual_Cost ».

-	L’un des objectifs est d’alerter au-delà d’un dépassement de plus de 15%. J’ai donc créé une colonne qui calcule le taux de dépassement pour déterminer si chaque phase du projet dépasse la valeur seuil.

-	J’ai également créé une colonne conditionnelle qui attribue un statut en fonction du taux de dépassement : « OK » si en-dessous de 15% de dépassement et « Dépassé » au-delà.

-	Les 2 étapes précédentes servent à alimenter les graphiques de focus de projet en découpant par phase et alimente aussi les classements des projets par taux de dépassement décroissant (les mesures ne me permettent pas d’alimenter correctement les graphiques).

-	Cependant, j’ai utilisé des mesures pour créer mon alerte de coûts. Les mesures reprennent d’abord le [Budget prévu], puis le [Budget réel]. Enfin la mesure qui définit l’alerte stipule que si [Budget réel] – [Budget prévu] est supérieur ou égal à [Budget prévu] x 0,15, alors doit s’afficher « Budget dépassé de plus de 15% », sinon « Budget Respecté ».

-	Création de la mesure d’écart de coûts qui sert d’info-bulle aux graphiques : [Budget réel] – [Budget prévu].

III)	Country_Profiles

-	Promotion de la première ligne en en-tête.

-	On transforme ensuite le type de données en Texte.

-	Suppression des lignes et colonnes vides.

IV)	Deliverables_status

-	Promotion de la première ligne en en-tête.

-	Renommer le titre de la colonne « ID » en « Project_ID » pour plus de cohérence dans la liaison des tables.

-	Suppression des lignes vides.

-	On crée ensuite la même clé primaire que dans la table précédente en fusionnant les 2 colonnes dupliquées en une colonne « Projet + Phase ID ».
-	On transforme ensuite le type de données en Texte pour « Projet + Phase ID » et « Phase », Nombre Entier pour « Project_ID » et Nombre Décimal pour « Var_Deliverables ».

-	J’ai utilisé une mesure pour créer mon alerte de taux de livrables manquants. La mesure calcule si la moyenne du taux de livrable manquants pour un projet est inférieure ou égale à -15%, alors doit s’afficher « Plus de 15% de livrables manquants », sinon « Projet Complété ».


V)	Projects_Locations

-	Promotion de la première ligne en en-tête.

-	Suppression des lignes vides.

-	Fusion Externe Gauche sur la base du champ « Country » avec la Table « Country_Profiles » pour rassembler en une seule tables toutes les données géographiques, donc rajouter à notre tables « Projects_Locations » la « Région » et le « Type » de pays.

-	Transformation du type de données : « Project_ID » en Nombre Entier et « Country », « Region » et « Type » en Texte.

VI)	Project type

-	Promotion de la première ligne en en-tête.

-	Renommer le titre de la colonne « Project ID » en « Project_ID » pour plus de cohérence dans la liaison des tables.

-	Suppression des lignes vides.

-	Transformation du type de données : « Project_ID » en Nombre Entier et « Project Type » en Texte.

VII)	Project_plans

-	Promotion de la première ligne en en-tête.

-	Etant donné que la table ne comporte pas de clé primaire (le même Project_ID se retrouve autant de fois qu’il y’a de phases pour un projet), on duplique les colonnes « Project_ID » et « Phase ».

-	On crée ensuite une clé primaire en fusionnant ces 2 colonnes en une colonne « Projet + Phase ID ».

-	Renommer le titre de la colonne « Project ID » en « Project_ID » pour plus de cohérence dans la liaison des tables.
-	Suppression des lignes vides.

VIII)	Création de la table PROJETS GLOBAL COUTS

-	Je voulais que les directeurs aient accès aux KPIs de l’ensemble des projets et non au projet ou à la phase. J’ai donc décidé de créer cette nouvelle table dans laquelle l’ID Projet serait une clé primaire, sans les phases.

-	J’ai donc utilisé les données déjà entrées dans la table ‘Actual_Costs’, notamment la colonne ID Projet et ai calculé la moyenne du taux de dépassement des coûts par projet.

-	Enfin j’ai créé une colonne conditionnelle qui attribue un statut en fonction du taux de dépassement : « OK » si en-dessous de 15% de dépassement et « Budget dépassé » au-delà.

IX)	Création de la table PROJETS GLOBAL DUREE

-	Dans la même idée que pour la table précédemment expliquée, j’ai fait de même pour la durée des projets.

-	J’ai donc utilisé les données déjà entrées dans la table ‘Actual_Duration’, notamment la colonne ID Projet et ai calculé la moyenne du taux de dépassement de durée par projet.

-	Enfin j’ai créé une colonne conditionnelle qui attribue un statut en fonction du taux de dépassement : « OK » si en-dessous de 15% de dépassement et « En Retard » au-delà.







