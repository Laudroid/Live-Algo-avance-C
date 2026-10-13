# Corrigé du QCM : Les bases du langage C (Niveau Bachelor 3)

Ce fichier contient les réponses officielles ainsi qu'une brève explication pour chaque question du QCM.

---

### Réponse 1 : B) 2 octets
* **Explication :** Selon la norme ISO C, la taille minimale garantie pour un `int` est de 2 octets (16 bits), bien qu'il soit de 4 octets sur la grande majorité des architectures modernes 32 et 64 bits.

### Réponse 2 : B) 2
* **Explication :** En C, la division de deux entiers (`5 / 2`) effectue une division entière. Le résultat est tronqué, donnant `2`.

### Réponse 3 : C) La valeur stockée dans `val` (soit 42)
* **Explication :** L'opérateur `*` est l'opérateur de déréférencement. Il permet d'accéder à la valeur pointée par l'adresse contenue dans le pointeur `p`.

### Réponse 4 : A) 9
* **Explication :** Les tableaux en C sont indicés à partir de 0. Un tableau de taille 10 possède donc des indices valides de `0` à `9`.

### Réponse 5 : C) Une variable dont la durée de vie s'étend sur toute l'exécution du programme, mais dont la visibilité reste limitée à la fonction
* **Explication :** Le mot-clé `static` à l'intérieur d'une fonction conserve sa valeur entre les appels successifs et persiste en mémoire statique, tout en restant invisible en dehors de la fonction.

### Réponse 6 : A) Elle s'exécute au moins une fois, car la condition est évaluée à la fin
* **Explication :** Contrairement au `while` classique (où la condition est testée avant le bloc), la boucle `do-while` exécute son corps une première fois avant de vérifier la condition de continuation.

### Réponse 7 : B) Définir une macro textuelle ou une constante de préprocesseur
* **Explication :** Le préprocesseur utilise `#define` pour effectuer de la substitution textuelle avant même que la compilation proprement dite ne commence.

### Réponse 8 : B) `fgets`
* **Explication :** `fgets` prend en paramètre la taille maximale du tampon, ce qui prévient les dépassements de mémoire (buffer overflows), contrairement à `gets` (retirée du standard) ou `scanf` sans limite de taille.

### Réponse 9 : B) 0001 (soit 1)
* **Explication :** En binaire, `5` s'écrit `0101` et `3` s'écrit `0011`. L'ET bit à bit (`&`) donne `0001`, soit `1`.

### Réponse 10 : B) Par valeur
* **Explication :** En C, les arguments passés aux fonctions sont toujours copiés (passage par valeur). Pour modifier la variable originale, il faut explicitement passer son adresse (pointeur).

### Réponse 11 : C) `p->age`
* **Explication :** L'opérateur flèche (`->`) est un sucre syntactique combinant le déréférencement et l'accès au membre d'une structure (`(*p).age`).

### Réponse 12 : A) `condition ? si_vrai : si_faux`
* **Explication :** C'est la structure classique de l'unique opérateur ternaire du langage C.

### Réponse 13 : B) Par un caractère nul `'\0'`
* **Explication :** Les chaînes de caractères en C sont des tableaux de caractères se terminant obligatoirement par un octet nul (`\0`, valeur ASCII 0).

### Réponse 14 : C) La compilation proprement dite
* **Explication :** Le compilateur traduit le code préprocessé en code assembleur ou directement en code objet binaire.

### Réponse 15 : B) 0
* **Explication :** Par défaut, si aucune valeur n'est spécifiée, le premier énumérateur d'une `enum` commence à 0, et les suivants s'incrémentent de 1.

### Réponse 16 : A) `int (*f)(int, int);`
* **Explication :** Les parenthèses autour de `*f` sont indispensables pour indiquer qu'il s'agit d'un pointeur vers une fonction et non d'une fonction retournant un pointeur (`int *f(int, int)`).

### Réponse 17 : B) La valeur pointée par `p` ne peut pas être modifiée via ce pointeur.
* **Explication :** `const int *p` (ou `int const *p`) protège la donnée pointée. Le pointeur lui-même peut en revanche changer de cible.

### Réponse 18 : B) Elle saute immédiatement à l'itération suivante en évaluant l'incrément/condition.
* **Explication :** `continue` interrompt l'itération en cours pour passer directement à la suivante, contrairement à `break` qui sort complètement de la boucle.

### Réponse 19 : C) 80 (si un `int` fait 4 octets)
* **Explication :** `sizeof` retourne la taille totale en octets de l'objet évalué. Pour un tableau statique de 20 entiers de 4 octets, cela fait $20 \times 4 = 80$ octets.

### Réponse 20 : C) $x = 6, y = 5$
* **Explication :** L'opérateur d'incrémentation postfixé (`x++`) renvoie d'abord la valeur actuelle de `x` (affectée à `y`, soit 5), puis incrémente `x` de 1 (qui devient 6).