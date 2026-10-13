# QCM : Les bases du langage C (Niveau Bachelor 3 Génie Informatique)

Ce questionnaire évalue vos connaissances fondamentales en langage C (hors allocation dynamique de mémoire). Pour chaque question, une seule réponse est correcte.

---

### Question 1 : Types de données et tailles
Quelle est la taille minimale garantie (en octets) d'un type `int` selon la norme ISO du langage C ?
- A) 1 octet
- B) 2 octets
- C) 4 octets
- D) 8 octets

### Question 2 : Opérateurs et priorité
Quel est le résultat de l'évaluation de l'expression suivante en C : `int x = 5 / 2;` ?
- A) 2.5
- B) 2
- C) 3
- D) Erreur de compilation

### Question 3 : Pointeurs et adresses
Soit la déclaration `int val = 42; int *p = &val;`. Que représente l'expression `*p` ?
- A) L'adresse mémoire de la variable `val`
- B) L'adresse mémoire du pointeur `p`
- C) La valeur stockée dans `val` (soit 42)
- D) La taille en octets de la variable `val`

### Question 4 : Tableaux et indices
En C, si l'on déclare `int tab[10];`, quel est l'indice du dernier élément valide de ce tableau ?
- A) 9
- B) 10
- C) 11
- D) Dépendant de l'architecture

### Question 5 : Portée des variables
Qu'est-ce qu'une variable déclarée avec le mot-clé `static` à l'intérieur d'une fonction ?
- A) Une variable dont la valeur ne peut jamais être modifiée
- B) Une variable globale accessible dans tous les fichiers du projet
- C) Une variable dont la durée de vie s'étend sur toute l'exécution du programme, mais dont la visibilité reste limitée à la fonction
- D) Une variable stockée obligatoirement dans un registre du processeur

### Question 6 : Structures de contrôle
Quel est le comportement de la boucle `do-while` par rapport à la boucle `while` classique ?
- A) Elle s'exécute au moins une fois, car la condition est évaluée à la fin
- B) Elle ne peut pas contenir d'instruction `break`
- C) Elle est plus rapide à l'exécution grâce à des optimisations du compilateur
- D) Elle nécessite obligatoirement un compteur d'itération

### Question 7 : Préprocesseur et macros
Quelle est la principale utilisation de la directive `#define` dans un programme C ?
- A) Déclarer une fonction récursive
- B) Définir une macro textuelle ou une constante de préprocesseur
- C) Inclure des bibliothèques dynamiques à l'exécution
- D) Allouer de la mémoire sur la pile

### Question 8 : Entrées / Sorties standard
Quelle fonction de la bibliothèque standard `stdio.h` est sécurisée pour lire une chaîne de caractères sécurisée contre les dépassements de tampon (contrairement à `gets`) ?
- A) `scanf` avec le format `%s`
- B) `fgets`
- C) `read_string`
- D) `input`

### Question 9 : Manipulation de bits
Quel est le résultat de l'opération bit à bit `5 & 3` (en écriture binaire : `0101 & 0011`) ?
- A) 0111 (soit 7)
- B) 0001 (soit 1)
- C) 0101 (soit 5)
- D) 0000 (soit 0)

### Question 10 : Passage de paramètres
En C, par défaut, comment les arguments sont-ils passés aux fonctions ?
- A) Par référence
- B) Par valeur
- C) Par adresse
- D) Par pointeur constant

### Question 11 : Structures (struct)
Comment accède-t-on au champ `age` d'une variable `etudiant` de type `struct Etudiant` via un pointeur `p` pointant vers cette structure ?
- A) `p.age`
- B) `*p.age`
- C) `p->age`
- D) `&p.age`

### Question 12 : Opérateur ternaire
Quelle est la syntaxe correcte de l'opérateur conditionnel (ternaire) en C ?
- A) `condition ? si_vrai : si_faux`
- B) `condition : si_vrai ? si_faux`
- C) `si_vrai ? condition : si_faux`
- D) `(condition) [si_vrai] {si_faux}`

### Question 13 : Chaines de caractères
Comment le langage C identifie-t-il la fin d'une chaîne de caractères stockée dans un tableau de caractères ?
- A) Par un espace blanc `' '`
- B) Par un caractère nul `'\0'`
- C) Par un point-virgule `';'`
- D) Par la valeur `-1`

### Question 14 : Compilation et liaisons
Quelle étape de la compilation transforme le code source en code objet (fichiers `.o` ou `.obj`) ?
- A) Le préprocessing
- B) L'édition de liens (Linking)
- C) La compilation proprement dite
- D) L'exécution

### Question 15 : Types énumérés (enum)
Quelle est la valeur par défaut du premier élément d'une énumération `enum` si aucune valeur explicite n'est assignée ?
- A) -1
- B) 0
- C) 1
- D) Indéfinie (garbage value)

### Question 16 : Pointeurs de fonctions
Comment déclare-t-on correctement un pointeur vers une fonction prenant deux entiers en paramètres et retournant un entier ?
- A) `int (*f)(int, int);`
- B) `int *f(int, int);`
- C) `int f(int, int*);`
- D) `int (*f)(void);`

### Question 17 : Qualificateur const
Que signifie la déclaration `const int *p;` ?
- A) Le pointeur `p` ne peut pas être modifié pour pointer vers une autre adresse.
- B) La valeur pointée par `p` ne peut pas être modifiée via ce pointeur.
- C) Ni le pointeur ni la valeur pointée ne peuvent être modifiés.
- D) La variable est stockée en lecture seule dans le disque.

### Question 18 : Boucles et sauts
Quel est l'effet de l'instruction `continue` à l'intérieur d'une boucle `for` ?
- A) Elle arrête complètement la boucle et sort du bloc.
- B) Elle saute immédiatement à l'itération suivante en évaluant l'incrément/condition.
- C) Elle redémarre la fonction depuis le début.
- D) Elle provoque une erreur de compilation.

### Question 19 : Opérateur de taille
Que retourne l'opérateur `sizeof` appliqué à un tableau statique `int tab[20];` sur une architecture 64 bits ?
- A) 20
- B) 4 ou 8 (selon le pointeur)
- C) 80 (si un `int` fait 4 octets)
- D) 160

### Question 20 : Opérateurs d'incrémentation
Soit `int x = 5; int y = x++;`. Quelles sont les valeurs finales de `x` et `y` après cette instruction ?
- A) $x = 5, y = 5$
- B) $x = 6, y = 6$
- C) $x = 6, y = 5$
- D) $x = 5, y = 6$