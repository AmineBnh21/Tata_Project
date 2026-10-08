# Prédiction de la délinquance (Geldium × Tata iQ)

Simulation Tata iQ : analyse exploratoire (partie 1), plan de modèle prédictif (partie 2) et rapport métier pour le recouvrement (partie 3) sur le jeu de données de délinquance de Geldium.

## Partie 1 — EDA (racine du dépôt)

| Fichier | Contenu |
|---|---|
| `Delinquency_prediction_dataset.xlsx` | Jeu de données brut (500 clients, 19 colonnes) |
| `EDA_Geldium.ipynb` | Notebook complet : étapes 1 à 4 (exploration, traitement des manquants, facteurs de risque, synthèse) |
| `Delinquency_prediction_cleaned.csv` | Jeu nettoyé produit à l'étape 2 (0 valeur manquante) |
| `delinquency_rates.png` | Graphique des taux de délinquance par groupe (étape 3) |
| `EDA_Report_Geldium.docx` / `.pdf` | **Livrable** : rapport EDA basé sur le modèle fourni |
| `EDA_SummaryReport_Template.docx` | Modèle de rapport d'origine |
| `Tata_Data_Analytics_Glossary (1).docx` | Glossaire fourni |

## Partie 2 — Plan de modèle prédictif (`Part2_Modelisation/`)

| Fichier | Contenu |
|---|---|
| `Model_Plan_Geldium.ipynb` | Notebook complet : étapes 1 à 4 (logique du modèle, justification, évaluation et équité, synthèse) |
| `Model_Plan_Geldium.docx` / `.pdf` | **Livrable** : plan de modèle basé sur le modèle fourni |
| `Task 2_ModelPlan_Template.docx` | Modèle de plan d'origine |

## Partie 3 — Rapport métier pour le recouvrement (`Part 3/`)

| Fichier | Contenu |
|---|---|
| `Business_Report_Geldium.ipynb` | Notebook complet : étapes 1 à 4 (segments à risque, recommandation SMART, éthique, synthèse) |
| `segments_risque.png` | Taux de délinquance par segment avec intervalles de confiance |
| `Business_Report_Geldium.docx` / `.pdf` | **Livrable** : rapport de deux pages basé sur le modèle fourni |
| `Updated_Business_Summary_Report_Template.docx` | Modèle de rapport d'origine |

## Reproduire

```bash
pip install pandas openpyxl scipy scikit-learn matplotlib jupyter
jupyter nbconvert --to notebook --execute --inplace EDA_Geldium.ipynb
cd Part2_Modelisation && jupyter nbconvert --to notebook --execute --inplace Model_Plan_Geldium.ipynb && cd ..
cd "Part 3" && jupyter nbconvert --to notebook --execute --inplace Business_Report_Geldium.ipynb
```

Les notebooks des parties 2 et 3 lisent `Delinquency_prediction_cleaned.csv`, produit par la partie 1. La partie 3 nécessite aussi `statsmodels`.

## Conclusions principales

**EDA**
- Valeurs manquantes limitées : `Income` (7,8 %), `Loan_Balance` (5,8 %) et `Credit_Score` (0,4 %), imputées par la médiane, avec un indicateur de valeur manquante.
- `Employment_Status` normalisé (6 libellés → 4 catégories).
- Aucune variable n'est significativement liée à la délinquance.
- Plusieurs profils sont impossibles (comptes ouverts avant 18 ans, cartes Student détenues par des 30 ans et plus), et `Missed_Payments` contredit l'historique mensuel.

**Plan de modèle**
- Modèle recommandé : **régression logistique pondérée**, transparente, conforme et simple à déployer, avec un gradient boosting comme challenger.
- 5 variables principales : historique de paiement, `Credit_Utilization`, `Debt_to_Income_Ratio`, `Credit_Score`, `Employment_Status`. `Age` et `Location` sont exclues du modèle et réservées à l'audit d'équité.
- Évaluation : AUC, rappel, précision, F1, score de Brier, impact disparate et égalité des chances, chacun avec un seuil d'acceptation.
- Sur les données actuelles, l'AUC est de 0,49 : le modèle est jugé **non déployable**. La chaîne de données doit être corrigée avec Geldium avant toute modélisation.

**Rapport métier**
- 3 principaux facteurs de risque : comptes de moins d'un an (28,6 % de défaut), revenus instables (jusqu'à 33 %), endettement élevé (18,9 %).
- Recommandation SMART : programme d'accompagnement des 12 premiers mois pour les nouveaux comptes, avec un objectif de 28,6 % à 20 % de défaut. Pilote avec groupe témoin du 1ᵉʳ décembre 2026 au 31 mai 2027.
- Éthique : risque de discrimination indirecte (`Location`, proxys de l'âge) et de sollicitation excessive des clients vulnérables, atténués par un programme uniquement aidant, des audits d'équité et une décision humaine.
