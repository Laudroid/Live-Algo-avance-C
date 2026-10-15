
### **TP : Maîtrise des Paradigmes Divide & Conquer et Backtracking en C**

**Objectifs Pédagogiques :**

À l'issue de ce TP, vous serez capable de :
*   Implémenter un algorithme de tri basé sur le principe Divide & Conquer (Merge Sort).
*   Comprendre et appliquer le principe de la récursivité pour la division et la combinaison.
*   Modéliser et résoudre un problème combinatoire simple (N-Dames pour N=4) en utilisant le Backtracking.
*   Gérer les structures de données nécessaires (tableaux, potentiellement allocation dynamique) en C.
*   Développer une approche structurée pour la résolution de problèmes algorithmiques.

**Prérequis :**

*   Maîtrise des bases du langage C (variables, boucles, conditions, fonctions, tableaux, pointeurs, récursivité).
*   Connaissance des concepts de base de la complexité algorithmique (sans nécessairement faire une analyse formelle ici).

**Matériel :**

*   Un environnement de développement C (GCC, Clang, Visual Studio, etc.).
*   Un éditeur de texte ou un IDE.

---

### **Partie 1 : Divide & Conquer - Implémentation du Merge Sort**

Le Merge Sort est un algorithme de tri efficace qui repose sur le principe "Diviser pour Régner". Il divise un tableau en deux moitiés, trie récursivement chaque moitié, puis fusionne les deux moitiés triées pour obtenir un tableau entièrement trié.

**Tâches à réaliser :**

1.  **Fonction `merge(int arr[], int l, int m, int r)` :**
    *   Cette fonction est le cœur de la phase de "combinaison".
    *   Elle prend en entrée un tableau `arr`, et trois indices : `l` (début de la première sous-partie), `m` (fin de la première sous-partie et début de la seconde), et `r` (fin de la seconde sous-partie).
    *   Votre tâche est de fusionner les deux sous-tableaux `arr[l...m]` et `arr[m+1...r]` (qui sont supposés déjà triés) en un seul sous-tableau trié `arr[l...r]`.
    *   Vous aurez probablement besoin d'un tableau temporaire pour stocker les éléments fusionnés avant de les recopier dans le tableau original. Pensez à l'allocation dynamique si vous voulez gérer des tailles variables, ou un tableau statique de taille maximale si vous travaillez avec une taille fixe connue.

2.  **Fonction `mergeSort(int arr[], int l, int r)` :**
    *   C'est la fonction récursive qui implémente la logique "Diviser pour Régner".
    *   `l` est l'indice de début du sous-tableau à trier, `r` est l'indice de fin.
    *   **Cas de base :** Si `l >= r`, le sous-tableau a 0 ou 1 élément, il est donc déjà trié. La fonction ne fait rien.
    *   **Étape récursive :**
        *   Calculez l'indice `m` du milieu : `m = l + (r - l) / 2`.
        *   Appelez récursivement `mergeSort` pour la première moitié : `mergeSort(arr, l, m)`.
        *   Appelez récursivement `mergeSort` pour la seconde moitié : `mergeSort(arr, m + 1, r)`.
        *   Appelez la fonction `merge` pour fusionner les deux moitiés triées : `merge(arr, l, m, r)`.

3.  **Fonction `main()` :**
    *   Déclarez un tableau d'entiers non trié (par exemple, `int data[] = {12, 11, 13, 5, 6, 7};`).
    *   Affichez le tableau avant le tri.
    *   Appelez `mergeSort` sur ce tableau.
    *   Affichez le tableau après le tri pour vérifier le résultat.

**Conseils :**
*   Commencez par la fonction `merge`. Testez-la avec deux sous-tableaux déjà triés pour vous assurer qu'elle fonctionne correctement.
*   La récursivité peut être déroutante au début. Dessinez l'arbre des appels si nécessaire.

---

### **Partie 2 : Backtracking - Le Problème des N-Dames (pour N=4)**

Le problème des N-Dames consiste à placer N reines sur un échiquier de N x N cases de telle sorte qu'aucune reine ne puisse en attaquer une autre. Deux reines s'attaquent si elles sont sur la même ligne, la même colonne ou la même diagonale. Nous allons implémenter une version simplifiée pour N=4.

**Modélisation :**
Nous pouvons représenter l'échiquier par un tableau 1D `int board[N]`, où `board[i]` représente la colonne où est placée la reine de la ligne `i`. Par exemple, `board[0] = 2` signifie que la reine de la ligne 0 est placée dans la colonne 2.

**Tâches à réaliser :**

1.  **Fonction `isSafe(int board[], int row, int col, int N)` :**
    *   Cette fonction vérifie si placer une reine à la position `(row, col)` est sûr, étant donné les reines déjà placées dans les lignes `0` à `row-1`.
    *   **Vérification de la colonne :** Parcourez les lignes précédentes (`i` de `0` à `row-1`). Si `board[i] == col`, cela signifie qu'une reine est déjà dans cette colonne. Retournez `false`.
    *   **Vérification des diagonales :** Parcourez les lignes précédentes (`i` de `0` à `row-1`).
        *   Si `abs(board[i] - col) == abs(i - row)`, cela signifie qu'une reine est sur une diagonale. Retournez `false`.
    *   Si aucune condition d'attaque n'est remplie, retournez `true`.

2.  **Fonction `printSolution(int board[], int N)` :**
    *   Affiche une solution trouvée de manière lisible. Par exemple, un échiquier avec des 'Q' pour les reines et des '.' pour les cases vides.

3.  **Fonction `solveNQueens(int board[], int row, int N)` :**
    *   C'est la fonction récursive qui implémente le Backtracking.
    *   `row` est la ligne actuelle où nous essayons de placer une reine.
    *   **Cas de base :** Si `row == N`, toutes les reines ont été placées avec succès. Appelez `printSolution` pour afficher cette configuration, puis retournez.
    *   **Étape récursive :**
        *   Pour chaque colonne `col` de `0` à `N-1` :
            *   Vérifiez si placer une reine à `(row, col)` est sûr en utilisant `isSafe`.
            *   Si c'est sûr :
                *   Placez la reine : `board[row] = col;`
                *   Appelez récursivement `solveNQueens(board, row + 1, N)` pour placer la reine suivante.
                *   **Backtracking :** Après l'appel récursif, il n'est pas strictement nécessaire de "retirer" la reine de `board[row]` car la prochaine itération de la boucle `for` ou le retour de la fonction écrasera cette valeur ou ignorera cette branche. Cependant, dans des problèmes plus complexes, il est souvent explicite de "défaire" le choix. Ici, le simple fait de passer à la colonne suivante dans la boucle `for` gère le backtracking.

4.  **Fonction `main()` :**
    *   Définissez `N = 4`.
    *   Déclarez un tableau `int board[N]`.
    *   Appelez `solveNQueens(board, 0, N)` pour commencer à placer les reines à partir de la ligne 0.

**Conseils :**
*   Le problème des 4-Dames a 2 solutions uniques. Votre programme devrait les trouver et les afficher.
*   La fonction `abs()` de `stdlib.h` peut être utile pour la vérification des diagonales.

---

### **Conseils pour l'utilisation de l'IA (ChatGPT, Copilot, etc.)**

L'utilisation d'outils d'IA est une réalité et une compétence en soi. Voici comment les exploiter au mieux pour ce TP :

1.  **Compréhension des concepts :** Si un concept (récursivité, Divide & Conquer, Backtracking) vous échappe, demandez à l'IA des explications, des exemples simplifiés ou des analogies.
2.  **Aide au débogage :** Si votre code ne fonctionne pas, copiez-le et demandez à l'IA de l'analyser, de pointer les erreurs potentielles ou de suggérer des tests.
3.  **Génération de cas de test :** Demandez à l'IA de vous fournir des jeux de données de test pour vos fonctions (par exemple, des tableaux pour `mergeSort`, des configurations de `board` pour `isSafe`).
4.  **Exploration d'alternatives :** Une fois que vous avez votre propre solution, vous pouvez demander à l'IA d'autres façons d'implémenter une partie du code (par exemple, une autre approche pour la fusion dans `mergeSort`), mais *toujours après* avoir implémenté la vôtre.
5.  **Ne demandez pas la solution complète d'emblée :** L'objectif est d'apprendre en codant. Si vous demandez directement la solution, vous manquerez l'opportunité d'acquérir les compétences. Utilisez l'IA comme un assistant, pas comme un substitut à votre réflexion.
6.  **Posez des questions "Pourquoi ?" :** Au lieu de "Donne-moi le code", demandez "Pourquoi cette approche est-elle meilleure ?" ou "Pourquoi cette ligne de code est-elle nécessaire ?".

---

### **Pour aller plus loin (Optionnel)**

*   **Merge Sort :**
    *   Implémentez une version "in-place" de la fonction `merge` (plus complexe, mais évite l'allocation de mémoire temporaire).
    *   Analysez la complexité temporelle et spatiale de votre implémentation.
*   **N-Dames :**
    *   Modifiez votre code pour qu'il compte le nombre total de solutions pour N=4, sans les afficher.
    *   Adaptez le code pour N=8. Combien de solutions trouvez-vous ?
    *   Implémentez une interface graphique simple (avec une bibliothèque comme SDL ou ncurses) pour visualiser les solutions.

---

Bon courage pour ce TP ! 