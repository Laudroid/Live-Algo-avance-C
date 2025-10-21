
Indiquez vos prénom et nom :


---

## QCM — Langage C et Algorithmique 

### 🔹 Partie 1 — Structures de contrôle

**1.** Quelle est la valeur finale de `x` après l’exécution suivante ?

```c
int x = 5;
if (x > 2)
    x = x + 3;
else
    x = x - 2;
```

A. 3
B. 5
C. 8
D. 7

Votre réponse :

---

**2.** Que fait l’instruction `break` dans une boucle `for` ?
A. Termine le programme
B. Termine la boucle immédiatement
C. Passe à l’itération suivante
D. Ignore l’instruction suivante

Votre réponse :

---

**3.** Quelle est la différence entre `while` et `do...while` ?
A. Aucune
B. `while` exécute toujours au moins une fois
C. `do...while` teste la condition avant l’exécution
D. `do...while` teste la condition après l’exécution

Votre réponse :

---

**4.** Quelle est la sortie du code suivant ?

```c
int i = 0;
while (i < 3)
    printf("%d", i++);
```

A. 123
B. 012
C. 0123
D. 01

Votre réponse :

---

**5.** Quelle est la portée d’une variable déclarée à l’intérieur d’un bloc `{ ... }` ?
A. Globale
B. Limitée à la fonction
C. Limitée au bloc
D. Illimitée

Votre réponse :

---

### 🔹 Partie 2 — Fonctions

**6.** Quel est le prototype correct d’une fonction retournant un entier et recevant deux `float` ?
A. `int f(float a, float b);`
B. `float f(int a, int b);`
C. `void f(float a, float b);`
D. `int f(void);`

Votre réponse :

---

**7.** Quelle est la sortie du code suivant ?

```c
int f(int x) {
    return x * 2;
}

int main() {
    int a = 3;
    printf("%d", f(a + 1));
}
```

A. 6
B. 8
C. 4
D. 7

Votre réponse :

---

**8.** Une fonction récursive doit :
A. Ne jamais s’appeler elle-même
B. Contenir un cas d’arrêt
C. Utiliser des boucles
D. Être déclarée globale

Votre réponse :

---

**9.** Quelle est la différence entre passage par valeur et passage par adresse ?
A. Aucune différence
B. Le passage par adresse permet de modifier la variable originale
C. Le passage par valeur est plus rapide
D. Le passage par adresse ne fonctionne qu’avec des entiers

Votre réponse :

---

**10.** Que renvoie cette fonction ?

```c
int mystery(int n) {
    if (n == 0) return 0;
    return n + mystery(n - 1);
}
```

A. La somme de 1 à n
B. Toujours 0
C. n
D. n²

Votre réponse :

---

### 🔹 Partie 3 — Programmation en C

**11.** Quelle est la bonne manière d’allouer dynamiquement un tableau de 10 entiers ?
A. `int *t = malloc(10);`
B. `int *t = malloc(10 * sizeof(int));`
C. `int t[10];`
D. `int *t = calloc(10);`

Votre réponse :

---

**12.** Quelle librairie faut-il inclure pour utiliser `malloc` ?
A. `<stdio.h>`
B. `<stdlib.h>`
C. `<math.h>`
D. `<string.h>`

Votre réponse :

---

**13.** Quelle est la sortie ?

```c
int a = 5, b = 2;
printf("%d", a / b);
```

A. 2.5
B. 2
C. 3
D. 2.0

Votre réponse :

---

**14.** Que fait `free(ptr);` ?
A. Libère la mémoire allouée dynamiquement
B. Efface le contenu de la variable
C. Supprime le pointeur
D. Réinitialise la RAM

Votre réponse :

---

**15.** Que contient `*(p + 2)` si `p` pointe sur un tableau `int tab[] = {3,5,7,9};` ?
A. 3
B. 5
C. 7
D. 9

Votre réponse :

---

**16.** Quelle est la taille en octets d’un `int` sur la plupart des architectures 64 bits ?
A. 2
B. 4
C. 8
D. Variable

Votre réponse :

---

**17.** Quelle est la sortie du code ?

```c
char s[] = "Hello";
printf("%c", *(s + 1));
```

A. H
B. e
C. l
D. o

Votre réponse :

---

**18.** Quelle est la fonction utilisée pour copier une chaîne de caractères ?
A. `copy()`
B. `strcpy()`
C. `strcat()`
D. `memcpy()`

Votre réponse :

---

**19.** Que se passe-t-il si l’on oublie de faire `free()` après un `malloc()` ?
A. Rien
B. Le programme plante
C. Fuite mémoire
D. Réallocation automatique

Votre réponse :

---

**20.** Quelle est la différence entre `printf("%d", x);` et `scanf("%d", &x);` ?
A. `printf` lit une valeur, `scanf` affiche
B. `printf` affiche, `scanf` lit
C. Elles font la même chose
D. Aucune différence

Votre réponse :

---

### 🔹 Partie 4 — Algorithmique avancée (listes, hashage, arbres) (10 questions)

**21.** Dans une liste chaînée simple, chaque élément contient :
A. Un pointeur vers l’élément suivant
B. Un pointeur vers l’élément précédent
C. Deux pointeurs
D. Aucun pointeur

Votre réponse :

---

**22.** L’insertion en tête d’une liste chaînée est :
A. O(n)
B. O(log n)
C. O(1)
D. O(n²)

Votre réponse :

---

**23.** Quelle est la structure la plus adaptée pour implémenter une table de hachage ?
A. Liste chaînée
B. Tableau
C. Arbre binaire
D. Pile

Votre réponse :

---

**24.** Quelle propriété un arbre binaire de recherche (BST) doit-il respecter ?
A. Les valeurs de gauche sont inférieures à la racine, celles de droite supérieures
B. Les valeurs sont triées de manière aléatoire
C. Toutes les feuilles ont le même niveau
D. Chaque nœud a exactement deux enfants

Votre réponse :

---

**25.** Quelle est la complexité moyenne de la recherche dans un arbre binaire équilibré ?
A. O(1)
B. O(n)
C. O(log n)
D. O(n log n)

Votre réponse :

---

**26.** Quelle sera la sortie du code suivant manipulant une liste chaînée ?

```c
#include <stdio.h>
#include <stdlib.h>

typedef struct node {
    int val;
    struct node *next;
} Node;

int main() {
    Node *a = malloc(sizeof(Node));
    Node *b = malloc(sizeof(Node));
    Node *c = malloc(sizeof(Node));

    a->val = 1; a->next = b;
    b->val = 2; b->next = c;
    c->val = 3; c->next = NULL;

    Node *p = a;
    while (p) {
        printf("%d ", p->val);
        p = p->next->next;
    }
}
```

A. 1 2 3
B. 1 3
C. 1 2
D. Erreur de compilation

Votre réponse :

---

**27.** Quelle est la principale cause de collision dans une table de hachage ?
A. Trop de clés similaires
B. Mauvaise fonction de hachage
C. Trop d’éléments stockés
D. Toutes les réponses ci-dessus

Votre réponse :

---

**28.** Quel algorithme de parcours d’arbre affiche les nœuds dans l’ordre croissant pour un BST ?
A. Préfixe (preorder)
B. Postfixe (postorder)
C. Infixe (inorder)
D. Largeur d’abord (breadth-first)

Votre réponse :

---

**29.** Quelle est la complexité **dans le pire cas** de l’insertion dans un arbre binaire de recherche **non équilibré** ?
A. O(1)
B. O(log n)
C. O(n)
D. O(n log n)

Votre réponse :

---

**30.** Que fait le code suivant ?

```c
typedef struct node {
    int val;
    struct node *left;
    struct node *right;
} Node;

int height(Node *root) {
    if (root == NULL) return 0;
    int hl = height(root->left);
    int hr = height(root->right);
    return 1 + (hl > hr ? hl : hr);
}
```

A. Calcule le nombre total de nœuds
B. Calcule la hauteur de l’arbre
C. Calcule la somme des valeurs
D. Calcule le niveau du nœud racine

Votre réponse :

---

**31.** Que fait le code suivant sur une liste chaînée ?

```c
typedef struct node {
    int val;
    struct node *next;
} Node;

Node* reverse(Node* head) {
    Node *prev = NULL, *curr = head, *next = NULL;
    while (curr != NULL) {
        next = curr->next;
        curr->next = prev;
        prev = curr;
        curr = next;
    }
    return prev;
}
```

A. Trie la liste
B. Supprime le dernier élément
C. Inverse la liste
D. Copie la liste

Votre réponse :

---

**32.** Que fait cette fonction de hachage ?

```c
unsigned int hash(char *s) {
    unsigned int h = 0;
    while (*s)
        h = 31 * h + *s++;
    return h;
}
```

A. Calcule une somme simple des caractères
B. Utilise la méthode du polynomial rolling hash
C. Trie les caractères avant de calculer la valeur
D. Produit toujours la même valeur

Votre réponse :

---

**33.** Quelle sera la sortie du parcours infixe de l’arbre ci-dessous ?

```
      8
     / \
    3   10
   / \    \
  1   6    14
```

A. 8 3 1 6 10 14
B. 1 3 6 8 10 14
C. 8 10 14 6 3 1
D. 3 1 6 8 10 14

Votre réponse :

---

**34.** Quelle structure de données serait la plus efficace pour implémenter une **file (FIFO)** ?
A. Tableau statique
B. Liste chaînée avec pointeur de tête et de queue
C. Arbre binaire
D. Table de hachage

Votre réponse :

---

**35.** Que fait la fonction suivante sur un arbre binaire ?

```c
int countNodes(Node *root) {
    if (root == NULL) return 0;
    if (root->left == NULL && root->right == NULL) return 1;
    return countNodes(root->left) + countNodes(root->right);
}
```

A. Compte le nombre total de nœuds
B. Compte le nombre de feuilles
C. Calcule la hauteur
D. Affiche les feuilles

Votre réponse :

---


## 🔹 Partie 5 — Analyse de code 

---

**36.** Que fait le code suivant ?

```c
#include <stdio.h>

void mystery(int *p) {
    *p += 5;
}

int main() {
    int x = 10;
    mystery(&x);
    printf("%d\n", x);
    return 0;
}
```

A. Affiche 10
B. Affiche 5
C. Affiche 15
D. Erreur de compilation

Votre réponse :

---

**37.** Trouvez l’erreur dans le code suivant :

```c
#include <stdio.h>
#include <stdlib.h>

int main() {
    int *tab = malloc(5 * sizeof(int));
    for (int i = 0; i <= 5; i++)
        tab[i] = i * 2;
    free(tab);
    return 0;
}
```

A. Rien, le code est correct
B. `malloc` doit être remplacé par `calloc`
C. Le tableau est indexé hors limites
D. `free(tab)` doit être avant la boucle

Votre réponse :

---

**38.** Que va afficher ce programme ?

```c
#include <stdio.h>

int f(int n) {
    static int c = 0;
    c += n;
    return c;
}

int main() {
    printf("%d ", f(1));
    printf("%d ", f(2));
    printf("%d ", f(3));
    return 0;
}
```

A. 1 2 3
B. 1 3 6
C. 6 6 6
D. Erreur de compilation

Votre réponse :

---

**39.** Quelle sera la sortie du code suivant ?

```c
#include <stdio.h>
#include <string.h>

int main() {
    char s1[] = "Hello";
    char s2[] = "Hello";
    if (s1 == s2)
        printf("Same");
    else
        printf("Different");
    return 0;
}
```

A. Same
B. Different
C. Dépend du compilateur
D. Erreur de compilation

Votre réponse :

---

**40.** Que fait ce code ?

```c
#include <stdio.h>

void printArray(int *t, int n) {
    if (n == 0) return;
    printf("%d ", t[0]);
    printArray(t + 1, n - 1);
}

int main() {
    int tab[] = {1, 2, 3, 4};
    printArray(tab, 4);
    return 0;
}
```

A. Affiche `4 3 2 1`
B. Affiche `1 2 3 4`
C. Erreur de compilation (pointeurs invalides)
D. Boucle infinie

Votre réponse :

---

### 🔹 Partie 6 — Questions rédactionnelles (5 questions)

**41.** Expliquez le fonctionnement d’une fonction récursive avec un exemple concret (autre que la factorielle).

Votre réponse :

---

**42.** Décrivez le principe du hachage et citez une application concrète.

Votre réponse :

---

**43.** Écrivez le pseudo-code d’une fonction qui insère un élément en fin d’une liste chaînée simple.

Votre réponse :

---

**44.** Comparez les structures de données suivantes : tableau, liste chaînée, et arbre binaire (avantages / inconvénients).

Votre réponse :

---

**45.** Écrivez un programme C minimal qui lit une série d’entiers jusqu’à -1 et affiche leur moyenne.

Votre réponse :
