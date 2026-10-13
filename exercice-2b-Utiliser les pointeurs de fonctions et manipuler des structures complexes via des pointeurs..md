
### **TP: Implémentation d'un callback via pointeur de fonction, gestion de matrices dynamiques**

**Objectif du TP :** Utiliser les pointeurs de fonctions et manipuler des structures complexes via des pointeurs.

**Contexte et Problématique :**
Lorsque l'on travaille avec des structures de données comme les matrices, il est souvent nécessaire d'appliquer des opérations variées à leurs éléments (ex: mettre au carré, prendre le sinus, inverser le signe). Plutôt que d'écrire une fonction spécifique pour chaque opération, une approche plus élégante et générique consiste à passer l'opération elle-même en paramètre. C'est là qu'interviennent les pointeurs de fonctions, permettant d'implémenter des mécanismes de "callback". Parallèlement, pour des matrices de tailles variables, l'allocation dynamique est indispensable.

**Prérequis :**
*   Maîtrise des bases du langage C (types, boucles, conditions).
*   Compréhension des pointeurs simples et de l'allocation mémoire dynamique (`malloc`, `free`).
*   Notions sur les `struct`.

**Consignes Générales :**
*   Créez un projet C (un ou plusieurs fichiers `.c` et `.h` si vous le souhaitez pour une meilleure organisation).
*   Compilez votre code avec `gcc` en activant les avertissements (`-Wall -Wextra -Werror`).
*   L'utilisation d'outils d'IA pour vous aider dans la compréhension ou la génération de code est tout à fait permise et même encouragée. L'objectif est d'apprendre et de comprendre, pas de réinventer la roue ou de lutter inutilement. Cependant, assurez-vous de *comprendre* le code que vous soumettez et soyez capable de l'expliquer.
*   N'hésitez pas à poser des questions si un point n'est pas clair.

---

### **Partie 1 : Gestion des Matrices Dynamiques**

Nous allons commencer par définir une structure pour représenter une matrice et implémenter les fonctions de base pour sa gestion. Pour simplifier, nous travaillerons avec des matrices de `double`.

1.  **Définition de la structure `Matrix` :**
    *   Créez une structure `Matrix` qui contiendra :
        *   Le nombre de lignes (`rows`).
        *   Le nombre de colonnes (`cols`).
        *   Un pointeur vers un tableau de pointeurs de `double` (`double** data`) pour stocker les éléments de la matrice.

2.  **Fonction `create_matrix(int rows, int cols)` :**
    *   Cette fonction alloue dynamiquement une nouvelle matrice de `rows` lignes et `cols` colonnes.
    *   Elle doit allouer la structure `Matrix` elle-même.
    *   Elle doit allouer le tableau de pointeurs de lignes (`double** data`).
    *   Pour chaque ligne, elle doit allouer le tableau de `double` correspondant.
    *   Initialisez tous les éléments de la matrice à `0.0`.
    *   Retournez un pointeur vers la `Matrix` allouée.
    *   Gérez les cas d'erreur d'allocation (retournez `NULL` si une allocation échoue).

3.  **Fonction `free_matrix(Matrix* m)` :**
    *   Cette fonction libère toute la mémoire allouée pour la matrice pointée par `m`.
    *   Libérez d'abord chaque ligne, puis le tableau de pointeurs de lignes, et enfin la structure `Matrix` elle-même.
    *   Assurez-vous de gérer le cas où `m` est `NULL`.

4.  **Fonction `print_matrix(const Matrix* m)` :**
    *   Affiche le contenu de la matrice de manière lisible.
    *   Gérez le cas où `m` est `NULL`.

5.  **Fonction `fill_matrix_random(Matrix* m)` :**
    *   Remplit la matrice avec des valeurs aléatoires (par exemple, entre 0.0 et 10.0).
    *   Utilisez `rand()` et `srand()` (n'oubliez pas d'initialiser `srand` une seule fois au début de votre `main`).

6.  **Testez vos fonctions :**
    *   Dans votre `main`, créez une matrice de taille 3x4, remplissez-la aléatoirement, affichez-la, puis libérez-la.

---

### **Partie 2 : Implémentation du Callback pour Traitement Unitaire**

Maintenant, nous allons implémenter le mécanisme de callback pour appliquer des opérations génériques aux éléments de la matrice.

1.  **Définition du type de pointeur de fonction :**
    *   Créez un `typedef` pour un pointeur de fonction qui prend un `double` en paramètre et retourne un `double`. Nommez-le par exemple `UnaryMatrixOperation`.
        ```c
        // Exemple :
        // typedef double (*UnaryMatrixOperation)(double value);
        ```

2.  **Fonction `apply_unary_operation(Matrix* m, UnaryMatrixOperation op_func)` :**
    *   Cette fonction parcourt tous les éléments de la matrice `m`.
    *   Pour chaque élément, elle appelle la fonction `op_func` passée en paramètre avec la valeur de l'élément.
    *   Elle remplace l'ancienne valeur de l'élément par le résultat retourné par `op_func`.
    *   Gérez le cas où `m` est `NULL` ou `op_func` est `NULL`.

3.  **Implémentation de fonctions d'opération :**
    *   Créez au moins trois fonctions distinctes qui correspondent au type `UnaryMatrixOperation` :
        *   `double square(double value)` : Retourne le carré de la valeur.
        *   `double negate(double value)` : Retourne l'opposé de la valeur.
        *   `double sinus(double value)` : Retourne le sinus de la valeur (utilisez `sin` de `<math.h>`, n'oubliez pas de lier avec `-lm` à la compilation).

4.  **Testez votre callback :**
    *   Dans votre `main`, après avoir créé et rempli une matrice :
        *   Appliquez la fonction `square` à la matrice et affichez le résultat.
        *   Appliquez la fonction `negate` à la matrice (qui a déjà été modifiée par `square`) et affichez le résultat.
        *   Appliquez la fonction `sinus` et affichez.
        *   Assurez-vous de libérer la matrice à la fin.

---

### **Partie 3 : Extension et Réflexion (Challenge)**

Pour aller plus loin et consolider votre compréhension :

1.  **Opérations binaires (facultatif mais recommandé) :**
    *   Définissez un nouveau `typedef` pour un pointeur de fonction qui prend deux `double` en paramètres et retourne un `double` (ex: `BinaryMatrixOperation`).
    *   Implémentez une fonction `apply_binary_operation(const Matrix* m1, const Matrix* m2, Matrix* result, BinaryMatrixOperation op_func)` :
        *   Cette fonction prend deux matrices `m1` et `m2` en entrée, et une matrice `result` (déjà allouée) en sortie.
        *   Elle applique `op_func` élément par élément (`result[i][j] = op_func(m1[i][j], m2[i][j])`).
        *   Gérez les cas d'erreur (matrices `NULL`, dimensions incompatibles).
    *   Implémentez des fonctions d'opération binaire comme `add_values(double v1, double v2)` et `multiply_values(double v1, double v2)`.
    *   Testez ces fonctions.

2.  **Réflexion sur la généricité :**
    *   Actuellement, nos matrices ne stockent que des `double`. Comment pourriez-vous modifier la structure `Matrix` et les fonctions associées pour qu'elles puissent stocker n'importe quel type de données (par exemple, `int`, `float`, ou même des structures personnalisées) ?
    *   Quelles seraient les implications pour les pointeurs de fonctions de callback ? (Vous n'avez pas besoin de l'implémenter, juste d'y réfléchir et d'expliquer votre approche).

---

Bon courage pour ce TP ! Amusez-vous à explorer ces concepts qui sont au cœur de nombreuses bibliothèques C.