# Cours Avancé en Algorithmique — Séance 6 : Programmation Dynamique (1h)  
## Partie 1 : Théorie — Introduction à la Programmation Dynamique (0.5h)  
### Contenu : Exemples introductifs — Suite de Fibonacci optimisée

---

## 1. Contexte et intérêt

La suite de Fibonacci, définie récursivement par :

\[
F(n) = \begin{cases}
0 & n = 0 \\
1 & n = 1 \\
F(n-1) + F(n-2) & n > 1
\end{cases}
\]

est souvent utilisée pour introduire les concepts de programmation dynamique, car la définition récursive naïve génère une explosion exponentielle d’appels redondants.

---

## 2. Calcul naïf récursif : inefficacité

Code simple en C :

```c
int fib(int n) {
    if (n <= 1)
        return n;
    return fib(n-1) + fib(n-2);
}
```

- Complexité temporelle : \( O(2^n) \).
- Nombre important de calculs redondants (sous-problèmes chevauchants).

---

## 3. Programmation dynamique : optimisation

### 3.1 Mémoïsation (top-down)

Mémoriser les résultats des appels récursifs pour éviter le recalcul.

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

Initialisation :

```c
int memo[n+1];
for (int i = 0; i <= n; i++) memo[i] = -1;
```

---

### 3.2 Tabulation (bottom-up)

Construire la solution à partir des cas de base vers \(n\).

```c
int fibTab(int n) {
    if (n <= 1) return n;
    int fibTable[n+1];
    fibTable[0] = 0;
    fibTable[1] = 1;
    for (int i = 2; i <= n; i++) {
        fibTable[i] = fibTable[i-1] + fibTable[i-2];
    }
    return fibTable[n];
}
```

---

### 3.3 Optimisation supplémentaire : espace constant

Puisqu’on n’a besoin que de \(F(n-1)\) et \(F(n-2)\) à chaque étape, on peut réduire la mémoire utilisée :

```c
int fibOpt(int n) {
    if (n <= 1) return n;
    int prev2 = 0, prev1 = 1, curr;
    for (int i = 2; i <= n; i++) {
        curr = prev1 + prev2;
        prev2 = prev1;
        prev1 = curr;
    }
    return curr;
}
```

- Complexité temporelle : \(O(n)\).
- Complexité spatiale : \(O(1)\).

---

## 4. Diagramme Mermaid — comparaison des approches

```mermaid
graph TD
    FibNaive --> FibNaive1[Fib(n-1)]
    FibNaive --> FibNaive2[Fib(n-2)]
    FibNaive1 --> FibNaive3[Fib(n-2)] %% recalcul répétitif
    FibMemo --> FibMemo1[Fib(n-1)]
    FibMemo --> FibMemo2[Fib(n-2)]
    FibMemo1 --> memo[Réutilisation résultat]
    FibMemo2 --> memo

    style memo fill:#82e0aa,stroke:#2d862d,stroke-width:2px
```

- Récursion naïve effectue plusieurs fois les mêmes appels,
- Mémoïsation stocke et réutilise.

---

## 5. Conclusion

- La suite de Fibonacci illustre les concepts clés : chevauchement de sous-problèmes et sous-structure optimale.
- La mémoïsation et la tabulation transforment la complexité exponentielle en linéaire.
- L’optimisation de l’espace est possible en exploitant la dépendance locale (uniquement les deux derniers termes).

---

## 6. Sources consultées

- [GeeksforGeeks — Fibonacci Series](https://www.geeksforgeeks.org/program-for-nth-fibonacci-number/)
- [Programiz — Fibonacci Series in C](https://www.programiz.com/c-programming/examples/fibonacci-series)
- [Wikipedia — Fibonacci number](https://en.wikipedia.org/wiki/Fibonacci_number)
- [TopCoder — Introduction to Dynamic Programming](https://www.topcoder.com/thrive/articles/Dynamic%20Programming)

---

La gestion efficiente de la suite de Fibonacci par programmation dynamique est un exemple emblématique pour comprendre comment optimiser des algorithmes récursifs grâce à systématisation du calcul des sous-problèmes.