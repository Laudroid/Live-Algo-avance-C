
### **TP : Graphes - Représentation par Listes d'Adjacence, Parcours BFS et DFS**

**Contexte :**
Les graphes sont des outils puissants pour modéliser des relations entre des entités. Que ce soit pour des réseaux sociaux, des cartes routières, des dépendances logicielles ou des systèmes de recommandation, comprendre et manipuler les graphes est une compétence clé. Ce TP vous propose d'implémenter une des représentations les plus courantes, la liste d'adjacence, et d'explorer un graphe à travers ses parcours en largeur et en profondeur.

**Objectifs Pédagogiques :**
*   Maîtriser la représentation d'un graphe non-orienté par liste d'adjacence en langage C.
*   Implémenter les algorithmes de parcours en largeur (BFS - Breadth-First Search).
*   Implémenter les algorithmes de parcours en profondeur (DFS - Depth-First Search).
*   Tester et valider ces implémentations sur un exemple concret.
*   Gérer la mémoire dynamique de manière appropriée pour les structures de graphe.

**Prérequis :**
*   Maîtrise du langage C (structures, pointeurs, allocation dynamique de mémoire, fonctions).
*   Connaissance des listes chaînées.
*   Notions de base sur les files (queues) et les piles (stacks).

**Consignes Générales (Utilisation de l'IA) :**
L'utilisation d'outils d'IA générative est permise et même encouragée pour vous aider à explorer des solutions, comprendre des concepts, débugger votre code ou obtenir des explications sur des parties complexes. Cependant, l'objectif principal reste votre *compréhension* et votre *capacité à expliquer* votre code. Ne vous contentez pas de copier-coller. Utilisez l'IA comme un assistant intelligent pour approfondir votre apprentissage, pas comme un substitut à votre réflexion. Soyez prêt à justifier vos choix et à expliquer le fonctionnement de votre implémentation.

---

### **Mini-Projet : Implémentation d'un Graphe et de ses Parcours**

Vous allez implémenter un module de gestion de graphes non-orientés en C.

**Étape 1 : Structure du Graphe (Représentation par Liste d'Adjacence)**

Un graphe sera représenté par un tableau de listes chaînées. Chaque indice du tableau correspondra à un sommet, et la liste chaînée à cet indice contiendra tous les sommets adjacents.

1.  **Définir la structure d'un nœud d'une liste d'adjacence :**
    ```c
    // Représente un nœud dans la liste d'adjacence
    typedef struct AdjListNode {
        int dest; // Sommet de destination de l'arête
        struct AdjListNode* next;
    } AdjListNode;
    ```

2.  **Définir la structure du graphe :**
    ```c
    // Représente le graphe lui-même
    typedef struct Graph {
        int numVertices; // Nombre total de sommets
        AdjListNode** adjLists; // Tableau de pointeurs vers les listes d'adjacence
    } Graph;
    ```

3.  **Implémenter les fonctions suivantes :**
    *   `AdjListNode* createNode(int dest)` : Crée un nouveau nœud pour la liste d'adjacence.
    *   `Graph* createGraph(int numVertices)` : Alloue et initialise un graphe avec `numVertices` sommets. Tous les pointeurs `adjLists` doivent être initialisés à `NULL`.
    *   `void addEdge(Graph* graph, int src, int dest)` : Ajoute une arête entre `src` et `dest`. Puisque le graphe est non-orienté, l'arête doit être ajoutée dans les deux sens (de `src` vers `dest` ET de `dest` vers `src`).
    *   `void printGraph(Graph* graph)` : Affiche le graphe sous forme de listes d'adjacence pour vérification.

**Étape 2 : Algorithme de Parcours en Largeur (BFS)**

Le BFS explore le graphe "couche par couche" à partir d'un sommet de départ. Il utilise une file (queue) pour gérer les sommets à visiter.

1.  **Implémenter une structure de file simple :**
    Vous pouvez utiliser un tableau circulaire ou une liste chaînée pour implémenter une file (`enqueue`, `dequeue`, `isEmpty`).

2.  **Implémenter la fonction `void BFS(Graph* graph, int startVertex)` :**
    *   Initialiser un tableau `visited` de booléens (ou entiers 0/1) pour marquer les sommets déjà visités.
    *   Créer une file et y ajouter le `startVertex`. Marquer `startVertex` comme visité.
    *   Tant que la file n'est pas vide :
        *   Retirer un sommet `currentVertex` de la file.
        *   Afficher `currentVertex`.
        *   Pour chaque voisin `neighbor` de `currentVertex` :
            *   Si `neighbor` n'a pas été visité, le marquer comme visité et l'ajouter à la file.

**Étape 3 : Algorithme de Parcours en Profondeur (DFS)**

Le DFS explore le graphe en s'enfonçant le plus loin possible avant de revenir sur ses pas. Il utilise une pile (stack) ou la récursivité pour gérer les sommets à visiter.

1.  **Implémenter la fonction `void DFS(Graph* graph, int startVertex)` :**
    *   Vous pouvez choisir une implémentation récursive (plus élégante) ou itérative (avec une pile explicite).
    *   Initialiser un tableau `visited` de booléens (ou entiers 0/1) pour marquer les sommets déjà visités.
    *   **Approche récursive :**
        *   Définir une fonction auxiliaire `void DFS_recursive(Graph* graph, int vertex, int* visited)` :
            *   Marquer `vertex` comme visité et l'afficher.
            *   Pour chaque voisin `neighbor` de `vertex` :
                *   Si `neighbor` n'a pas été visité, appeler `DFS_recursive(graph, neighbor, visited)`.
        *   La fonction `DFS` principale appellera `DFS_recursive` après avoir initialisé le tableau `visited`.
    *   **Approche itérative (avec pile) :**
        *   Implémenter une structure de pile simple (`push`, `pop`, `isEmpty`).
        *   Ajouter `startVertex` à la pile et le marquer comme visité.
        *   Tant que la pile n'est pas vide :
            *   Retirer un sommet `currentVertex` de la pile.
            *   Afficher `currentVertex`.
            *   Pour chaque voisin `neighbor` de `currentVertex` :
                *   Si `neighbor` n'a pas été visité, le marquer comme visité et l'ajouter à la pile.

**Étape 4 : Test sur un Exemple Simple**

Créez un fichier `main.c` pour tester vos implémentations.

1.  **Construire le graphe suivant :**
    *   5 sommets (numérotés de 0 à 4)
    *   Arêtes : (0,1), (0,3), (1,2), (1,4), (3,4)

    ```
        0 --- 1 --- 2
        |     |
        3 --- 4
    ```

2.  **Appeler `printGraph`** pour vérifier la structure.
3.  **Appeler `BFS(graph, 0)`** et observer l'ordre de parcours.
    *   *Exemple d'ordre attendu (peut varier légèrement selon l'ordre d'ajout des voisins) :* `0 1 3 2 4`
4.  **Appeler `DFS(graph, 0)`** et observer l'ordre de parcours.
    *   *Exemple d'ordre attendu (peut varier légèrement selon l'ordre d'ajout des voisins) :* `0 1 2 4 3`

**Étape 5 : Gestion de la Mémoire**

1.  **Implémenter la fonction `void freeGraph(Graph* graph)` :**
    *   Cette fonction doit libérer toute la mémoire allouée dynamiquement pour le graphe (les nœuds des listes d'adjacence, puis le tableau de pointeurs `adjLists`, et enfin la structure `Graph` elle-même).
    *   N'oubliez pas d'appeler cette fonction à la fin de votre `main` pour éviter les fuites de mémoire.

---

**Livrables :**
*   Un ou plusieurs fichiers `.c` (par exemple, `graph.c` pour les fonctions du graphe et `main.c` pour les tests).
*   Un fichier `.h` si vous avez séparé les déclarations.
*   Un `Makefile` simple pour compiler votre projet.

**Pour Aller Plus Loin (Bonus) :**
*   Modifier les fonctions BFS/DFS pour qu'elles puissent gérer des graphes déconnectés (c'est-à-dire, parcourir toutes les composantes connexes).
*   Ajouter la possibilité de représenter des graphes orientés (modifier `addEdge`).
*   Implémenter la détection de cycles dans le graphe en utilisant BFS ou DFS.
*   Mesurer le temps d'exécution des algorithmes pour des graphes de tailles différentes.

---

N'hésitez pas à poser des questions si vous rencontrez des difficultés. Bon courage et amusez-vous bien avec les graphes !