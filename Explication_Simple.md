# 🚖 Explication Simple — Ce qu'on a fait avec le dataset Sosso Trajet

> **À qui s'adresse ce fichier ?**  
> À quelqu'un qui ne connaît pas forcément la Data Science, mais qui veut comprendre ce qu'on a fait avec les données.

---

## 🤔 C'est quoi ce projet ?

On participe à un **hackathon** (compétition de data science).  
On a un fichier de données (un tableau Excel en gros) qui contient **7 341 courses de taxi** à Yaoundé et Douala.

Chaque ligne du tableau = une course, avec des infos comme :
- D'où part le taxi et où il va
- Combien ça coûte (3 types de prix)
- Combien de temps ça prend
- L'heure de la course
- Si des taxis sont disponibles ou pas

**Notre but final :** Utiliser ces données pour construire un programme qui peut **prédire le prix d'une course** ou **recommander le meilleur moment pour partir**.

---

## 🧰 Ce qu'on a fait : 3 étapes, comme des préparatifs avant de cuisiner

Imagine que tu veux cuisiner un plat. Avant de commencer, tu dois :
1. Sortir les ingrédients du frigo
2. Regarder ce que tu as
3. Vérifier ce qui est périmé ou manquant

C'est exactement ce qu'on a fait avec les données. 👇

---

## 📦 ÉTAPE 1 — On a ouvert le fichier

**Ce qu'on a fait :**  
On a "ouvert" le fichier CSV (comme ouvrir un fichier Excel) avec Python.

**Ce qu'on a vérifié :**
- Combien de lignes et colonnes il y a → **7 341 lignes, 11 colonnes**
- Les noms de toutes les colonnes (id_course, ville, prix, etc.)
- Les 5 premières lignes pour voir à quoi ça ressemble

**En gros :** On a sorti les ingrédients du frigo et on a regardé l'étiquette.

---

## 🔍 ÉTAPE 2 — On a inspecté les données en détail

**Ce qu'on a fait :**  
On a regardé chaque colonne de plus près pour comprendre ce qu'elle contient.

### Ce qu'on a regardé :

#### 📊 Les types de données
On a vérifié si chaque colonne contient des **chiffres** ou du **texte**.  
Exemple : La colonne `prix_eco` doit contenir des chiffres. Si Python la traite comme du texte, il y a un problème.

#### 📈 Les statistiques de base
Pour les colonnes de prix et de distance, on a calculé :
- Le prix **minimum** et **maximum**
- Le prix **moyen**
- Le prix **médian** (la valeur du milieu si on trie tout)

→ Ça permet de détecter des **prix aberrants** (ex: une course à 0 FCFA ou à 1 000 000 FCFA, c'est suspect).

#### 🏙️ La répartition Yaoundé / Douala
On a compté combien de courses viennent de chaque ville.  
→ Si Yaoundé a 90% des courses et Douala seulement 10%, c'est un **déséquilibre** qui peut fausser nos analyses. On a mis une **alerte automatique** si ça arrive.

#### ⏰ Les heures et disponibilités
On a listé toutes les plages horaires (ex: "8h–9h", "13h–14h"...) et les statuts de disponibilité (vert, jaune, orange, rouge, gris).

#### 🚨 Les valeurs bizarres (outliers)
On a détecté les **prix extrêmes** qui sont anormalement hauts ou bas.  
→ Méthode utilisée : **l'écart interquartile (IQR)** — une technique statistique simple pour trouver les valeurs qui s'éloignent trop de la normale.

---

## 🩺 ÉTAPE 3 — On a cherché les données manquantes

**Ce qu'on a fait :**  
Dans un vrai tableau de données, il y a souvent des **cases vides** (NaN = "Not a Number" = case vide).

C'est comme une recette de cuisine où il manque des ingrédients.

### Ce qu'on a cherché :

#### ❓ Combien de cases vides par colonne ?
On a créé un tableau qui montre, pour chaque colonne :
- Combien de cases sont vides
- En pourcentage (ex: "15% des prix_eco sont vides")
- Un code couleur 🔴🟠🟡🟢 selon la gravité

#### 🔬 Les cases vides sont-elles liées à quelque chose ?
On a regardé si les cases vides dans les colonnes de prix apparaissent **plus souvent à certaines heures** ou **dans certaines villes**.

Si oui, c'est une information utile : les données ne manquent pas au hasard, elles manquent pour une raison précise.

#### 🛠️ Comment remplir les cases vides ?

On ne peut pas juste supprimer toutes les lignes avec des données manquantes (on perdrait trop d'info). Alors on **remplit intelligemment**.

**Notre stratégie choisie :**  
Pour les colonnes de prix, on remplace la case vide par la **médiane des courses similaires** — c'est-à-dire les courses de la **même ville** à la **même heure**.

**Pourquoi c'est mieux ?**  
Parce qu'un taxi à Yaoundé à 8h du matin et un taxi à Douala à 23h, ça n'a pas le même prix. En prenant la médiane par groupe, on remplit avec la valeur la plus réaliste possible.

**Et si tout un groupe est vide ?**  
On prend la médiane globale de toute la colonne comme plan de secours.

---

## ✅ Résumé — Ce qu'on a appris

| Étape | Ce qu'on a fait | Ce qu'on a découvert |
|---|---|---|
| **1. Ouverture** | On a chargé le fichier | 7 341 courses, 11 colonnes |
| **2. Inspection** | On a analysé chaque colonne | Déséquilibre possible Yaoundé/Douala, prix aberrants, heures à formater |
| **3. Données manquantes** | On a trouvé et planifié de remplir les cases vides | Les prix manquants se remplissent par la médiane du groupe (ville + heure) |

---

## 🔜 Prochaines étapes (pas encore faites)

1. **Nettoyer les données** — Appliquer vraiment la stratégie de remplissage
2. **Créer de nouvelles variables** — Extraire "8h" de "8h–9h", transformer les couleurs en chiffres (vert = 5, rouge = 1)
3. **Construire le modèle** — Entraîner un algorithme qui prédit le prix
4. **Évaluer les résultats** — Mesurer si notre modèle est précis

---

*Fichier généré le 18 Avril 2026 — Hackathon DATA 4 CHANGE — Projet Sosso Trajet*
