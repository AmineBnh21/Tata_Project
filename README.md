# EDA — Prédiction de la délinquance (Geldium × Tata iQ)

Analyse exploratoire du jeu de données de délinquance de Geldium, réalisée dans le cadre de la simulation Tata iQ.

## Fichiers

| Fichier | Contenu |
|---|---|
| `Delinquency_prediction_dataset.xlsx` | Jeu de données brut (500 clients, 19 colonnes) |
| `EDA_Geldium.ipynb` | Notebook complet : étapes 1 à 4 (exploration, traitement des manquants, facteurs de risque, synthèse) |
| `Delinquency_prediction_cleaned.csv` | Jeu nettoyé produit à l'étape 2 (0 valeur manquante) |
| `delinquency_rates.png` | Graphique des taux de délinquance par groupe (étape 3) |
| `EDA_Report_Geldium.docx` / `.pdf` | **Livrable** : rapport EDA basé sur le modèle fourni |
| `EDA_SummaryReport_Template.docx` | Modèle de rapport d'origine |
| `Tata_Data_Analytics_Glossary (1).docx` | Glossaire fourni |

## Reproduire

```bash
pip install pandas openpyxl scipy scikit-learn matplotlib jupyter
jupyter nbconvert --to notebook --execute --inplace EDA_Geldium.ipynb
```

## Conclusions principales

- Valeurs manquantes limitées : `Income` (7,8 %), `Loan_Balance` (5,8 %) et `Credit_Score` (0,4 %), imputées par la médiane, avec un indicateur de valeur manquante.
- `Employment_Status` normalisé (6 libellés → 4 catégories).
- Aucune variable n'est significativement liée à la délinquance, et les modèles de référence obtiennent un AUC ≤ 0,50.
- Plusieurs profils sont impossibles (comptes ouverts avant 18 ans, cartes Student détenues par des 30 ans et plus), et `Missed_Payments` contredit l'historique mensuel. La chaîne de données doit être vérifiée avec Geldium avant toute modélisation.
