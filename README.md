# Clustering sur Breast Cancer Wisconsin Diagnostic Dataset

## Contexte
Analyse de clustering non-supervisé sur le dataset Breast Cancer Wisconsin Diagnostic contenant 569 tumeurs mammaires caractérisées par 30 features morphologiques extraites d'images de biopsie.

## 1. Exploration des Données

### Caractéristiques du dataset
- **569 échantillons** : 357 tumeurs bénignes (62.7%), 212 malignes (37.3%)
- **30 features numériques** : 10 mesures × 3 statistiques (mean, standard error, worst)
- **Qualité parfaite** : Aucune valeur manquante
- **Features** : radius, texture, perimeter, area, smoothness, compactness, concavity, concave points, symmetry, fractal dimension

### Analyse des corrélations
- **44 paires de features avec corrélation > 0.8** détectées
- **Corrélations extrêmes** : radius ↔ perimeter (0.998), radius ↔ area (0.987)
- **Redondance identifiée** :
  - Mesures géométriques (radius, perimeter, area) très corrélées
  - Features de forme (compactness, concavity, concave_points) corrélées
  - Statistiques mean ↔ worst corrélées pour une même mesure

## 2. Preprocessing Appliqué

### Standardisation
- **StandardScaler** appliqué sur toutes les features
- **Justification** : Échelles très différentes (ex: radius ~14 vs area ~654)
- **Obligatoire** pour K-means (sensible aux échelles)

### Réduction de dimensions - ACP
- **10 composantes principales** pour conserver **95% de la variance**
- **Réduction efficace** : 30 → 10 dimensions (67% de réduction)
- **Key features identifiées** : area_worst, concave_points_worst, perimeter_worst, radius_worst, concavity_mean, etc.
- **Objectif** : Éliminer la multicolinéarité et la redondance d'information

### Visualisations
- Scatter plots entre key features ACP
- Distribution des corrélations
- Matrice de corrélation par groupes (mean, se, worst)

## 3. Clustering K-means (Test Initial)

### Deux jeux de données testés
1. **Données complètes** : 30 features standardisées
2. **Key features ACP** : 10 features principales standardisées

### Résultats préliminaires (k=2 à k=10)

**Sur données complètes (30 features) :**
- Meilleur k : **k=2** (Silhouette = 0.451)
- Cohérent avec les 2 classes naturelles (bénin/malin)

**Sur key features ACP (10 features) :**
- Meilleur k : **k=2** (Silhouette = 0.362)
- Performances légèrement inférieures mais plus stable

### Métriques utilisées (non-supervisées)
- **Silhouette Score** : Cohésion intra-cluster vs séparation inter-cluster
- **Calinski-Harabasz** : Ratio variance inter/intra clusters
- **Davies-Bouldin** : Similarité moyenne entre clusters

## 4. Conclusions du Preprocessing

### Observations principales
1. **Forte séparabilité naturelle en 2 groupes** correspondant aux classes bénin/malin
2. **Redondance importante** (44 corrélations fortes) justifie l'ACP
3. **Données complètes > Key features** pour la qualité du clustering
4. **K=2 optimal** selon toutes les métriques non-supervisées

### Recommandations
- **Standardisation indispensable** : échelles hétérogènes
- **ACP recommandée** : réduit efficacement la multicolinéarité

### Limites identifiées
- Sur-représentation des caractéristiques géométriques (radius, perimeter, area quasi-identiques)
- Redondance par design (mean, se, worst mesurent la même feature)

## 5. Suite de l'Étude

### À réaliser
1. **Comparaison des variantes K-means** : Standard, Mini-Batch, Bisecting
2. **Validation supervisée** : ARI, NMI avec les vraies classes
3. **Analyse approfondie** : Confusion matrix, profil des clusters
4. **Comparaison finale** : Clustering avant/après ACP

### Données prêtes
- `X_all_scaled` : 30 features standardisées (569, 30)
- `X_key_scaled` : Key features ACP standardisées (569, 10)
- `y` : Labels vrais pour validation finale

# étude PCA montre que :
PCA 90.0% variance: 7 composantes (0.910 variance)
PC1: concave_points_mean
PC2: fractal_dimension_mean
PC3: texture_se
PC4: texture_worst
PC5: smoothness_mean
PC6: symmetry_worst
PC7: fractal_dimension_worst
   PCA 95.0% variance: 10 composantes (0.952 variance)
PC1: concave_points_mean
PC2: fractal_dimension_mean
PC3: texture_se
PC4: texture_worst
PC5: smoothness_mean
PC6: symmetry_worst
PC7: fractal_dimension_worst
PC8: smoothness_se
PC9: concavity_se
PC10: symmetry_mean
   PCA 99.0% variance: 17 composantes (0.991 variance)
PC1: concave_points_mean
PC2: fractal_dimension_mean
PC3: texture_se
PC4: texture_worst
PC5: smoothness_mean
PC6: symmetry_worst
PC7: fractal_dimension_worst
PC8: smoothness_se
PC9: concavity_se
PC10: symmetry_mean
PC11: concavity_se
PC12: compactness_worst
PC13: concave_points_se
PC14: compactness_se
PC15: fractal_dimension_mean
PC16: fractal_dimension_worst
PC17: symmetry_worst

