# Cours Algorithmique avance langage C

## Objectifs de la formation

- Maîtriser les concepts et implémentations des structures de données dynamiques avancées en C.
- Appliquer les paradigmes algorithmiques avancés pour résoudre des problèmes complexes.
- Comprendre et analyser la complexité des algorithmes et optimiser les solutions.
- Développer des compétences pratiques en C pour l'implémentation d'algorithmes sophistiqués.


## 1: Rappels & Introduction avancée

### Objectifs pédagogiques

- Réviser et approfondir les concepts de complexité algorithmique.
- Comprendre l'importance du choix des structures de données pour l'optimisation.
- Maîtriser la gestion de la mémoire (pile, tas, pointeurs) en C pour l'algorithmique avancée.
### Contenus

#### Théorie: Complexité Algorithmique

- Rappel des notations (O, Ω, Θ).
- Analyse de cas (pire, meilleur, moyen).
- Calcul de complexité pour des algorithmes récursifs et itératifs.
- Impact de la complexité sur la performance des grands jeux de données.
#### Théorie: Optimisation et Gestion Mémoire Avancée

- Choix de structures de données : tableau vs liste chaînée, leurs compromis.
- Rappel sur la pile et le tas (stack and heap).
- Utilisation avancée des pointeurs et des pointeurs de fonctions.
- `malloc`, `calloc`, `realloc`, `free` : bonnes pratiques et erreurs courantes (fuites mémoire, pointeurs sauvages).
- Introduction aux dernières évolutions du langage C (C17, C23) et leur impact potentiel sur la gestion mémoire ou la sûreté du code.
#### TP: Analyse et Gestion Mémoire

## 2: Structures de données dynamiques avancées
### Objectifs pédagogiques

- Concevoir et implémenter des listes doublement chaînées et circulaires.
- Comprendre le principe et les méthodes de résolution des collisions dans les tables de hachage.
- Implémenter des piles et des files avancées basées sur des listes chaînées.
### Contenus

#### Théorie: Listes Doublement Chaînées et Circulaires

- Principes et structures des nœuds.
- Opérations (insertion, suppression, parcours) en tête, en queue, à une position donnée.
- Avantages et inconvénients par rapport aux listes simplement chaînées.
- Implémentation de listes circulaires.
#### Théorie: Tables de Hachage

- Principe du hachage, fonctions de hachage.
- Gestion des collisions (chaînage séparé, adressage ouvert - linéaire, quadratique).
- Complexité des opérations (insertion, recherche, suppression).
#### TP: Implémentation des Structures Avancées 

## 3: Arbres binaires et arbres équilibrés

### Objectifs pédagogiques

- Concevoir et manipuler des arbres binaires de recherche (ABR).
- Comprendre le concept d'équilibrage des arbres et ses avantages.
- Appréhender les principes des arbres équilibrés (AVL, Rouge-Noir).
### Contenus

#### Théorie: Arbres Binaires de Recherche (ABR) 

- Définition et propriétés des ABR.
- Opérations fondamentales : insertion, recherche, suppression (cas simples et complexes).
- Parcours d'arbres (infixe, préfixe, postfixe) : applications.
- Analyse de complexité des opérations.
#### Théorie: Introduction aux Arbres Équilibrés

- Problème des ABR dégénérés.
- Principe de l'équilibrage : pourquoi et comment.
- Introduction aux arbres AVL (facteur d'équilibre, rotations - présentation).
- Introduction aux arbres Rouge-Noir (propriétés, quelques exemples de rééquilibrage).
#### TP: Manipulation d'ABR

## 4: Graphes et algorithmes associés

### Objectifs pédagogiques

- Représenter des graphes de différentes manières en C.
- Implémenter les algorithmes de parcours (BFS, DFS).
- Comprendre les principes des algorithmes de chemins les plus courts et des arbres couvrants minimaux.
### Contenus

#### Théorie: Représentations de Graphes

- Définition et terminologie des graphes (sommets, arêtes, poids, orienté/non-orienté).
- Matrice d'adjacence : implémentation, avantages, inconvénients.
- Listes d'adjacence : implémentation, avantages, inconvénients.
- Choix de représentation en fonction du problème (graphes denses/creux).
#### Théorie: Algorithmes de Parcours

- Breadth-First Search (BFS) : principe, implémentation (file), applications.
- Depth-First Search (DFS) : principe, implémentation (pile/récursivité), applications.
- Analyse de complexité.
#### TP: Implémentation et Parcours de Graphes

## 5: Paradigmes avancés : Divide & Conquer, Backtracking

### Objectifs pédagogiques

- Comprendre et appliquer le paradigme Divide & Conquer (Diviser pour Régner).
- Implémenter des algorithmes de tri basés sur Divide & Conquer (Merge Sort, Quick Sort).
- Comprendre le principe du Backtracking et résoudre des problèmes combinatoires.
### Contenus

#### Théorie: Divide & Conquer

- Principe général du Diviser pour Régner.
- Analyse des récurrences (Master Theorem - introduction).
- Merge Sort : principe, complexité, implémentation.
- Quick Sort : principe, choix du pivot, complexité (pire, moyen), implémentation.
#### Théorie: Backtracking

- Principe général : exploration d'un espace d'état, retour sur trace.
- Cas typiques : problème des N-Dames, résolution de labyrinthe, Sudoku.
- Structure d'un algorithme de backtracking récursif.
#### TP: Implémentation de Tri et Backtracking

## 6: Programmation dynamique

### Objectifs pédagogiques

- Distinguer la programmation dynamique de la récursion simple.
- Appliquer les techniques de mémoïsation et de tabulation.
- Résoudre des problèmes d'optimisation classiques avec la programmation dynamique.
### Contenus

#### Théorie: Introduction à la Programmation Dynamique

- Quand utiliser la programmation dynamique (sous-problèmes chevauchants, sous-structure optimale).
- Différence et avantages par rapport à la récursion simple (redondance des calculs).
- Mémoïsation (top-down) : principe, implémentation avec tableaux.
- Tabulation (bottom-up) : principe, implémentation avec tableaux.
- Exemples introductifs : Suite de Fibonacci optimisée.
#### TP: Application de la Programmation Dynamique
