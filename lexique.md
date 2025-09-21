**Structures de données dynamiques avancées** : Structures de données dont la taille peut varier pendant l'exécution d'un programme.

**Paradigmées algorithmiques avancés** : Approches générales pour la conception d'algorithmes complexes, comme Diviser pour Régner, Backtracking et Programmation Dynamique.

**Complexité des algorithmes** : Mesure des ressources (temps, mémoire) nécessaires à l'exécution d'un algorithme en fonction de la taille des données.

**Optimiser les solutions** : Améliorer l'efficacité (temps, mémoire) des algorithmes.

**Gestion de la mémoire (pile, tas, pointeurs)** : Processus de gestion de l'allocation et de la libération de la mémoire vive, incluant la connaissance de la pile, du tas et l'utilisation des pointeurs.

**Pointeurs** : Variables qui stockent des adresses mémoire, permettant un accès direct aux données et une gestion dynamique de la mémoire.

**Notations (O, Ω, Θ)** : Notations mathématiques pour exprimer la complexité asymptotique des algorithmes (pire cas, meilleur cas, cas moyen).

**Analyse de cas (pire, meilleur, moyen)** : Évaluation de la complexité d'un algorithme dans différentes conditions d'entrée pour déterminer sa performance.

**Algorithmes récursifs** : Algorithmes qui se définissent en s'appelant eux-mêmes pour résoudre des sous-problèmes.

**Algorithmes itératifs** : Algorithmes qui utilisent des boucles pour résoudre des problèmes, sans appel récursif.

**Pile (stack)** : Zone de la mémoire gérée en LIFO (Last In, First Out), où sont stockées les variables locales et les appels de fonction.

**Tas (heap)** : Zone de la mémoire pour l'allocation dynamique de données dont la durée de vie n'est pas connue à la compilation.

**Pointeurs de fonctions** : Pointeurs qui stockent l'adresse d'une fonction, permettant de l'appeler indirectement ou de la passer en argument.

**`malloc`** : Fonction C pour allouer dynamiquement un bloc de mémoire sur le tas.

**`calloc`** : Fonction C pour allouer dynamiquement un bloc de mémoire sur le tas et l'initialiser à zéro.

**`realloc`** : Fonction C pour redimensionner un bloc de mémoire alloué dynamiquement.

**`free`** : Fonction C pour libérer un bloc de mémoire alloué dynamiquement.

**Fuites mémoire** : Erreurs où un programme alloue de la mémoire mais ne la libère jamais, entraînant une saturation progressive.

**Pointeurs sauvages** : Pointeurs qui pointent vers une adresse mémoire invalide ou libérée, pouvant causer des erreurs.

**C17, C23** : Versions récentes du standard du langage de programmation C, impactant potentiellement la gestion mémoire ou la sûreté du code.

**Listes doublement chaînées** : Structure de données dynamique où chaque nœud contient un pointeur vers le suivant et un vers le précédent.

**Listes circulaires** : Structure de données dynamique où le dernier nœud pointe vers le premier, formant une boucle.

**Tables de hachage** : Structure de données associant des clés à des valeurs en utilisant une fonction de hachage pour calculer un index.

**Principe du hachage** : Processus de transformation d'une clé d'entrée en une valeur de hachage (index) pour son stockage dans une table.

**Fonctions de hachage** : Fonctions qui prennent une clé et retournent un index dans une table de hachage.

**Collisions (tables de hachage)** : Situation où deux clés différentes sont mappées au même index par une fonction de hachage.

**Chaînage séparé** : Méthode de résolution des collisions où chaque entrée de la table de hachage est une liste chaînée.

**Adressage ouvert** : Famille de méthodes de résolution des collisions où tous les éléments sont stockés directement dans le tableau, cherchant un autre emplacement si besoin.

**Adressage linéaire** : Méthode d'adressage ouvert où l'on cherche le prochain emplacement vide consécutif en cas de collision.

**Adressage quadratique** : Méthode d'adressage ouvert où l'on cherche des emplacements vides en incrémentant l'index par des carrés successifs en cas de collision.

**Piles (structures de données)** : Structure de données de type LIFO (Last In, First Out) avec ajout et retrait par une seule extrémité.

**Files (structures de données)** : Structure de données de type FIFO (First In, First Out) avec ajout à une extrémité et retrait de l'autre.

**Arbres binaires de recherche (ABR)** : Arbre binaire où les clés du sous-arbre gauche d'un nœud sont inférieures à sa clé, et celles du droit sont supérieures.

**Nœuds (arbres)** : Éléments constitutifs d'un arbre, contenant une valeur et des pointeurs vers des enfants.

**Parcours d'arbres (infixe, préfixe, postfixe)** : Différentes manières de visiter tous les nœuds d'un arbre binaire.

**ABR dégénérés** : ABR dont la structure ressemble à une liste chaînée, entraînant une mauvaise performance.

**Équilibrage des arbres** : Processus de maintien d'une hauteur minimale pour un ABR afin de garantir des performances optimales.

**Arbres AVL** : Type d'arbre binaire de recherche auto-équilibré, où la différence de hauteur des sous-arbres d'un nœud est au maximum de 1.

**Facteur d'équilibre (AVL)** : Différence de hauteur entre le sous-arbre droit et le sous-arbre gauche d'un nœud dans un arbre AVL.

**Rotations (AVL)** : Opérations effectuées sur les nœuds d'un arbre AVL pour le rééquilibrer.

**Arbres Rouge-Noir** : Autre type d'arbre binaire de recherche auto-équilibré, utilisant des règles de coloration et des rotations.

**Graphes** : Structure de données non linéaire composée de sommets (nœuds) et d'arêtes (liens) qui les connectent.

**Sommets (graphes)** : Éléments individuels d'un graphe, également appelés nœuds.

**Arêtes (graphes)** : Liens ou connexions entre les sommets d'un graphe.

**Poids (graphes)** : Valeur associée à une arête d'un graphe, représentant un coût ou une distance.

**Graphes orientés** : Graphes où les arêtes ont une direction, allant d'un sommet à un autre.

**Graphes non-orientés** : Graphes où les arêtes n'ont pas de direction, permettant un parcours dans les deux sens.

**Matrice d'adjacence** : Représentation d'un graphe sous forme de tableau bidimensionnel indiquant les connexions entre sommets.

**Listes d'adjacence** : Représentation d'un graphe où chaque sommet est associé à une liste de ses voisins.

**Graphes denses** : Graphes où le nombre d'arêtes est proche du nombre maximal possible.

**Graphes creux** : Graphes où le nombre d'arêtes est significativement inférieur au nombre maximal possible.

**Breadth-First Search (BFS)** : Algorithme de parcours de graphe qui explore les voisins niveau par niveau, utilisant une file.

**Depth-First Search (DFS)** : Algorithme de parcours de graphe qui explore le plus loin possible le long d'une branche avant de revenir, utilisant une pile ou la récursivité.

**Algorithmes de chemins les plus courts** : Algorithmes visant à trouver le chemin avec le poids total minimal entre deux sommets d'un graphe.

**Arbres couvrants minimaux** : Sous-graphe d'un graphe connexe pondéré qui est un arbre et qui connecte tous les sommets avec le poids total d'arêtes minimal.

**Divide & Conquer (Diviser pour Régner)** : Paradigme algorithmique divisant un problème en sous-problèmes plus petits, résolus récursivement, puis combinés.

**Analyse des récurrences (Master Theorem)** : Technique mathématique pour résoudre les relations de récurrence des algorithmes Diviser pour Régner.

**Merge Sort** : Algorithme de tri Divide & Conquer qui divise, trie récursivement les moitiés, puis les fusionne.

**Quick Sort** : Algorithme de tri Divide & Conquer qui choisit un pivot, partitionne le tableau autour, puis trie récursivement.

**Pivot (Quick Sort)** : Élément choisi dans Quick Sort pour partitionner le tableau en sous-tableaux (inférieurs/supérieurs au pivot).

**Backtracking** : Paradigme algorithmique construisant des solutions pas à pas et revenant en arrière si une voie ne mène pas à un résultat valide.

**Problème des N-Dames** : Problème combinatoire de placement de N dames sur un échiquier sans qu'elles s'attaquent, résolu par backtracking.

**Résolution de labyrinthe** : Application du backtracking pour trouver un chemin à travers un labyrinthe.

**Sudoku** : Jeu de logique dont la résolution peut être abordée par des algorithmes de backtracking.

**Programmation dynamique** : Paradigme algorithmique résolvant des problèmes complexes en décomposant en sous-problèmes chevauchants et en stockant leurs résultats.

**Sous-problèmes chevauchants** : Caractéristique d'un problème où les mêmes sous-problèmes sont rencontrés et résolus plusieurs fois par une approche récursive simple.

**Sous-structure optimale** : Propriété d'un problème où la solution optimale globale est construite à partir des solutions optimales de ses sous-problèmes.

**Mémoïsation (top-down)** : Technique de programmation dynamique qui stocke les résultats des sous-problèmes (souvent récursivement) pour éviter les recalculs.

**Tabulation (bottom-up)** : Technique de programmation dynamique qui résout les sous-problèmes de manière itérative, des plus petits aux plus grands.

**Suite de Fibonacci optimisée** : Calcul de la suite de Fibonacci utilisant la programmation dynamique pour éviter les calculs redondants de la solution récursive naïve.

