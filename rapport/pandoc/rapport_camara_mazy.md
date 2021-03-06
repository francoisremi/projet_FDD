Classifications par apprentissage
==============

***Examen mi-parcours -- M2SSD UE Analyse de données***

**Élèves:** *Jah Sylla Camara, François-Rémi Mazy*

# Exercice 1

## Deux mises en place de GridSearch

Nous nous intéressons à deux manières différentes de mettre en place la fonction `GridSearchCV` :

Code 1 : 

~~~~~~~ { .python .numberLines}
grid_search.fit(X, y)
acc = grid_search.best_score_
~~~~~~~

Code 2 : 

~~~~~~~ { .python .numberLines}
cv_res = cross_val_score(grid_search, X, y, cv = 10)
acc = cv_res.mean()
~~~~~~~

La fonction `GridSearchCV` permet de réaliser plusieurs estimations pour différents paramètres (une grille de paramètres) afin de déterminer le jeu de paramètres qui obtient le score maximal. Ces estimations sont effectuées en appliquant la méthode de validation croisée. Par défaut, le jeu de données d'apprentissage est séparé en trois parties (*3-fold cross validation*).

Le code 1 effectue donc :

1. Pour chaque jeu de paramètres :
	i) entraînement du modèle par validation croisée en 3 parties.
	ii) calcul du score qui correspond au score moyen de la validation croisée.
2. Détermination du jeu de paramètre avec le score maximal.
3. attribuer à `acc` la valeur de ce score maximal.

Le code 2 quant à lui effectue une nouvelle cross validation, cette fois en 10 parties, sur le résultat de `grid_search`. On répète donc l'opération du code 1 dix fois et on récupère ensuite la valeur moyenne.

## Application à un classifieur SVM à noyau RBF

### Génération des données

Nous générons les données avec la fonction `make_moons` fournie par `scikit-learn`. Nous tirons 500 échantillons avec un bruit de $0.6$.
 
![Données générées par `make_moons(n_samples=500, noise=0.6, random_state=27)`](../../jupyter/figures/two-moon.svg "moons")

### Code 1

Nous appliquons le code 1 avec le classifieur SVM à noyau RBF pour une grille de paramètres définie comme suit :

~~~~~~~ { .python .numberLines}
param_C = [0.1, 0.5, 1.0, 5.0, 10.0]
param_gamma = [0.1, 0.5, 1, 5.0, 10.0]
~~~~~~~

En prenant 80% du jeu de donnée (le reste étant dédié au test ultérieur), il retourne un score maximal de $0.79$ pour le jeu de paramètre $(C,\gamma)=(1,1)$.

Nous entrons alors spécifiquement ce jeu de paramètre pour inférer sur les données test et il vient un score de $0.8$.

![données moons](../../jupyter/figures/two-moon-svm-rbf-gs.svg "moons-svm-rbf-gs")

### Code 2

Le code 2, appliqué lui aussi aux données d'entraînement nous retourne un score moyen de $0.77$, légèrement inférieur donc au score obtenu précédement. De plus, ce code ne permet pas d'accéder aux paramètres optimaux qui permettrait ensuite de les appliquer sur les données test. 