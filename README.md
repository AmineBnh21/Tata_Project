# Prédiction de la délinquance (Geldium × Tata iQ)

Simulation Tata iQ : analyse exploratoire (partie 1) puis plan de modèle prédictif (partie 2) sur le jeu de données de délinquance de Geldium.

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

## Reproduire

```bash
pip install pandas openpyxl scipy scikit-learn matplotlib jupyter
jupyter nbconvert --to notebook --execute --inplace EDA_Geldium.ipynb
cd Part2_Modelisation && jupyter nbconvert --to notebook --execute --inplace Model_Plan_Geldium.ipynb
```

Le notebook de la partie 2 lit `Delinquency_prediction_cleaned.csv`, produit par la partie 1.

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
