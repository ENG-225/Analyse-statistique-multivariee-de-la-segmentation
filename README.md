# 🛍️ Segmentation de la Clientèle d'un Centre Commercial — Analyse Multivariée

> **Mini-Projet N°2 — Analyse des Données Multidimensionnelles**  
> INSSEDS — Master 2 : Statistique – Économétrie – Data Science  
> Année académique : 2025-2026 | Encadrant : AKPOSSO Didier Martial

---

## 📌 Description

Ce projet met en œuvre les principales techniques d'**analyse multivariée** pour segmenter 200 clients d'un centre commercial. À partir de variables démographiques, financières et comportementales, il identifie des **profils types** permettant d'orienter la stratégie marketing.

Quatre méthodes sont appliquées en séquence logique :

| Méthode | Objectif |
|---------|----------|
| **ACP** | Synthétiser les 6 variables quantitatives en composantes |
| **ACM** | Explorer les associations entre 3 variables qualitatives |
| **CAH** | Identifier la structure hiérarchique naturelle des groupes |
| **K-Means** | Produire une segmentation opérationnelle et actionnable |

---

## 📊 Données

- **Source** : Dataset enrichi « Mall Customers » (200 observations, 9 variables)
- **Valeurs manquantes** : 4 sur `Groupe d'Âge` → imputées par KNN (K=5, pondération distance)

### Variables quantitatives

| Variable | Moy. | Min | Max | CV (%) |
|----------|------|-----|-----|--------|
| Âge | 38,85 ans | 18 | 70 | 36 % |
| Revenu Annuel | 60,56 k$ | 15 | 137 | 43 % |
| Score Dépenses | 50,20 / 100 | 1 | 99 | 51 % |
| Épargne Estimée | 40,25 k$ | 6,46 | 120,56 | 53 % |
| Score de Crédit | 743,68 / 850 | 300 | 850 | 21 % |
| Années de Fidélité | 5,93 ans | 2 | 9 | 26 % |

### Variables qualitatives

| Variable | Modalités | Distribution |
|----------|-----------|--------------|
| Sexe | Femme / Homme | 56 % / 44 % |
| Groupe d'Âge | 18-25 / 26-35 / 36-50 / 51-65 / 65+ | 38 / 60 / 62 / 28 / 12 |
| Catégorie Préférée | Budget / Electronics / Fashion / Luxury | 27 / 78 / 41 / 54 |

---

## 📈 Résultats

### ACP — 95,02 % de variance expliquée en 3 axes

| Axe | λ | % Inertie | Cumulé | Variables dominantes |
|-----|---|-----------|--------|----------------------|
| **F1** | 2,756 | **45,94 %** | 45,94 % | Épargne (r=0,957), Score Crédit (r=0,857), Revenu (r=0,814) |
| **F2** | 1,676 | **27,92 %** | 73,86 % | Années Fidélité (r=0,936), Âge (r=0,787) |
| F3 | 1,270 | 21,16 % | 95,02 % | Score Dépenses |

**F1 = "Axe Richesse Financière"** | **F2 = "Axe Fidélité-Maturité"**

### ACM — Associations qualitatives

| Association détectée | Force |
|----------------------|-------|
| Electronics ↔ 36-50, 51-65, 65+ (seniors) | Très forte |
| Fashion ↔ 18-25, 26-35 (jeunes) | Forte |
| Luxury ↔ 26-35 ans (jeunes à revenus croissants) | Forte |
| Sexe → effet modéré sur les axes | Faible |

### Segmentation finale — 5 clusters (CAH & K-Means convergents)

| Cluster | Profil | n | % | Revenu | Sc.Dép. | Stratégie prioritaire |
|---------|--------|---|---|--------|---------|----------------------|
| 🔵 Seniors Electronics | Âge ~56 ans, fidèles 7,3 ans | 56 | 28 % | 49 k$ | 43 | Ateliers tech, newsletters |
| 💛 Riches Économes | Revenu élevé, épargne max, peu dépensiers | 34 | 17 % | 89 k$ | 17 | VIP, conseiller dédié |
| 🔴 Jeunes Précaires | Très jeunes, faibles revenus, score crédit bas | 28 | 14 % | 26 k$ | 72 | Paiement fractionné, parrainage |
| ⭐ **Jeunes Premium** | **Revenus élevés, gros dépensiers, excellent crédit** | **40** | **20 %** | **86 k$** | **82** | **PRIORITÉ 1 — VIP club, exclusivité** |
| 🟣 Jeunes Femmes Fashion | Féminin 69 %, Fashion 64 % | 42 | 21 % | 52 k$ | 43 | Instagram/TikTok, éditions limitées |

### Convergence CAH vs K-Means

| Segment | CAH | K-Means | Accord |
|---------|-----|---------|--------|
| Riches Économes | n=35 | n=34 | ✅ Très forte |
| Jeunes Premium | n=39 | n=40 | ✅ Très forte |
| Seniors Fidèles | n=51 | n=56 | ✅ Forte |
| Jeunes Précaires | n=21 | n=28 | ✅ Forte |

---

## 🛠️ Stack technique

![Python](https://img.shields.io/badge/Python-3.10+-blue?logo=python)
![scikit--learn](https://img.shields.io/badge/scikit--learn-1.x-orange)
![matplotlib](https://img.shields.io/badge/matplotlib-3.x-green)
![numpy](https://img.shields.io/badge/numpy-1.x-blue)

```python
from sklearn.preprocessing import StandardScaler
from sklearn.decomposition import PCA
from sklearn.cluster import KMeans, AgglomerativeClustering
from sklearn.impute import KNNImputer
import pandas as pd, numpy as np
import matplotlib.pyplot as plt
```

---

## 📁 Structure du dépôt

```
Analyse-Multivariee-Segmentation-Client/
├── README.md
├── analyse_multidim.py        # Script analyse statistique (ACP, ACM, CAH, K-Means)
├── generate_charts.py         # Script génération des graphiques
├── Mall_Customers.csv         # Dataset 200 clients enrichi
└── rapport/
    └── MINI_PROJET_ANALYSE_MULTIVARIEE.pdf
```

---

## ▶️ Lancement rapide

```bash
git clone https://github.com/ENG-225/Analyse-Multivariee-Segmentation-Client.git
cd Analyse-Multivariee-Segmentation-Client
pip install pandas numpy scikit-learn matplotlib Pillow
python analyse_multidim.py    # Analyse statistique
python generate_charts.py     # Génération des figures
```

---

## 💡 Recommandations marketing

| Segment | Action clé |
|---------|-----------|
| ⭐ Jeunes Premium (20 %) | **PRIORITÉ 1** — VIP club, accès anticipé, personnalisation |
| 💛 Riches Économes (17 %) | Offres sur-mesure, invitations exclusives, conseiller dédié |
| 🟣 Jeunes Fashion (21 %) | Campagnes Instagram/TikTok, influenceuses, éditions limitées |
| 🔵 Seniors (28 %) | Ateliers pratiques, newsletters spécialisées, programme fidélité |
| 🔴 Jeunes Précaires (14 %) | Paiement fractionné, entrée de gamme Luxury, parrainage |

---

## 📚 Références

- Abdi, H. & Williams, L.J. (2010). Principal Component Analysis. *WIREs*, 2(4), 433–459.
- Greenacre, M. (2017). *Correspondence Analysis in Practice*. Chapman & Hall.
- Jain, A.K. (2010). Data clustering: 50 years beyond K-Means. *Pattern Recognition Letters*, 31(8).
- Ward, J.H. (1963). Hierarchical grouping to optimize an objective function. *JASA*, 58(301).

---

## 👤 Auteur

**YAO Kouakou Jean Martinien**  
Master 2 — Statistique, Économétrie & Data Science  
INSSEDS | Année académique 2025-2026  
*Encadrant : AKPOSSO Didier Martial*
