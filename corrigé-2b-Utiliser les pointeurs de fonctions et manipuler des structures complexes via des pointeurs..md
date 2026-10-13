
# Solution 1 : Implémentation Modulaire et Complète

Cette solution organise le code en plusieurs fichiers pour une meilleure modularité et implémente toutes les parties du TP, y compris le bonus sur les opérations binaires et la réflexion sur la généricité.

## Structure des fichiers

*   `matrix.h` : Définitions de la structure `Matrix` et des prototypes des fonctions de gestion de matrice.
*   `matrix.c` : Implémentation des fonctions de gestion de matrice.
*   `operations.h` : Définitions des types de pointeurs de fonctions et prototypes des fonctions d'opération.
*   `operations.c` : Implémentation des fonctions d'opération (unaires et binaires).
*   `main.c` : Programme principal pour tester toutes les fonctionnalités.

## Fichier `matrix.h`



```c
#ifndef MATRIX_H
#define MATRIX_H

#include <stddef.h> // Pour size_t

// Structure pour représenter une matrice de doubles
typedef struct Matrix {
    int rows;           // Nombre de lignes
    int cols;           // Nombre de colonnes
    double** data;      // Pointeur vers un tableau de pointeurs de double (pour les lignes)
} Matrix;

/**
 * @brief Alloue dynamiquement une nouvelle matrice.
 * @param rows Le nombre de lignes.
 * @param cols Le nombre de colonnes.
 * @return Un pointeur vers la matrice allouée, ou NULL en cas d'échec d'allocation.
 */
Matrix* create_matrix(int rows, int cols);

/**
 * @brief Libère toute la mémoire allouée pour une matrice.
 * @param m Un pointeur vers la matrice à libérer.
 */
void free_matrix(Matrix* m);

/**
 * @brief Affiche le contenu d'une matrice de manière lisible.
 * @param m Un pointeur constant vers la matrice à afficher.
 */
void print_matrix(const Matrix* m);

/**
 * @brief Remplit une matrice avec des valeurs aléatoires entre 0.0 et 10.0.
 * @param m Un pointeur vers la matrice à remplir.
 */
void fill_matrix_random(Matrix* m);

#endif // MATRIX_H
```



## Fichier `matrix.c`



```c
#include "matrix.h"
#include <stdio.h>  // Pour printf, fprintf
#include <stdlib.h> // Pour malloc, free, rand
#include <time.h>   // Pour srand (si utilisé ici, mais préférable dans main)

// Implémentation de create_matrix
Matrix* create_matrix(int rows, int cols) {
    if (rows <= 0 || cols <= 0) {
        fprintf(stderr, "Erreur: Les dimensions de la matrice doivent être positives.\n");
        return NULL;
    }

    // 1. Allouer la structure Matrix elle-même
    Matrix* m = (Matrix*)malloc(sizeof(Matrix));
    if (m == NULL) {
        fprintf(stderr, "Erreur d'allocation mémoire pour la structure Matrix.\n");
        return NULL;
    }

    m->rows = rows;
    m->cols = cols;

    // 2. Allouer le tableau de pointeurs de lignes (double** data)
    m->data = (double**)malloc(rows * sizeof(double*));
    if (m->data == NULL) {
        fprintf(stderr, "Erreur d'allocation mémoire pour le tableau de pointeurs de lignes.\n");
        free(m); // Libérer la structure Matrix si l'allocation des lignes échoue
        return NULL;
    }

    // 3. Pour chaque ligne, allouer le tableau de doubles correspondant
    for (int i = 0; i < rows; i++) {
        m->data[i] = (double*)malloc(cols * sizeof(double));
        if (m->data[i] == NULL) {
            fprintf(stderr, "Erreur d'allocation mémoire pour la ligne %d.\n", i);
            // En cas d'échec, libérer toutes les lignes déjà allouées et le tableau de pointeurs
            for (int j = 0; j < i; j++) {
                free(m->data[j]);
            }
            free(m->data);
            free(m);
            return NULL;
        }
        // Initialiser les éléments de la ligne à 0.0
        for (int j = 0; j < cols; j++) {
            m->data[i][j] = 0.0;
        }
    }

    return m;
}

// Implémentation de free_matrix
void free_matrix(Matrix* m) {
    if (m == NULL) {
        return; // Rien à libérer
    }

    // Libérer chaque ligne
    for (int i = 0; i < m->rows; i++) {
        free(m->data[i]);
    }
    // Libérer le tableau de pointeurs de lignes
    free(m->data);
    // Libérer la structure Matrix elle-même
    free(m);
}

// Implémentation de print_matrix
void print_matrix(const Matrix* m) {
    if (m == NULL) {
        printf("Matrice NULL.\n");
        return;
    }
    printf("Matrice (%dx%d):\n", m->rows, m->cols);
    for (int i = 0; i < m->rows; i++) {
        for (int j = 0; j < m->cols; j++) {
            printf("%8.2f ", m->data[i][j]); // Formatage pour une meilleure lisibilité
        }
        printf("\n");
    }
}

// Implémentation de fill_matrix_random
void fill_matrix_random(Matrix* m) {
    if (m == NULL) {
        fprintf(stderr, "Erreur: Impossible de remplir une matrice NULL.\n");
        return;
    }
    for (int i = 0; i < m->rows; i++) {
        for (int j = 0; j < m->cols; j++) {
            // Génère des valeurs aléatoires entre 0.0 et 10.0
            m->data[i][j] = (double)rand() / RAND_MAX * 10.0;
        }
    }
}
```



## Fichier `operations.h`



```c
#ifndef OPERATIONS_H
#define OPERATIONS_H

#include "matrix.h" // Nécessaire pour la structure Matrix

// --- Partie 2: Callback pour Traitement Unitaire ---

// Définition du type de pointeur de fonction pour les opérations unaires sur matrice
typedef double (*UnaryMatrixOperation)(double value);

/**
 * @brief Applique une opération unaire à tous les éléments d'une matrice.
 * @param m Un pointeur vers la matrice à modifier.
 * @param op_func La fonction d'opération unaire à appliquer.
 */
void apply_unary_operation(Matrix* m, UnaryMatrixOperation op_func);

// Fonctions d'opération unaire
double square(double value);
double negate(double value);
double sinus(double value);

// --- Partie 3: Opérations binaires (Bonus) ---

// Définition du type de pointeur de fonction pour les opérations binaires sur matrice
typedef double (*BinaryMatrixOperation)(double value1, double value2);

/**
 * @brief Applique une opération binaire élément par élément entre deux matrices.
 * @param m1 Pointeur constant vers la première matrice opérande.
 * @param m2 Pointeur constant vers la deuxième matrice opérande.
 * @param result Pointeur vers la matrice où stocker le résultat (doit être déjà allouée).
 * @param op_func La fonction d'opération binaire à appliquer.
 * @return 0 en cas de succès, -1 en cas d'erreur (matrices NULL, dimensions incompatibles).
 */
int apply_binary_operation(const Matrix* m1, const Matrix* m2, Matrix* result, BinaryMatrixOperation op_func);

// Fonctions d'opération binaire
double add_values(double v1, double v2);
double multiply_values(double v1, double v2);

#endif // OPERATIONS_H
```



## Fichier `operations.c`



```c
#include "operations.h"
#include <stdio.h>  // Pour fprintf
#include <math.h>   // Pour sin

// --- Partie 2: Implémentation du Callback pour Traitement Unitaire ---

// Implémentation d'apply_unary_operation
void apply_unary_operation(Matrix* m, UnaryMatrixOperation op_func) {
    if (m == NULL) {
        fprintf(stderr, "Erreur: Impossible d'appliquer une opération à une matrice NULL.\n");
        return;
    }
    if (op_func == NULL) {
        fprintf(stderr, "Erreur: La fonction d'opération unaire est NULL.\n");
        return;
    }

    for (int i = 0; i < m->rows; i++) {
        for (int j = 0; j < m->cols; j++) {
            m->data[i][j] = op_func(m->data[i][j]); // Appelle la fonction de callback
        }
    }
}

// Implémentation des fonctions d'opération unaire
double square(double value) {
    return value * value;
}

double negate(double value) {
    return -value;
}

double sinus(double value) {
    return sin(value);
}

// --- Partie 3: Implémentation des Opérations binaires (Bonus) ---

// Implémentation d'apply_binary_operation
int apply_binary_operation(const Matrix* m1, const Matrix* m2, Matrix* result, BinaryMatrixOperation op_func) {
    if (m1 == NULL || m2 == NULL || result == NULL) {
        fprintf(stderr, "Erreur: Une ou plusieurs matrices sont NULL pour l'opération binaire.\n");
        return -1;
    }
    if (op_func == NULL) {
        fprintf(stderr, "Erreur: La fonction d'opération binaire est NULL.\n");
        return -1;
    }
    if (m1->rows != m2->rows || m1->cols != m2->cols ||
        m1->rows != result->rows || m1->cols != result->cols) {
        fprintf(stderr, "Erreur: Dimensions de matrices incompatibles pour l'opération binaire.\n");
        return -1;
    }

    for (int i = 0; i < m1->rows; i++) {
        for (int j = 0; j < m1->cols; j++) {
            result->data[i][j] = op_func(m1->data[i][j], m2->data[i][j]); // Appelle la fonction de callback
        }
    }
    return 0; // Succès
}

// Implémentation des fonctions d'opération binaire
double add_values(double v1, double v2) {
    return v1 + v2;
}

double multiply_values(double v1, double v2) {
    return v1 * v2;
}
```



## Fichier `main.c`



```c
#include <stdio.h>
#include <stdlib.h> // Pour srand, rand
#include <time.h>   // Pour time

#include "matrix.h"     // Inclut les fonctions de gestion de matrice
#include "operations.h" // Inclut les types de callbacks et les fonctions d'opération

int main() {
    // Initialisation du générateur de nombres aléatoires une seule fois
    srand(time(NULL));

    printf("--- Partie 1: Gestion des Matrices Dynamiques ---\n");

    // Création d'une matrice 3x4
    Matrix* m1 = create_matrix(3, 4);
    if (m1 == NULL) {
        fprintf(stderr, "Échec de création de m1. Fin du programme.\n");
        return EXIT_FAILURE;
    }
    print_matrix(m1); // Affiche la matrice initialisée à 0.0

    // Remplissage aléatoire et affichage
    fill_matrix_random(m1);
    printf("\nMatrice m1 après remplissage aléatoire:\n");
    print_matrix(m1);

    printf("\n--- Partie 2: Implémentation du Callback pour Traitement Unaire ---\n");

    // Application de la fonction square
    printf("\nApplication de 'square' à m1:\n");
    apply_unary_operation(m1, square);
    print_matrix(m1);

    // Application de la fonction negate
    printf("\nApplication de 'negate' à m1:\n");
    apply_unary_operation(m1, negate);
    print_matrix(m1);

    // Application de la fonction sinus
    printf("\nApplication de 'sinus' à m1:\n");
    apply_unary_operation(m1, sinus);
    print_matrix(m1);

    printf("\n--- Partie 3: Opérations binaires (Bonus) ---\n");

    // Création d'une deuxième matrice m2 et d'une matrice résultat m_res
    Matrix* m2 = create_matrix(3, 4);
    Matrix* m_res = create_matrix(3, 4);
    if (m2 == NULL || m_res == NULL) {
        fprintf(stderr, "Échec de création de m2 ou m_res. Libération et fin du programme.\n");
        free_matrix(m1);
        free_matrix(m2); // free(NULL) est sûr
        free_matrix(m_res); // free(NULL) est sûr
        return EXIT_FAILURE;
    }

    fill_matrix_random(m2);
    printf("\nMatrice m2 après remplissage aléatoire:\n");
    print_matrix(m2);

    // Application de l'opération binaire 'add_values'
    printf("\nApplication de 'add_values' (m1 + m2) dans m_res:\n");
    if (apply_binary_operation(m1, m2, m_res, add_values) == 0) {
        print_matrix(m_res);
    } else {
        fprintf(stderr, "Échec de l'opération d'addition.\n");
    }

    // Application de l'opération binaire 'multiply_values'
    printf("\nApplication de 'multiply_values' (m1 * m2) dans m_res:\n");
    if (apply_binary_operation(m1, m2, m_res, multiply_values) == 0) {
        print_matrix(m_res);
    } else {
        fprintf(stderr, "Échec de l'opération de multiplication.\n");
    }

    // Libération de toutes les matrices
    printf("\n--- Libération de la mémoire ---\n");
    free_matrix(m1);
    free_matrix(m2);
    free_matrix(m_res);
    printf("Toutes les matrices ont été libérées.\n");

    printf("\n--- Partie 3: Réflexion sur la généricité ---\n");
    printf("Voir les explications dans le rapport pour cette partie.\n");

    return EXIT_SUCCESS;
}
```



## Compilation

Pour compiler cette solution, utilisez la commande suivante :
`gcc -Wall -Wextra -Werror -std=c11 matrix.c operations.c main.c -o matrix_app -lm`
L'option `-lm` est nécessaire pour lier la bibliothèque mathématique (`sin`).

## Explications des Concepts Clés (Solution 1)

### Partie 1 : Gestion des Matrices Dynamiques

*   **`struct Matrix`**: Définit une structure pour encapsuler les propriétés d'une matrice (lignes, colonnes) et un pointeur `double** data`. Ce `double**` est la clé de l'allocation dynamique d'une matrice 2D en C : c'est un tableau de pointeurs, où chaque pointeur pointe vers le début d'une ligne (un tableau de `double`).
*   **`create_matrix(int rows, int cols)`**:
    *   Alloue d'abord la structure `Matrix` elle-même.
    *   Alloue ensuite un tableau de `rows` pointeurs de `double` (`m->data`). Chaque élément de ce tableau sera un pointeur vers une ligne de la matrice.
    *   Dans une boucle, alloue `cols` `double` pour chaque ligne (`m->data[i]`).
    *   Initialise tous les éléments à `0.0`.
    *   **Gestion des erreurs**: Chaque appel à `malloc` est vérifié. En cas d'échec, la fonction libère la mémoire déjà allouée pour éviter les fuites avant de retourner `NULL`.
*   **`free_matrix(Matrix* m)`**:
    *   Libère la mémoire dans l'ordre inverse de l'allocation : d'abord chaque ligne, puis le tableau de pointeurs de lignes, et enfin la structure `Matrix`.
    *   Gère le cas `m == NULL` pour éviter un crash.
*   **`print_matrix(const Matrix* m)`**: Parcourt la matrice et affiche ses éléments. L'utilisation de `const Matrix* m` indique que la fonction ne modifiera pas la matrice.
*   **`fill_matrix_random(Matrix* m)`**: Remplit la matrice avec des valeurs aléatoires. `srand(time(NULL))` est appelé une seule fois dans `main` pour initialiser le générateur de nombres aléatoires.

### Partie 2 : Implémentation du Callback pour Traitement Unitaire

*   **`typedef double (*UnaryMatrixOperation)(double value);`**: C'est la définition du pointeur de fonction. Elle crée un alias de type `UnaryMatrixOperation` pour toute fonction qui prend un `double` et retourne un `double`. Cela rend le code plus lisible et plus sûr.
*   **`apply_unary_operation(Matrix* m, UnaryMatrixOperation op_func)`**:
    *   Cette fonction est le cœur du mécanisme de callback. Elle prend la matrice et le pointeur de fonction `op_func` en paramètres.
    *   Elle parcourt tous les éléments de la matrice et, pour chaque élément, appelle `op_func` avec la valeur de l'élément, puis stocke le résultat.
    *   **Gestion des erreurs**: Vérifie si `m` ou `op_func` sont `NULL`.
*   **Fonctions d'opération (`square`, `negate`, `sinus`)**: Ce sont des fonctions ordinaires qui correspondent à la signature de `UnaryMatrixOperation`. Elles peuvent être passées à `apply_unary_operation` pour appliquer différentes transformations.

### Partie 3 : Extension et Réflexion (Challenge)

#### Opérations binaires (Bonus)

*   **`typedef double (*BinaryMatrixOperation)(double value1, double value2);`**: Similaire à `UnaryMatrixOperation`, mais pour des fonctions prenant deux `double`.
*   **`apply_binary_operation(const Matrix* m1, const Matrix* m2, Matrix* result, BinaryMatrixOperation op_func)`**:
    *   Prend deux matrices d'entrée (`m1`, `m2`) et une matrice `result` (déjà allouée).
    *   Vérifie les dimensions des matrices pour s'assurer qu'elles sont compatibles pour une opération élément par élément.
    *   Applique `op_func` à chaque paire d'éléments correspondants de `m1` et `m2`, stockant le résultat dans `result`.
*   **Fonctions d'opération binaire (`add_values`, `multiply_values`)**: Des fonctions simples qui correspondent à la signature de `BinaryMatrixOperation`.

#### Réflexion sur la généricité

Pour rendre la structure `Matrix` générique (capable de stocker `int`, `float`, ou des structures personnalisées) :

1.  **Modification de `Matrix`**:
    *   Le champ `double** data` devrait être remplacé par `void** data` (ou `void* data` si on alloue un bloc contigu). `void*` est un pointeur générique qui peut pointer vers n'importe quel type de données.
    *   Il faudrait ajouter un champ `size_t element_size` pour stocker la taille en octets d'un élément (par exemple, `sizeof(int)`, `sizeof(MyStruct)`).

2.  **Implications pour les fonctions de gestion de matrice**:
    *   `create_matrix`: Lors de l'allocation des lignes, il faudrait allouer `cols * element_size` octets pour chaque ligne. L'initialisation à `0.0` devrait être remplacée par `memset` pour initialiser à zéro octet par octet.
    *   `free_matrix`: Resterait similaire, car `free` fonctionne avec `void*`.
    *   `print_matrix`: Deviendrait beaucoup plus complexe. Elle ne saurait plus comment afficher un élément sans connaître son type. Il faudrait lui passer un pointeur de fonction de "print" spécifique au type (par exemple, `void (*print_element_func)(const void* element)`).
    *   `fill_matrix_random`: Deviendrait également complexe. Il faudrait lui passer un pointeur de fonction de "remplissage aléatoire" spécifique au type.

3.  **Implications pour les pointeurs de fonctions de callback**:
    *   Les `UnaryMatrixOperation` et `BinaryMatrixOperation` devraient également devenir génériques.
    *   Elles prendraient des `void*` comme paramètres et retourneraient un `void*`.
    *   Par exemple, `typedef void* (*GenericUnaryOperation)(void* element);`.
    *   À l'intérieur de ces fonctions, il faudrait caster les `void*` vers le type réel (par exemple, `*(double*)element`) pour effectuer l'opération, puis recaster le résultat en `void*`.
    *   Cela introduirait une complexité de gestion des types et des casts manuels, augmentant le risque d'erreurs si les types ne correspondent pas.

En résumé, rendre la matrice générique en C est possible avec `void*` et des pointeurs de fonctions pour les opérations spécifiques au type, mais cela déplace la vérification de type du compilateur vers le programmeur, augmentant la complexité et le risque d'erreurs à l'exécution. C'est pourquoi des langages comme C++ avec les templates ou d'autres langages avec des génériques intégrés sont souvent préférés pour ce genre de tâche.

---

# Solution 2 : Implémentation Monofichier avec Ajout d'une Opération Unaire

Cette solution regroupe tout le code dans un seul fichier `main.c` pour simplifier la compilation et ajoute une opération unaire supplémentaire pour la démonstration.

## Fichier `main.c`



```c
#include <stdio.h>
#include <stdlib.h> // Pour malloc, free, rand, srand, EXIT_SUCCESS, EXIT_FAILURE
#include <time.h>   // Pour time
#include <math.h>   // Pour sin, cos (nécessite -lm à la compilation)

// --- Partie 1 : Gestion des Matrices Dynamiques ---

// Définition de la structure Matrix
typedef struct Matrix {
    int rows;
    int cols;
    double** data;
} Matrix;

/**
 * @brief Alloue dynamiquement une nouvelle matrice.
 * @param rows Le nombre de lignes.
 * @param cols Le nombre de colonnes.
 * @return Un pointeur vers la matrice allouée, ou NULL en cas d'échec d'allocation.
 */
Matrix* create_matrix(int rows, int cols) {
    if (rows <= 0 || cols <= 0) {
        fprintf(stderr, "Erreur: Les dimensions de la matrice doivent être positives.\n");
        return NULL;
    }

    Matrix* m = (Matrix*)malloc(sizeof(Matrix));
    if (m == NULL) {
        fprintf(stderr, "Erreur d'allocation mémoire pour la structure Matrix.\n");
        return NULL;
    }

    m->rows = rows;
    m->cols = cols;

    m->data = (double**)malloc(rows * sizeof(double*));
    if (m->data == NULL) {
        fprintf(stderr, "Erreur d'allocation mémoire pour le tableau de pointeurs de lignes.\n");
        free(m);
        return NULL;
    }

    for (int i = 0; i < rows; i++) {
        m->data[i] = (double*)malloc(cols * sizeof(double));
        if (m->data[i] == NULL) {
            fprintf(stderr, "Erreur d'allocation mémoire pour la ligne %d.\n", i);
            for (int j = 0; j < i; j++) {
                free(m->data[j]);
            }
            free(m->data);
            free(m);
            return NULL;
        }
        for (int j = 0; j < cols; j++) {
            m->data[i][j] = 0.0;
        }
    }
    return m;
}

/**
 * @brief Libère toute la mémoire allouée pour une matrice.
 * @param m Un pointeur vers la matrice à libérer.
 */
void free_matrix(Matrix* m) {
    if (m == NULL) {
        return;
    }
    for (int i = 0; i < m->rows; i++) {
        free(m->data[i]);
    }
    free(m->data);
    free(m);
}

/**
 * @brief Affiche le contenu d'une matrice de manière lisible.
 * @param m Un pointeur constant vers la matrice à afficher.
 */
void print_matrix(const Matrix* m) {
    if (m == NULL) {
        printf("Matrice NULL.\n");
        return;
    }
    printf("Matrice (%dx%d):\n", m->rows, m->cols);
    for (int i = 0; i < m->rows; i++) {
        for (int j = 0; j < m->cols; j++) {
            printf("%8.2f ", m->data[i][j]);
        }
        printf("\n");
    }
}

/**
 * @brief Remplit une matrice avec des valeurs aléatoires entre 0.0 et 10.0.
 * @param m Un pointeur vers la matrice à remplir.
 */
void fill_matrix_random(Matrix* m) {
    if (m == NULL) {
        fprintf(stderr, "Erreur: Impossible de remplir une matrice NULL.\n");
        return;
    }
    for (int i = 0; i < m->rows; i++) {
        for (int j = 0; j < m->cols; j++) {
            m->data[i][j] = (double)rand() / RAND_MAX * 10.0;
        }
    }
}

// --- Partie 2 : Implémentation du Callback pour Traitement Unitaire ---

// Définition du type de pointeur de fonction pour les opérations unaires sur matrice
typedef double (*UnaryMatrixOperation)(double value);

/**
 * @brief Applique une opération unaire à tous les éléments d'une matrice.
 * @param m Un pointeur vers la matrice à modifier.
 * @param op_func La fonction d'opération unaire à appliquer.
 */
void apply_unary_operation(Matrix* m, UnaryMatrixOperation op_func) {
    if (m == NULL) {
        fprintf(stderr, "Erreur: Impossible d'appliquer une opération à une matrice NULL.\n");
        return;
    }
    if (op_func == NULL) {
        fprintf(stderr, "Erreur: La fonction d'opération unaire est NULL.\n");
        return;
    }

    for (int i = 0; i < m->rows; i++) {
        for (int j = 0; j < m->cols; j++) {
            m->data[i][j] = op_func(m->data[i][j]);
        }
    }
}

// Fonctions d'opération unaire
double square(double value) {
    return value * value;
}

double negate(double value) {
    return -value;
}

double sinus(double value) {
    return sin(value);
}

// Nouvelle fonction d'opération unaire : Cosinus
double cosinus(double value) {
    return cos(value);
}

// --- Partie 3 : Opérations binaires (Bonus) ---

// Définition du type de pointeur de fonction pour les opérations binaires sur matrice
typedef double (*BinaryMatrixOperation)(double value1, double value2);

/**
 * @brief Applique une opération binaire élément par élément entre deux matrices.
 * @param m1 Pointeur constant vers la première matrice opérande.
 * @param m2 Pointeur constant vers la deuxième matrice opérande.
 * @param result Pointeur vers la matrice où stocker le résultat (doit être déjà allouée).
 * @param op_func La fonction d'opération binaire à appliquer.
 * @return 0 en cas de succès, -1 en cas d'erreur (matrices NULL, dimensions incompatibles).
 */
int apply_binary_operation(const Matrix* m1, const Matrix* m2, Matrix* result, BinaryMatrixOperation op_func) {
    if (m1 == NULL || m2 == NULL || result == NULL) {
        fprintf(stderr, "Erreur: Une ou plusieurs matrices sont NULL pour l'opération binaire.\n");
        return -1;
    }
    if (op_func == NULL) {
        fprintf(stderr, "Erreur: La fonction d'opération binaire est NULL.\n");
        return -1;
    }
    if (m1->rows != m2->rows || m1->cols != m2->cols ||
        m1->rows != result->rows || m1->cols != result->cols) {
        fprintf(stderr, "Erreur: Dimensions de matrices incompatibles pour l'opération binaire.\n");
        return -1;
    }

    for (int i = 0; i < m1->rows; i++) {
        for (int j = 0; j < m1->cols; j++) {
            result->data[i][j] = op_func(m1->data[i][j], m2->data[i][j]);
        }
    }
    return 0;
}

// Fonctions d'opération binaire
double add_values(double v1, double v2) {
    return v1 + v2;
}

double multiply_values(double v1, double v2) {
    return v1 * v2;
}

int main() {
    srand(time(NULL));

    printf("--- Partie 1: Gestion des Matrices Dynamiques ---\n");

    Matrix* m1 = create_matrix(3, 4);
    if (m1 == NULL) return EXIT_FAILURE;
    print_matrix(m1);

    fill_matrix_random(m1);
    printf("\nMatrice m1 après remplissage aléatoire:\n");
    print_matrix(m1);

    printf("\n--- Partie 2: Implémentation du Callback pour Traitement Unitaire ---\n");

    printf("\nApplication de 'square' à m1:\n");
    apply_unary_operation(m1, square);
    print_matrix(m1);

    printf("\nApplication de 'negate' à m1:\n");
    apply_unary_operation(m1, negate);
    print_matrix(m1);

    printf("\nApplication de 'sinus' à m1:\n");
    apply_unary_operation(m1, sinus);
    print_matrix(m1);

    printf("\nApplication de 'cosinus' à m1:\n");
    apply_unary_operation(m1, cosinus); // Nouvelle opération unaire
    print_matrix(m1);

    printf("\n--- Partie 3: Opérations binaires (Bonus) ---\n");

    Matrix* m2 = create_matrix(3, 4);
    Matrix* m_res = create_matrix(3, 4);
    if (m2 == NULL || m_res == NULL) {
        free_matrix(m1); free_matrix(m2); free_matrix(m_res);
        return EXIT_FAILURE;
    }

    fill_matrix_random(m2);
    printf("\nMatrice m2 après remplissage aléatoire:\n");
    print_matrix(m2);

    printf("\nApplication de 'add_values' (m1 + m2) dans m_res:\n");
    if (apply_binary_operation(m1, m2, m_res, add_values) == 0) {
        print_matrix(m_res);
    } else {
        fprintf(stderr, "Échec de l'opération d'addition.\n");
    }

    printf("\nApplication de 'multiply_values' (m1 * m2) dans m_res:\n");
    if (apply_binary_operation(m1, m2, m_res, multiply_values) == 0) {
        print_matrix(m_res);
    } else {
        fprintf(stderr, "Échec de l'opération de multiplication.\n");
    }

    printf("\n--- Libération de la mémoire ---\n");
    free_matrix(m1);
    free_matrix(m2);
    free_matrix(m_res);
    printf("Toutes les matrices ont été libérées.\n");

    printf("\n--- Partie 3: Réflexion sur la généricité ---\n");
    printf("Voir les explications dans le rapport pour cette partie.\n");

    return EXIT_SUCCESS;
}
```



## Compilation

Pour compiler cette solution, utilisez la commande suivante :
`gcc -Wall -Wextra -Werror -std=c11 main.c -o matrix_app_single -lm`

## Explications des Concepts Clés (Solution 2)

Cette solution reprend les mêmes concepts fondamentaux que la Solution 1, mais les regroupe dans un seul fichier `main.c`.

### Partie 1 : Gestion des Matrices Dynamiques

*   **`struct Matrix`**: Identique à la Solution 1, elle définit la structure de la matrice.
*   **`create_matrix`, `free_matrix`, `print_matrix`, `fill_matrix_random`**: Ces fonctions sont implémentées de la même manière que dans la Solution 1, avec la même attention à l'allocation dynamique, la libération de mémoire et la gestion des erreurs.

### Partie 2 : Implémentation du Callback pour Traitement Unitaire

*   **`typedef double (*UnaryMatrixOperation)(double value);`**: La définition du pointeur de fonction est identique.
*   **`apply_unary_operation`**: Le mécanisme de callback est le même, permettant d'appliquer une fonction passée en paramètre à chaque élément de la matrice.
*   **Fonctions d'opération (`square`, `negate`, `sinus`, `cosinus`)**: Une fonction `cosinus` a été ajoutée pour démontrer la facilité d'extension du système avec de nouvelles opérations unaires sans modifier le code de `apply_unary_operation`.

### Partie 3 : Opérations binaires (Bonus)

*   **`typedef double (*BinaryMatrixOperation)(double value1, double value2);`**: La définition du pointeur de fonction binaire est identique.
*   **`apply_binary_operation`**: Cette fonction est implémentée de la même manière, gérant les matrices d'entrée, la matrice de résultat et les vérifications de dimensions.
*   **Fonctions d'opération binaire (`add_values`, `multiply_values`)**: Identiques à la Solution 1.

### Réflexion sur la généricité

La réflexion sur la généricité est la même que celle détaillée dans la Solution 1. L'approche avec `void*` et des pointeurs de fonctions pour les opérations spécifiques au type reste la méthode en C, avec ses compromis en termes de sécurité de type et de complexité.

### Avantages et Inconvénients de l'approche monofichier

*   **Avantages**:
    *   **Simplicité de compilation**: Une seule commande `gcc` suffit.
    *   **Facilité pour les petits projets/TP**: Moins de fichiers à gérer.
*   **Inconvénients**:
    *   **Manque de modularité**: Le code est moins organisé, plus difficile à lire et à maintenir pour des projets plus grands.
    *   **Moins de réutilisabilité**: Les fonctions ne sont pas facilement séparables pour être utilisées dans d'autres projets.
    *   **Conflits de noms**: Plus de risques de conflits si des fonctions ou variables globales ont le même nom.

Pour des projets réels, l'approche modulaire de la Solution 1 est fortement recommandée. Pour un TP d'apprentissage, l'approche monofichier peut être acceptable si l'objectif principal est de se concentrer sur les concepts sans se soucier de l'organisation du projet.

---