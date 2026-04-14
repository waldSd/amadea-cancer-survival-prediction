# 🧬 AMADEA — Prédiction de la Survie au Cancer du Sein par ML sur Données Génomiques

> Projet académique réalisé dans le cadre du diplôme d'Ingénieur IA & Science des Données — CNAM BDIA P4 (2023)

---

## 🇫🇷 Version Française

### Contexte

Le cancer du sein est l'un des cancers les plus fréquents et les plus étudiés au monde. Prédire la survie d'un patient à partir de son profil génomique est un défi majeur de la médecine de précision : avec des dizaines de milliers de gènes potentiellement impliqués, identifier ceux qui ont un réel pouvoir prédictif sur la survie est un problème de sélection de features en haute dimensionnalité.

Ce projet avait pour objectif de développer un pipeline complet de ML pour prédire le statut vital d'un patient atteint d'un cancer du sein à partir de ses données génomiques, en deux étapes : sélection des gènes les plus explicatifs via une optimisation ROC itérative, puis entraînement d'un réseau de neurones sur ces features sélectionnées.

---

### Données

| Propriété | Valeur |
|-----------|--------|
| Nombre de patients | 379 |
| Nombre de gènes (features) | 17 284 |
| Variable cible | `vital_status` (survie/décès) |
| Type de problème | Classification binaire |

Le dataset présentait un défi classique en bioinformatique : un nombre de features (17 284 gènes) très largement supérieur au nombre d'observations (379 patients), rendant la sélection de features indispensable avant toute modélisation.

---

### Méthodologie

#### Étape 1 — Sélection de Features par Optimisation ROC (AMADEA)

Face à 17 284 gènes candidats, une sélection exhaustive était impossible. Nous avons utilisé la plateforme **AMADEA** pour mettre en place un processus itératif de sélection :

1. **Symbolisation des données** : transformation des valeurs continues en classes symboliques par comparaison aux médianes et percentiles de chaque variable
2. **Échantillonnage aléatoire** : tirage aléatoire de grappes de colonnes (jusqu'à 1% du total soit ~172 gènes par tirage)
3. **Calcul du ROC** : évaluation de la qualité explicative de chaque combinaison via le ratio ROC réel / ROC idéal
4. **Itération** : 1 500 répétitions pour explorer l'espace des combinaisons possibles
5. **Sélection** : conservation de la combinaison de gènes ayant obtenu le meilleur score ROC

**Résultat :** 173 gènes sélectionnés sur 17 284, avec un ROC de 1.0 sur l'ensemble d'entraînement.

#### Étape 2 — Réseau de Neurones (JNETTE)

Un réseau de neurones fully-connected a été entraîné sur les 173 gènes sélectionnés :

| Couche | Neurones | Fonction d'activation |
|--------|----------|----------------------|
| Entrée | 173 | — |
| Couche 1 | 200 | Sigmoid |
| Couche 2 | 250 | ReLU |
| Couche 3 | 300 | Tanh |
| Sortie | 1 | Binaire |

- **Méthode d'entraînement :** Resilient Backpropagation
- **Itérations :** 100
- **Résultat :** Précision de 100% sur l'ensemble de test, matrice de confusion parfaite

---

### Résultats & Analyse Critique

Le modèle atteint une précision de 100% sur les données de test. Ces résultats, bien qu'impressionnants, doivent être interprétés avec prudence :

**Limites identifiées :**
- Le ratio features/observations (17 284 gènes pour 379 patients) est extrêmement défavorable et constitue un terrain fertile pour le surapprentissage
- L'absence de validation croisée stricte et de jeu de test indépendant limite la généralisation des conclusions
- Une précision parfaite sur données médicales est rarement atteignable en conditions réelles et doit systématiquement alerter sur un possible data leakage ou surapprentissage

**Ce que ce projet démontre :**
- Capacité à travailler sur des données biomédicales haute dimensionnalité
- Maîtrise des métriques d'évaluation en classification médicale (ROC, matrice de confusion)
- Esprit critique sur les résultats : identification proactive des biais potentiels
- Compréhension des enjeux de sélection de features en bioinformatique

---

### Stack Technique

- **AMADEA** — Plateforme de recherche opérationnelle pour la sélection de features
- **JNETTE** — Environnement de construction et d'entraînement de réseaux de neurones
- **Analyse ROC** — Évaluation de l'explicabilité des variables
- **Réseaux de neurones fully-connected** — Classification binaire

---

### Fichiers


---

---

# 🧬 AMADEA — Breast Cancer Survival Prediction with ML on Genomic Data

> Academic project completed as part of the AI & Data Science Engineering degree — CNAM BDIA P4 (2023)

---

## 🇬🇧 English Version

### Context

Breast cancer is one of the most studied cancers worldwide. Predicting patient survival from genomic profiles is a core challenge in precision medicine: with tens of thousands of potentially relevant genes, identifying those with genuine predictive power over survival is a high-dimensional feature selection problem.

This project aimed to build a complete ML pipeline to predict the vital status of breast cancer patients from genomic data, in two steps: selecting the most informative genes via iterative ROC optimization, then training a neural network on the selected features.

---

### Data

| Property | Value |
|----------|-------|
| Number of patients | 379 |
| Number of genes (features) | 17,284 |
| Target variable | `vital_status` (survived/deceased) |
| Problem type | Binary classification |

The dataset presented a classic bioinformatics challenge: the number of features (17,284 genes) vastly exceeded the number of observations (379 patients), making feature selection a critical prerequisite to any modeling step.

---

### Methodology

#### Step 1 — Feature Selection via ROC Optimization (AMADEA)

With 17,284 candidate genes, exhaustive selection was computationally infeasible. We used the **AMADEA** platform to implement an iterative selection process:

1. **Data symbolization**: continuous values converted to symbolic classes by comparison to column medians and percentiles
2. **Random sampling**: random sampling of gene clusters (up to 1% of total features per draw, ~172 genes)
3. **ROC computation**: evaluating the explanatory quality of each combination via the real ROC / ideal ROC ratio
4. **Iteration**: 1,500 repetitions to explore the space of possible combinations
5. **Selection**: retaining the gene combination with the highest ROC score

**Result:** 173 genes selected out of 17,284, with a ROC score of 1.0 on the training set.

#### Step 2 — Neural Network (JNETTE)

A fully-connected neural network was trained on the 173 selected genes:

| Layer | Neurons | Activation |
|-------|---------|------------|
| Input | 173 | — |
Layer 1 | 200 | Sigmoid |
| Layer 2 | 250 | ReLU |
| Layer 3 | 300 | Tanh |
| Output | 1 | Binary |

- **Training method:** Resilient Backpropagation
- **Iterations:** 100
- **Result:** 100% accuracy on test set, perfect confusion matrix

---

### Results & Critical Analysis

The model achieved 100% accuracy on test data. While impressive, these results must be interpreted carefully:

**Identified limitations:**
- The feature-to-observation ratio (17,284 genes for 379 patients) is extremely unfavorable and is a classic setup for overfitting
- The absence of strict cross-validation and an independent holdout test set limits the generalizability of the findings
- Perfect accuracy on medical data is rarely achievable in real-world conditions and should systematically raise concerns about potential data leakage or overfitting

**What this project demonstrates:**
- Ability to work with high-dimensional biomedical data
- Proficiency in medical classification evaluation metrics (ROC curve, confusion matrix)
- Critical thinking on model results: proactive identification of potential biases
- Understanding of feature selection challenges in bioinformatics

---

### Technical Stack

- **AMADEA** — Operational research platform for feature selection
- **JNETTE** — Neural network construction and training environment
- **ROC Analysis** — Variable explanatory power evaluation
- **Fully-connected neural networks** — Binary classification

---

### Files
