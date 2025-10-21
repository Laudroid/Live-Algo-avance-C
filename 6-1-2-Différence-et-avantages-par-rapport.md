# Cours Avancé en Algorithmique — Séance 6 : Programmation Dynamique (1h)  
## Partie 1 : Théorie — Introduction à la Programmation Dynamique (0.5h)  
### Contenu : Différence et avantages par rapport à la récursion simple (redondance des calculs)

---

## 1. Récursion simple : définition et limites

La récursion est une technique où une fonction s’appelle elle-même pour résoudre un problème plus petit. Bien que simple et intuitive, **la récursion naïve peut entraîner une explosion du nombre d’appels**.

### Exemple classique

Calcul de la suite de Fibonacci :

\[
F(n) = \begin{cases}
n & \text{si } n \leq 1 \\
F(n-1) + F(n-2) & \text{sinon}
\end{cases}
\]

Voici une implémentation récursive simple :

```c
int fib(int n) {
    if (n <= 1)
        return n;
    return fib(n-1) + fib(n-2);
}
```

### Problème majeur : **Redondance des calculs**

- De nombreux sous-problèmes sont recalculés plusieurs fois.
- L’arbre d’appels est exponentiel en taille (\(O(2^n)\)).
- Impact fort sur les performances.

---

## 2. Programmation Dynamique : éliminer la redondance

La programmation dynamique (PD) optimise la récursion en **mémorisant** les résultats des sous-problèmes dès qu’ils sont calculés, une technique appelée **mémoïsation**.

### Principe

- Stocker dans une table (tableau, dictionnaire) les résultats partiels déjà calculés.
- Avant d’appeler récursivement avec un argument donné, vérifier si le résultat existe déjà.
- Renvoi direct si le résultat est connu.

---

## 3. Exemple de Fibonacci avec mémoïsation

```c
int fibMemo(int n, int memo[]) {
    if (memo[n] != -1)
        return memo[n];
    if (n <= 1)
        memo[n] = n;
    else
        memo[n] = fibMemo(n-1, memo) + fibMemo(n-2, memo);
    return memo[n];
}
```

- Le tableau `memo` est initialisé à \(-1\),
- Chaque valeur calculée est stockée,
- Chaque sous-problème est résolu une seule fois.

**Complexité améliorée :** \(O(n)\).

---

## 4. Comparaison visuelle : arbre d’appel récursif

```mermaid
graph TD
    A[Fib(5)]
    A --> B[Fib(4)]
    A --> C[Fib(3)]
    B --> D[Fib(3)]
    B --> E[Fib(2)]
    C --> F[Fib(2)]
    C --> G[Fib(1)]
```

- Sans mémorisation, Fib(3) et Fib(2) sont recalculés plusieurs fois.
- Avec mémoïsation, ils sont calculés une seule fois.

---

## 5. Autres avantages de la programmation dynamique

- **Décomposition claire du problème :** facilite la compréhension.
- **Réduction drastique du temps d’exécution** pour problèmes avec sous-problèmes chevauchants.
- Parfois, permet aussi une **implémentation itérative**, appelée programmation dynamique par "bottom-up".

---

## 6. Résumé des différences

| Aspect                 | Récursion simple                           | Programmation Dynamique                       |
|------------------------|-------------------------------------------|----------------------------------------------|
| Répétition calculs     | Oui, calculs redondants fréquents          | Non, résultats mémorisés                      |
| Temps d’exécution      | Exponentiel pour certains problèmes        | Polynomial en général                          |
| Complexité             | Souvent impraticable sur grands cas        | Optimisation efficace des calculs interdépendants |
| Mémoire                | Faible (pile)                              | Nécessite une structure mémoire supplémentaire |

---

## 7. Sources consultées

- [GeeksforGeeks — Difference Between Recursion and Dynamic Programming](https://www.geeksforgeeks.org/difference-recursion-dynamic-programming/)
- [Wikipedia — Dynamic Programming](https://en.wikipedia.org/wiki/Dynamic_programming)
- [Programiz — Dynamic Programming Tutorial](https://www.programiz.com/dsa/dynamic-programming)
- [TopCoder Tutorial — Dynamic Programming vs Recursion](https://www.topcoder.com/thrive/articles/Dynamic%20Programming)

---

L’optimisation offerte par la programmation dynamique rend possible la résolution de problèmes complexes, dont la récursion pure serait inefficace à cause de la redondance des calculs. La mémoïsation ou la construction de solutions par "bottom-up" sont les clés de cette efficacité.