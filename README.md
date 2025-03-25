# Deep Learning Project

**Auteurs : MAIA Valentin, VUONG The Tai**

## 1. Introduction

Dans un contexte où la disponibilité massive de données et la puissance de calcul ont transformé des domaines comme la reconnaissance d'images, ce projet a pour objectif de construire, de zéro, un framework de deep learning. Le projet se décompose en plusieurs parties : la construction d’un réseau de neurones à deux couches, l’extension vers un réseau multicouche personnalisable, et la mise en œuvre d’un réseau de neurones convolutionnel inspiré de l’architecture LeNet-5. Deux exercices complémentaires portent sur la construction d’un réseau multicouche et d’un réseau convolutionnel, dont les détails sont expliqués tout au long du rapport.

## 2. Partie I : Réseau de Neurones à Deux Couches

### Concepts Clés
- **Neurone et couches :**  
  Un neurone réalise un produit scalaire entre les entrées et les poids, auquel s’ajoute un biais, puis applique une fonction d’activation.  
- **Réseau linéaire vs non linéaire :**  
  Un réseau composé d’une seule couche est linéaire. L’ajout d’une couche cachée avec une fonction d’activation non linéaire permet de modéliser des fonctions complexes.
- **Fonction d'activation de sortie :**  
  Pour une tâche de classification, la fonction softmax est utilisée pour convertir les sorties en probabilités.

## 3. Partie II : Réseau de Neurones Multicouche

### 3.1. Extensions et Personnalisations
- **Fonctions d’activation étendues :**  
  Outre la ReLU, plusieurs fonctions d’activation ont été implémentées :
  - **Tanh**
  - **Leaky ReLU** (donne une petite valeur aux entrées négatives)
  - **SELU** (auto-normalisante)
  - **ELU**
  
- **Initialisation des paramètres :**  
  La fonction d'initialisation est généralisée pour supporter un nombre variable de couches.  
  - Le nombre de couches total est défini comme le nombre de couches cachées + la couche de sortie.
  - La taille des matrices de poids est déterminée par le nombre de features en entrée et le nombre de neurones de chaque couche.

- **Dropout :**  
  La technique du dropout est intégrée pour réduire le sur-apprentissage en masquant aléatoirement certains neurones pendant l'entraînement.  
  - Une fonction de dropout applique un masque sur les entrées et ajuste l'échelle pour maintenir la constance du signal.

### 3.2. Architecture et Implémentation
- La fonction **multilayer_network** enchaîne les opérations de chaque couche en utilisant la fonction dense modifiée qui intègre le dropout si nécessaire.
- La classe **StochasticOptimizationProblem** et la fonction **stochastic_training** ont été adaptées pour gérer le réseau multicouche avec la nouvelle initialisation des paramètres.

## 4. Partie III : Réseau de Neurones Convolutionnel (LeNet-5)

### 4.1. Composants de l’Architecture CNN
- **Convolution :**  
  Une fonction de convolution est implémentée pour extraire des caractéristiques des images en faisant glisser un filtre sur les données.  
  - L’implémentation prend en compte le mode "valid" pour obtenir la forme de sortie correcte.
  
- **Pooling :**  
  Une couche de pooling (max pooling) est utilisée pour réduire la dimension spatiale des représentations sans modifier le nombre de canaux.
  
- **Flatten :**  
  La fonction flatten transforme un tenseur 4D en un vecteur 2D avant d'alimenter les couches entièrement connectées.
  
- **Initialisation des paramètres CNN :**  
  L’architecture LeNet-5 se compose de :
  - 2 couches de convolution avec paramètres connus :
    - Première couche : filtre de taille 5x5, 1 canal en entrée, 6 filtres (sortie 24x24x6)
    - Deuxième couche : filtre de taille 5x5, 6 canaux en entrée, 16 filtres (sortie 8x8x16)
  - 2 couches entièrement connectées :
    - Première couche FC : entrée aplatie (256 unités) vers 120 neurones
    - Deuxième couche FC : 120 vers 84 neurones
  - Une couche de sortie avec 10 neurones correspondant aux classes (chiffres de 0 à 9).

### 4.2. Fonctionnement du Réseau
- **Passage avant (Forward Pass) :**  
  La fonction de convolution suit l’ordre suivant :  
  Convolution → Pooling → Convolution → Pooling → Flatten → Fully Connected → Fully Connected → Softmax (pour la classification).
  
- **Optimisation :**  
  La classe de problème d’optimisation stochastique et la fonction d'entraînement ont été ajustées pour le réseau CNN, tenant compte des spécificités des couches convolutionnelles et de pooling.

## 5. Entraînement et Résultats

### 5.1. Entraînement sur MNIST
- **Prétraitement des données :**  
  Les images du dataset MNIST sont redimensionnées, normalisées et leurs labels encodés en one-hot.
- **Paramétrage et itérations :**  
  Différents nombres d’itérations ont été testés :
  - Un premier entraînement avec un nombre réduit d’itérations (ex. 60) pour une première validation.
  - Augmentation progressive des itérations pour observer une amélioration de la précision, avec des compromis sur le temps de calcul.
  
### 5.2. Performances
- **Réseau multicouche :**  
  Une précision de test de l'ordre de 95–96% a été obtenue avec un choix judicieux du nombre de couches, de neurones et d’activations.
- **CNN (LeNet-5) :**  
  - Un premier entraînement a donné environ 79–80% de précision.
  - En augmentant le nombre d’itérations, des améliorations significatives ont été observées, avec des précisions pouvant atteindre plus de 90%, tout en tenant compte du temps de calcul (trade-off entre performance et temps d'entraînement).

## 6. Conclusion et Axes d'Amélioration

Ce projet a permis de passer d’un simple réseau à deux couches à des architectures plus complexes telles que le réseau multicouche et le CNN (LeNet-5). Les principaux enseignements comprennent :
- L’importance de la personnalisation des architectures (choix des activations, nombre de couches, dropout).
- La nécessité d’un prétraitement efficace des données pour accélérer la convergence.
- Un compromis constant entre la complexité du modèle, la précision obtenue et le temps d’entraînement.

Les axes d’amélioration futurs pourraient inclure :
- L’optimisation des hyperparamètres (nombre de neurones, taux de dropout, etc.).
- L’expérimentation avec d’autres architectures CNN ou techniques de régularisation.
- L’application du framework sur d’autres types de données et tâches de classification.

---
