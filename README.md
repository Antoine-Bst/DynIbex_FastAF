# DYNIbex — Quick Computation of Zonotope from Dynibex

## 📌 Description
Ce programme illustre une méthode pour :

- manipuler des représentations **affines incertaines** avec [**DynIbex**](https://perso.ensta-paris.fr/~chapoutot/dynibex/),  
- générer les **sommets candidats** d’un **zonotope**,  
- calculer son **enveloppe convexe** à l’aide de [**Qhull**](http://www.qhull.org/),  
- extraire et afficher les **sommets de l’enveloppe convexe**.  

Il a été développé dans le cadre de la recherche à l’**ENSTA Paris**  
(projet STARTS - CIEDS - Institut Polytechnique).

---

## ⚙️ Dépendances
Pour compiler et exécuter ce programme, vous devez avoir installés :

- [Ibex/DynIbex](https://perso.ensta-paris.fr/~chapoutot/dynibex/) **!Utiliser la verision perso dans mon repo!**
- [Qhull](http://www.qhull.org/)

### Installation Qhull (Ubuntu/Debian)
```bash
sudo apt-get install qhull-bin libqhull-dev
````

---

## 🏗️ Compilation

Le projet est compilé via un **Makefile**.

Compilation :

```bash
make
```

Exécution :

```bash
./simulation.out
```

---

## 🧩 Structure du code

### 0. Représentation affine (`AffineDecomp`)

* `center` : valeur centrale,
* `coeffs` : coefficients associés aux variables de bruit `eps_i`,
* `garbage` : intervalle représentant l’erreur.

Outils :

* `make_affine` : crée une forme affine DynIbex depuis ma forme perso,
* `decompose_affine` : convertit en format interne depuis dynibex,
* `DyniAff2Vec` : transforme un vecteur affine DynIbex en `vector<AffineDecomp>`.

---

### 1. Pipeline complet

* `Affine2Vertices` :

  1. génère les sommets candidats via `candidate_vertices`,
  2. applique Qhull pour obtenir l’enveloppe convexe.

---


### 2. Génération de sommets candidats

* `build_generators` : construit les générateurs du zonotope,
* `generate_sign_combinations` : toutes les combinaisons de signes possibles,
* `candidate_vertices` : calcule les sommets candidats (centre + combinaisons).

---

### 3. Enveloppe convexe avec Qhull

* `computeConvexHullVertices` : prend un `std::vector<std::vector<double>>`
  et retourne les sommets de l’enveloppe convexe.

---


### 4. Exemple principal (`main`)

* Définit deux variables affines (`x1`, `x2`),
* Construit un vecteur affine (`yinit_aff`),
* Calcule les sommets du zonotope avec `Affine2Vertices`,
* Affiche le résultat.

Une **simulation différentielle (Van der Pol)** est incluse en commentaire pour illustrer l’intégration avec DynIbex.

---

## Exemple d’exécution
Sortie (console) :

```
AffineDecomp vector (2 elements):
  [0] 4 + 0.1*eps_1 + 0.1*eps_2 + 0.1*eps_3 + 0.25*eps_4 + [-0, 0]
  [1] 2 - 0.08*eps_1 + 0.15*eps_2 + 0.05*eps_3 + [-0, 0]
Known generator : eps_1 ; eps_2 ; eps_3 ; eps_4 ; 
Sommets de l'enveloppe convexe :
3.65 2.18
4.35 2.28
3.85 2.28
3.65 1.72
3.45 1.88
4.35 1.82
4.55 2.12
4.15 1.72

```

---

## 📖 Licence

Ce programme est distribué sous licence **GNU LGPL**.
