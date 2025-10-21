# Cours Avancé en Algorithmique — Séance 6 : Programmation Dynamique (1h)  
## Partie 1 : Théorie — Introduction à la Programmation Dynamique (0.5h)  
### Contenu : Tabulation (bottom-up) : principe, implémentation avec tableaux

---

## 1. Principe de la tabulation (bottom-up)

La tabulation est une approche de programmation dynamique qui **résout explicitement les plus petits sous-problèmes en premier** et construit progressivement la solution finale. Cette méthode part "du bas" (bottom-up) et évite la récursion, remplaçant souvent la structure top-down par un simple parcours itératif.

---

## 2. Différence avec la mémoïsation (top-down)

| Caractéristique         | Mémoïsation (Top-down)               | Tabulation (Bottom-up)               |
|------------------------|------------------------------------|-----------------------------------|
| Mode d’exécution       | Récursion avec cache                | Itération avec tableau             |
| Ordre de résolution    | Sous-problèmes nécessaires à la demande | De petits sous-problèmes vers grands |
| Complexité mémorielle  | Peut souffrir du coût récursif      | Allocation et parcours contrôlés   |
| Facilite l’analyse     | Plus naturelle à écrire             | Plus facile à comprendre les dépendances |

---

## 3. Modèle générique d’un algorithme par tabulation

- Initialiser un tableau (souvent à une taille correspondant à la taille du problème).
- Initialiser les cas de base.
- Itérer sur les indices/cas en garantissant que les sous-problèmes nécessaires sont déjà résolus.
- Remplir le tableau progressivement.
- Retourner la solution stockée dans la cellule finale.

---

## 4. Exemple : Fibonacci (bottom-up)

### Algorithme

- On calcule \(F(0) = 0\), \(F(1) = 1\) en base,
- Puis on construit \(F(k) = F(k-1) + F(k-2)\) pour \(k\) de 2 à \(n\).

### Implémentation C

```c
int fib(int n) {
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

## 5. Diagramme Mermaid illustrant la tabulation

```mermaid
graph LR
    F0[Fib(0) = 0] --> F2[Fib(2)]
    F1[Fib(1) = 1] --> F2[Fib(2)]
    F2 --> F3[Fib(3)]
    F3 --> F4[Fib(4)]
    F4 --> F5[Fib(5)]
```

Chaque valeur est calculée uniquement après avoir calculé les dépendances (les deux précédentes).

---

## 6. Avantages et limites

### Avantages

- Pas de surcharge liée à la récursion (pile d’appels).
- Contrôle explicite sur l’ordre d’évaluation.
- Parfaite maîtrise mémoire (allouer tableau suffisant).
- Plus simple à optimiser dans certains langages ou environnements.

### Limites

- Nécessite de comprendre la **dépendance entre sous-problèmes**.
- Parfois moins intuitive que la récursion.
- Pour certains problèmes, moins flexible pour gestion cas particuliers.

---

## 7. Applications classiques utilisant tabulation

- Problème du sac à dos (Knapsack).
- Plus longue sous-séquence commune (LCS).
- Traversée de grille, chemins minimaux.
- Nombre de manières de faire un montant donné avec des pièces.

---

## 8. Sources consultées

- [GeeksforGeeks — Dynamic Programming Tabulation](https://www.geeksforgeeks.org/dynamic-programming/)
- [Wikipedia — Dynamic programming](https://en.wikipedia.org/wiki/Dynamic_programming)
- [Programiz — Tabulation in Dynamic Programming](https://www.programiz.com/dsa/dynamic-programming)
- [TopCoder Tutorial — Dynamic Programming](https://www.topcoder.com/thrive/articles/Dynamic%20Programming)

---

La tabulation est une méthode rigoureuse et efficace qui exploite pleinement les dépendances entre sous-problèmes, s’appuyant sur une construction ordonnée des solutions partielles et souvent privilégiée pour son exécution rapide et sa simplicité d’implémentation itérative.