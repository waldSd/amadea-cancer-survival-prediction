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
