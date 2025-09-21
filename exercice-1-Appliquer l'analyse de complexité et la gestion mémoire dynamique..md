Bonjour à toutes et à tous !

Ce TP est conçu pour renforcer votre compréhension et votre maîtrise de deux piliers fondamentaux en programmation C avancée : la gestion de la mémoire dynamique et l'analyse de la complexité algorithmique. Nous allons explorer ces concepts à travers un mini-projet concret, en mettant l'accent sur les bonnes pratiques et la détection d'erreurs courantes.

---

## TP #1: Maîtrise de la Mémoire Dynamique et Analyse de Complexité en C

### Objectifs du TP

À la fin de ce TP, vous serez capable de :
*   Allouer et désallouer de la mémoire dynamiquement pour des tableaux 1D et 2D (matrices) en C.
*   Implémenter des fonctions robustes gérant la mémoire dynamique avec `malloc` et `free`.
*   Analyser la complexité temporelle de vos fonctions.
*   Identifier, expliquer et corriger une fuite mémoire dans un code C donné.
*   Utiliser des outils de détection de fuites mémoire comme `Valgrind`.

### Contexte et Prérequis

Ce TP suppose une bonne connaissance des pointeurs en C, des boucles, des conditions et de la compilation de programmes C.

---

### Partie 1: Manipulation de Vecteurs Dynamiques (Tableaux 1D)

Nous allons commencer par gérer des tableaux 1D de manière dynamique, que nous appellerons "vecteurs".

1.  **Allocation d'un vecteur :**
    *   Écrire une fonction `int* creer_vecteur(int taille)` qui alloue dynamiquement un tableau d'entiers de `taille` éléments.
    *   La fonction doit retourner un pointeur vers le premier élément du tableau alloué, ou `NULL` en cas d'échec d'allocation.
    *   **Analyse de complexité :** Quelle est la complexité temporelle de cette fonction en fonction de `taille` ? Justifiez.

2.  **Remplissage d'un vecteur :**
    *   Écrire une fonction `void remplir_vecteur(int* vecteur, int taille)` qui remplit le vecteur avec des valeurs aléatoires (par exemple, entre 0 et 99).
    *   **Analyse de complexité :** Quelle est la complexité temporelle de cette fonction en fonction de `taille` ? Justifiez.

3.  **Affichage d'un vecteur :**
    *   Écrire une fonction `void afficher_vecteur(const int* vecteur, int taille)` qui affiche les éléments du vecteur.
    *   **Analyse de complexité :** Quelle est la complexité temporelle de cette fonction en fonction de `taille` ? Justifiez.

4.  **Libération d'un vecteur :**
    *   Écrire une fonction `void liberer_vecteur(int* vecteur)` qui libère la mémoire allouée pour le vecteur.
    *   **Analyse de complexité :** Quelle est la complexité temporelle de cette fonction ? Justifiez.

5.  **Fonction `main` de test :**
    *   Dans votre fonction `main`, testez ces fonctions :
        *   Demandez à l'utilisateur la taille du vecteur.
        *   Créez, remplissez, affichez et libérez un vecteur.
        *   Assurez-vous de gérer les cas d'échec d'allocation.

---

### Partie 2: Manipulation de Matrices Dynamiques (Tableaux 2D)

Nous allons maintenant étendre notre gestion mémoire aux tableaux 2D, ou "matrices", en utilisant l'approche du "tableau de pointeurs".

1.  **Allocation d'une matrice :**
    *   Écrire une fonction `int** creer_matrice(int lignes, int colonnes)` qui alloue dynamiquement une matrice d'entiers de `lignes` lignes et `colonnes` colonnes.
    *   Rappelez-vous qu'une matrice 2D dynamique en C est généralement un tableau de pointeurs, où chaque pointeur pointe vers une ligne (un tableau 1D).
    *   La fonction doit retourner un pointeur vers le tableau de pointeurs, ou `NULL` en cas d'échec d'allocation (pensez à libérer ce qui a déjà été alloué si une allocation intermédiaire échoue).
    *   **Analyse de complexité :** Quelle est la complexité temporelle de cette fonction en fonction de `lignes` et `colonnes` ? Justifiez.

2.  **Remplissage d'une matrice :**
    *   Écrire une fonction `void remplir_matrice(int** matrice, int lignes, int colonnes)` qui remplit la matrice avec des valeurs aléatoires.
    *   **Analyse de complexité :** Quelle est la complexité temporelle de cette fonction en fonction de `lignes` et `colonnes` ? Justifiez.

3.  **Affichage d'une matrice :**
    *   Écrire une fonction `void afficher_matrice(const int** matrice, int lignes, int colonnes)` qui affiche les éléments de la matrice.
    *   **Analyse de complexité :** Quelle est la complexité temporelle de cette fonction en fonction de `lignes` et `colonnes` ? Justifiez.

4.  **Addition de deux matrices :**
    *   Écrire une fonction `int** additionner_matrices(const int** mat1, const int** mat2, int lignes, int colonnes)` qui prend deux matrices de mêmes dimensions et retourne une *nouvelle* matrice contenant la somme des deux.
    *   La fonction doit allouer dynamiquement la matrice résultat.
    *   **Analyse de complexité :** Quelle est la complexité temporelle de cette fonction en fonction de `lignes` et `colonnes` ? Justifiez.

5.  **Libération d'une matrice :**
    *   Écrire une fonction `void liberer_matrice(int** matrice, int lignes)` qui libère toute la mémoire allouée pour la matrice.
    *   Attention à l'ordre de libération : d'abord les lignes, puis le tableau de pointeurs.
    *   **Analyse de complexité :** Quelle est la complexité temporelle de cette fonction en fonction de `lignes` ? Justifiez.

6.  **Fonction `main` de test :**
    *   Dans votre fonction `main`, testez ces fonctions :
        *   Demandez à l'utilisateur les dimensions des matrices.
        *   Créez deux matrices, remplissez-les, affichez-les.
        *   Effectuez leur addition, affichez la matrice résultat.
        *   Libérez *toutes* les matrices allouées.
        *   Assurez-vous de gérer les cas d'échec d'allocation.

---

### Partie 3: Détection et Correction de Fuite Mémoire

Voici un extrait de code C qui contient une fuite mémoire.

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

void fonction_avec_fuite(int n) {
    char* buffer = NULL;
    for (int i = 0; i < n; i++) {
        buffer = (char*) malloc(10 * sizeof(char)); // Alloue 10 octets
        if (buffer == NULL) {
            perror("Erreur d'allocation");
            return;
        }
        sprintf(buffer, "Test %d", i);
        printf("Buffer: %s\n", buffer);
        // Oubli de libération ici
    }
    // Et ici, que se passe-t-il si n > 0 ?
}

int main() {
    printf("Exécution de fonction_avec_fuite(3):\n");
    fonction_avec_fuite(3);
    printf("\nExécution de fonction_avec_fuite(1):\n");
    fonction_avec_fuite(1);
    return 0;
}
```

1.  **Identifier la fuite :**
    *   Compilez ce code.
    *   Exécutez-le avec `Valgrind` (ou un outil similaire si vous n'êtes pas sur Linux/macOS) :
        `gcc -g -Wall -o fuite fuite.c`
        `valgrind --leak-check=full ./fuite`
    *   Analysez la sortie de `Valgrind`. Où la fuite est-elle signalée ? Combien d'octets sont perdus ?

2.  **Expliquer la cause :**
    *   Décrivez précisément pourquoi ce code provoque une fuite mémoire. Expliquez le mécanisme de la fuite.

3.  **Corriger le code :**
    *   Modifiez la fonction `fonction_avec_fuite` pour qu'elle ne contienne plus de fuite mémoire.
    *   Assurez-vous que le programme fonctionne toujours comme prévu.

4.  **Vérifier la correction :**
    *   Recompilez et réexécutez votre code corrigé avec `Valgrind`.
    *   Confirmez que `Valgrind` ne signale plus de fuites mémoire.

---

### Conseils et Bonnes Pratiques

*   **Vérification de `malloc` :** Toujours vérifier si `malloc` retourne `NULL`. C'est crucial pour la robustesse de votre code.
*   **`free` systématique :** Chaque `malloc` doit avoir un `free` correspondant. C'est la règle d'or de la gestion mémoire dynamique.
*   **Ordre de `free` pour les 2D :** Pour une matrice allouée comme un tableau de pointeurs, libérez d'abord chaque ligne, puis le tableau de pointeurs lui-même.
*   **Utilisation de `const` :** Utilisez `const` pour les pointeurs passés en argument à des fonctions qui ne modifient pas les données pointées (ex: `afficher_vecteur`, `afficher_matrice`). Cela améliore la clarté et la sécurité du code.
*   **Outils :** `Valgrind` est votre meilleur ami pour la détection de fuites mémoire et d'autres erreurs d'accès mémoire. Apprenez à l'utiliser efficacement.
*   **Utilisation de l'IA :** L'utilisation d'outils d'IA (comme ChatGPT) est encouragée pour vous aider à comprendre des concepts, débugger, ou même générer des ébauches de code. Cependant, comprendre ce que vous faites reste primordial. Ne vous contentez pas de copier-coller ; analysez, adaptez et soyez capable d'expliquer chaque ligne de votre solution. C'est ainsi que vous apprendrez le plus.

---

### Rendu

Vous devrez rendre :
1.  Un fichier C (`.c`) contenant toutes les fonctions demandées pour les Parties 1 et 2, ainsi qu'une fonction `main` complète permettant de tester l'ensemble.
2.  Le fichier C corrigé de la Partie 3.
3.  Un court rapport (fichier texte ou PDF) incluant :
    *   Les analyses de complexité pour chaque fonction demandée.
    *   L'explication détaillée de la fuite mémoire dans le code initial de la Partie 3.
    *   La sortie de `Valgrind` avant et après correction pour la Partie 3.

Bon courage et amusez-vous bien !