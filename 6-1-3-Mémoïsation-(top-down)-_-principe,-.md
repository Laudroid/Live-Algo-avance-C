# Cours Avancé en Algorithmique — Séance 6 : Programmation Dynamique (1h)  
## Partie 1 : Théorie — Introduction à la Programmation Dynamique (0.5h)  
### Contenu : Mémoïsation (top-down) : principe, implémentation avec tableaux

---

## 1. Définition de la mémoïsation

La mémoïsation est une technique algorithmique qui consiste à **mémoriser les résultats de sous-problèmes calculés** afin d’éviter de les recalculer plusieurs fois. Elle fait partie intégrante de la programmation dynamique, adoptant une approche **top-down**, c’est-à-dire en résolvant le problème global en partant de l’état complet vers les sous-problèmes.

---

## 2. Principe général

- On écrit l’algorithme en récursion naturelle, exposant clairement la décomposition du problème.
- Avant de lancer le calcul d’un sous-problème, on vérifie s’il est déjà résolu (consultation dans une structure mémoire).
- Si oui, on renvoie le résultat mémorisé.
- Sinon, on calcule le résultat, puis on le stocke pour usage futur.

Cette approche évite la redondance de calculs caractéristiques de la **récursion simple**, particulièrement dans les cas de sous-problèmes chevauchants.

---

## 3. Structure typique d’un algorithme avec mémoïsation

```c
int fonctionMemo(int n, int memo[]) {
    if (memo[n] != -1)
        return memo[n];
    if (condition_terminaison)
        memo[n] = valeur_base;
    else
        memo[n] = combinaison_des_appels_recursifs;
    return memo[n];
}
```

- `memo[]` est typiquement un tableau initialisé à une valeur indiquant que le sous-problème n’est pas encore traité (souvent \(-1\)).
- La fonction appelle récursivement d’autres sous-problèmes.
- Chaque résultat est stocké dans `memo[n]` avant le retour.

---

## 4. Exemple concret : Calcul de Fibonacci avec mémoïsation

### Implémentation en C

```c
#include <stdio.h>

int fibMemo(int n, int memo[]) {
    if (memo[n] != -1)
        return memo[n];
    if (n <= 1)
        memo[n] = n;
    else
        memo[n] = fibMemo(n-1, memo) + fibMemo(n-2, memo);
    return memo[n];
}

int main() {
    int n = 10;
    int memo[11];
    for (int i = 0; i <= n; i++)
        memo[i] = -1; // Initialisation

    printf("Fibonacci(%d) = %d\n", n, fibMemo(n, memo));
    return 0;
}
```

---

## 5. Diagramme Mermaid illustrant l’approche top-down

```mermaid
graph TD
    A[Fib(5)] 
    A --> B[Fib(4)]
    A --> C[Fib(3)]
    B --> D[Fib(3)]
    B --> E[Fib(2)]
    C --> F[Fib(2)]
    C --> G[Fib(1)]

    click B href "#memoization"
    click C href "#memoization"
```

- Ici, Fib(3) et Fib(2) sont calculés une fois,
- Ensuite re-utilisés depuis différentes branches, évitant le recalcul.

---

## 6. Comparaison avec récursion simple

| Aspect             | Récursion simple               | Mémoïsation (top-down)               |
|--------------------|-------------------------------|------------------------------------|
| Complexité         | Exponentielle (ex: Fibonacci \(O(2^n)\)) | Polynomial (ex: Fibonacci \(O(n)\))    |
| Gestion des sous-problèmes | Calcul multiple des mêmes sous-problèmes | Calcul unique, mémorisation des résultats |
| Complexité spatiale | Usage mémoire réduit (pile récursive)    | Usage mémoire supplémentaire pour `memo[]` |

---

## 7. Extensibilité de la mémoïsation

- Peut être étendue à des sous-problèmes multidimensionnels (ex : matrice `memo[i][j]`).
- Utilisable aussi bien pour problèmes d’optimisation que pour comptage.
- Facilement adaptable à des langages de plus haut niveau avec des structures associatives (HashMap en Java, dictionnaire en Python).

---

## 8. Sources consultées

- [GeeksforGeeks — Memoization in Dynamic Programming](https://www.geeksforgeeks.org/memoization-in-dynamic-programming-set-1-introduction/)
- [Wikipedia — Memoization](https://en.wikipedia.org/wiki/Memoization)
- [Programiz — Dynamic Programming Tutorial](https://www.programiz.com/dsa/dynamic-programming)
- [TopCoder — Introduction to Dynamic Programming](https://www.topcoder.com/thrive/articles/Dynamic%20Programming)

---

La mémoïsation optimise la récursion en exploitant la persistance des résultats intermédiaires, permettant d’aborder des problèmes qui seraient inaccessibles par une récursion naïve, tout en conservant une structure algorithmique claire et naturelle.