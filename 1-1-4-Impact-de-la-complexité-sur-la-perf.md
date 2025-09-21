# Cours Avancé en Algorithmique — Séance 1 : Rappels et Introduction avancée  
## Partie 1 : Théorie — Complexité Algorithmique (1h)  
### Contenu : Impact de la complexité sur la performance des grands jeux de données

---

## 1. Introduction

La complexité algorithmique se focalise sur la façon dont le temps d’exécution ou l’espace mémoire d’un algorithme évolue avec la taille des données d’entrée, notée en général \( n \). Cette section détaille comment la complexité influence les performances lorsque les jeux de données deviennent très volumineux.

---

## 2. Effets de la complexité en fonction de la taille des données

### 2.1 Notions clés

- **Complexité faible (ex. \( O(1), O(\log n), O(n) \))** : le temps d’exécution augmente lentement même pour des grandes données.
- **Complexité modérée (\( O(n \log n) \))** : acceptable pour des grandes tailles mais peut vite devenir lente si \( n \) est extrême.
- **Complexité élevée (\( O(n^2), O(2^n), O(n!) \))** : devient rapidement inapplicable en pratique, même pour des tailles modestes.

---

### 2.2 Exemples d’impact sur les performances

| Taille \( n \) | \( O(n) \) (ex: somme) | \( O(n^2) \) (ex: tri à bulles) | \( O(n \log n) \) (ex: tri fusion) |
|----------------|------------------------|---------------------------------|-----------------------------------|
| 1 000          | 1 000                  | 1 000 000                       | ~10 000                           |
| 1 000 000      | 1 000 000              | \(10^{12}\) (très lent)         | ~20 000 000                      |
| 1 000 000 000  | \(10^9\)               | \(10^{18}\) (impossible)         | ~30 000 000 000                  |

---

## 3. Illustration graphique

```mermaid
graph TD
    A["Taille des données ( n )] --> B["Temps pour ( O(n) )"]
    A --> C["Temps pour ( O(n log n) )"]
    A --> D["Temps pour ( O(n^2) )"]

    style B fill:#6f9,stroke:#333,stroke-width:2px
    style C fill:#f96,stroke:#333,stroke-width:2px
    style D fill:#f66,stroke:#333,stroke-width:2px
```

Ce diagramme reflète la croissance rapide du temps d’exécution pour des algorithmes quadratiques au fur et à mesure que \( n \) augmente.

---

## 4. Conséquences pratiques

### 4.1 Traitement Big Data

- Les algorithmes avec complexités quadratiques ou exponentielles sont inutilisables pour le Big Data.
- Les approches optimisées (par ex. algorithmes \( O(n \log n) \) ou linéaires) sont privilégiées.

### 4.2 Impact sur le choix des algorithmes

- Un tri à bulles (\( O(n^2) \)) fonctionne bien pour \( n \leq 10^3 \) mais devient inefficace pour \( n \geq 10^6 \).
- Tri fusion ou quicksort (\( O(n \log n) \)) restent viables pour des millions d’éléments.

### 4.3 Ressources matérielles

- Une complexité élevée génère une consommation mémoire et CPU démesurée, amplifiée sur de gros volumes.
- En temps réel ou systèmes embarqués, la complexité influe aussi sur la latence et la réactivité.

---

## 5. Exemple concret : Recherche dans une base non triée vs triée

- Recherche linéaire dans liste non triée : \( O(n) \)
- Recherche dichotomique dans liste triée : \( O(\log n) \)

Pour \( n = 10^9 \), la recherche dichotomique effectue environ 30 comparaisons contre 1 milliard pour la recherche linéaire, un écart énorme.

---

## 6. Diagramme avec exemples comparatifs

```mermaid
flowchart TB
    Start((Début))
    Start --> CheckSize{Taille des données (n)}
    CheckSize -->|Petit < 10^2| InsertionSort
    CheckSize -->|Moyen / Grand ≥ 10^2| MergeOrQuick
    CheckSize -->|Cas particuliers (entiers, clés bornées)| LinearSort

    InsertionSort([Tri par insertion: O(n^2)])
    MergeOrQuick([Tri fusion / quicksort: O(n log n)])
    LinearSort([Tri par comptage / radix: O(n)])

    style InsertionSort fill:#f66,stroke:#333,stroke-width:2px
    style MergeOrQuick fill:#f96,stroke:#333,stroke-width:2px
    style LinearSort fill:#6f9,stroke:#333,stroke-width:2px

```

---

## 7. Sources et lectures complémentaires

- [Big O Cheat Sheet - Complexity & Algorithm Analysis](https://www.bigocheatsheet.com/)
- [Geeks for Geeks - Importance of Algorithmic Complexity](https://www.geeksforgeeks.org/importance-of-algorithmic-complexity-in-software-development/)
- [Stack Overflow - How does algorithmic complexity affect performance](https://stackoverflow.com/questions/6690962/how-does-algorithmic-complexity-affect-performance)
- [Harvard CS50 Lecture - Algorithms and Complexity](https://cs50.harvard.edu/x/2020/notes/4/)
- [Wikipedia - Algorithmic complexity](https://en.wikipedia.org/wiki/Analysis_of_algorithms)

---

Cet exposé met en lumière pourquoi la complexité algorithmique n’est pas une simple abstraction mathématique, mais un facteur concret déterminant la faisabilité et les performances effectives sur des gros volumes de données, influençant directement le choix et la conception d’algorithmes dans les applications pratiques.
