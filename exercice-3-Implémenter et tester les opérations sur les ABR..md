Bonjour à toutes et à tous !

Ce TP est conçu pour vous immerger dans l'univers des Arbres Binaires de Recherche (ABR), une structure de données fondamentale en informatique. L'objectif n'est pas seulement d'écrire du code, mais aussi de comprendre le comportement de ces arbres face à différentes situations.

N'oubliez pas que l'utilisation d'outils d'IA générative est une ressource à votre disposition. L'important est de comprendre ce que vous produisez, de le valider et de l'adapter. C'est une compétence en soi.

---

## TP : Implémentation et Exploration des Arbres Binaires de Recherche (ABR) en C

### Objectifs du TP

*   Implémenter les opérations fondamentales d'un ABR (insertion, recherche, parcours) en langage C.
*   Analyser l'impact des séquences d'insertion sur la structure de l'arbre.
*   Développer une approche critique face à la génération de code et à sa validation.

### Contexte

Les Arbres Binaires de Recherche (ABR) sont des structures de données arborescentes qui organisent les données de manière hiérarchique. Leur propriété clé est que, pour chaque nœud, toutes les valeurs de son sous-arbre gauche sont inférieures à sa propre valeur, et toutes les valeurs de son sous-arbre droit sont supérieures. Cette propriété permet des opérations de recherche, d'insertion et de suppression efficaces en moyenne.

### Prérequis

*   Maîtrise du langage C (pointeurs, structures, fonctions récursives).
*   Connaissances de base sur les structures de données (listes chaînées, arbres).
*   Environnement de développement C (compilateur GCC/Clang, éditeur de texte ou IDE).

### Matériel

*   Un ordinateur avec un compilateur C installé.
*   Un éditeur de texte ou un environnement de développement intégré (IDE).
*   Accès à des outils d'IA générative (facultatif mais encouragé pour l'exploration et la comparaison).

### Travail à Réaliser

Votre tâche consiste à implémenter un ABR en C et à le tester rigoureusement.

#### Étape 1 : Définition de la structure de l'ABR

Commencez par définir la structure d'un nœud d'ABR. Chaque nœud doit contenir une valeur entière, un pointeur vers son enfant gauche et un pointeur vers son enfant droit.

```c
// Exemple de structure (à adapter si besoin)
typedef struct NoeudABR {
    int valeur;
    struct NoeudABR *gauche;
    struct NoeudABR *droite;
} NoeudABR;
```

#### Étape 2 : Implémentation des fonctions fondamentales

Implémentez les fonctions suivantes. Pensez à la récursivité, qui est souvent très élégante pour les opérations sur les arbres.

1.  **`NoeudABR* creerNoeud(int valeur)`**
    *   Crée et initialise un nouveau nœud avec la valeur donnée. Les pointeurs `gauche` et `droite` doivent être initialisés à `NULL`.

2.  **`NoeudABR* inserer(NoeudABR* racine, int valeur)`**
    *   Insère une nouvelle valeur dans l'ABR. Si la racine est `NULL`, le nouveau nœud devient la racine. Sinon, la fonction parcourt l'arbre pour trouver la position correcte en respectant la propriété des ABR.
    *   La fonction doit retourner la nouvelle racine de l'arbre (utile si l'arbre était vide au départ).

3.  **`NoeudABR* rechercher(NoeudABR* racine, int valeur)`**
    *   Recherche une valeur spécifique dans l'ABR.
    *   Retourne un pointeur vers le nœud contenant la valeur si elle est trouvée, `NULL` sinon.

4.  **`void parcoursInfixe(NoeudABR* racine)`**
    *   Effectue un parcours infixe (ou inorder) de l'arbre : gauche -> racine -> droite.
    *   Affiche les valeurs des nœuds visités. Ce parcours doit afficher les éléments dans l'ordre croissant.

5.  **`void parcoursPrefixe(NoeudABR* racine)`**
    *   Effectue un parcours préfixe (ou preorder) de l'arbre : racine -> gauche -> droite.
    *   Affiche les valeurs des nœuds visités.

6.  **`void parcoursPostfixe(NoeudABR* racine)`**
    *   Effectue un parcours postfixe (ou postorder) de l'arbre : gauche -> droite -> racine.
    *   Affiche les valeurs des nœuds visités.

7.  **`void libererArbre(NoeudABR* racine)`**
    *   Libère toute la mémoire allouée pour l'arbre. C'est une étape cruciale pour éviter les fuites de mémoire. Un parcours postfixe est souvent utilisé pour cela.

#### Étape 3 : Programme principal et tests

Dans votre fonction `main`, créez un ABR et testez vos fonctions avec différentes séquences d'insertion.

Pour chaque séquence :
*   Insérez les éléments.
*   Affichez l'arbre en utilisant les trois types de parcours.
*   Recherchez au moins deux éléments existants et deux éléments non existants.
*   Appelez la fonction `libererArbre` à la fin.

**Séquences de test :**

1.  **Séquence "équilibrée" :** `50, 30, 70, 20, 40, 60, 80`
    *   *Question :* Décrivez la forme générale de l'arbre obtenu. Quelle est sa hauteur ?

2.  **Séquence "dégénérée" (croissante) :** `10, 20, 30, 40, 50, 60, 70`
    *   *Question :* Décrivez la forme de cet arbre. À quelle autre structure de données cela vous fait-il penser ? Quelle serait la complexité de la recherche dans ce cas ?

3.  **Séquence "dégénérée" (décroissante) :** `70, 60, 50, 40, 30, 20, 10`
    *   *Question :* Comparez la forme de cet arbre avec la séquence précédente.

4.  **Séquence aléatoire :** Générez 10 à 15 nombres entiers aléatoires (par exemple, entre 1 et 100) et insérez-les.
    *   *Question :* Quelle est la forme de cet arbre par rapport aux séquences précédentes ? Est-il plus proche de la séquence équilibrée ou dégénérée ?

#### Étape 4 : Réflexion sur l'utilisation de l'IA (si applicable)

Si vous avez utilisé un outil d'IA générative pour vous aider dans ce TP :

*   Décrivez votre démarche : quelles requêtes avez-vous formulées ?
*   Quels ont été les avantages et les inconvénients de cette approche ?
*   Avez-vous dû corriger, adapter ou améliorer le code généré ? Si oui, pourquoi et comment ?
*   Comment avez-vous validé la correction du code obtenu (qu'il soit généré par IA ou écrit par vous) ?

### Rendu

Vous devrez soumettre les éléments suivants :

1.  Un fichier `main.c` contenant l'intégralité de votre code source. Le code doit être clair, bien structuré et commenté.
2.  Un fichier `RAPPORT.md` (ou `.txt`) qui inclura :
    *   Les réponses aux questions posées dans l'Étape 3 pour chaque séquence de test.
    *   Les sorties complètes de votre programme pour chaque séquence de test (affichages des parcours, résultats des recherches).
    *   Les réponses aux questions de l'Étape 4 concernant l'utilisation de l'IA.

### Pour aller plus loin (Bonus)

Si vous avez terminé le travail principal et que vous souhaitez explorer davantage :

*   **Implémenter la suppression :** La suppression d'un nœud dans un ABR est l'opération la plus complexe.
*   **Calcul de la hauteur :** Écrivez une fonction qui calcule et retourne la hauteur de l'ABR.
*   **Implémentation itérative :** Tentez d'implémenter la fonction `inserer` ou `rechercher` de manière itérative plutôt que récursive.
*   **Visualisation :** Si vous êtes à l'aise, explorez des outils ou des techniques pour visualiser la structure de l'arbre (même de manière textuelle simple).

---

Amusez-vous bien avec ce TP. C'est une excellente occasion de solidifier vos compétences en C et votre compréhension des structures de données ! N'hésitez pas à expérimenter et à poser des questions.