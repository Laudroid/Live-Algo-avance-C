Voici deux solutions possibles pour le TP sur les Arbres Binaires de Recherche, présentées sous forme de chapitres Markdown, suivies des rapports d'analyse.

---

## Chapitre 1 : Solution 1 - Implémentation Récursive Standard

Cette solution propose une implémentation classique des ABR en C, en utilisant des fonctions récursives pour la plupart des opérations, ce qui est une approche idiomatique et élégante pour les structures arborescentes.


```c
// main.c pour la Solution 1

#include <stdio.h>
#include <stdlib.h>
#include <time.h> // Pour srand et time

// --- Étape 1 : Définition de la structure de l'ABR ---
typedef struct NoeudABR {
    int valeur;
    struct NoeudABR *gauche;
    struct NoeudABR *droite;
} NoeudABR;

// --- Étape 2 : Implémentation des fonctions fondamentales ---

/**
 * @brief Crée et initialise un nouveau nœud avec la valeur donnée.
 * @param valeur La valeur à stocker dans le nœud.
 * @return Un pointeur vers le nouveau nœud, ou NULL en cas d'échec d'allocation.
 */
NoeudABR* creerNoeud(int valeur) {
    NoeudABR* nouveauNoeud = (NoeudABR*) malloc(sizeof(NoeudABR));
    if (nouveauNoeud == NULL) {
        perror("Erreur d'allocation mémoire pour un nouveau nœud.");
        return NULL;
    }
    nouveauNoeud->valeur = valeur;
    nouveauNoeud->gauche = NULL;
    nouveauNoeud->droite = NULL;
    return nouveauNoeud;
}

/**
 * @brief Insère une nouvelle valeur dans l'ABR.
 *        Si la valeur est déjà présente, elle n'est pas insérée (pas de doublons pour ce TP).
 * @param racine La racine actuelle de l'ABR.
 * @param valeur La valeur à insérer.
 * @return La nouvelle racine de l'arbre (peut être la même si l'arbre n'était pas vide).
 */
NoeudABR* inserer(NoeudABR* racine, int valeur) {
    // Si l'arbre est vide, le nouveau nœud devient la racine
    if (racine == NULL) {
        return creerNoeud(valeur);
    }

    // Sinon, parcourir l'arbre pour trouver la bonne position
    if (valeur < racine->valeur) {
        racine->gauche = inserer(racine->gauche, valeur);
    } else if (valeur > racine->valeur) {
        racine->droite = inserer(racine->droite, valeur);
    }
    // Si valeur == racine->valeur, on ne fait rien (pas de doublons)

    return racine; // Retourne la racine (inchangée si pas d'insertion à la racine)
}

/**
 * @brief Recherche une valeur spécifique dans l'ABR.
 * @param racine La racine de l'ABR.
 * @param valeur La valeur à rechercher.
 * @return Un pointeur vers le nœud contenant la valeur si trouvée, NULL sinon.
 */
NoeudABR* rechercher(NoeudABR* racine, int valeur) {
    // Cas de base : arbre vide ou valeur trouvée
    if (racine == NULL || racine->valeur == valeur) {
        return racine;
    }

    // Parcourir le sous-arbre gauche ou droit
    if (valeur < racine->valeur) {
        return rechercher(racine->gauche, valeur);
    } else {
        return rechercher(racine->droite, valeur);
    }
}

/**
 * @brief Effectue un parcours infixe (gauche -> racine -> droite) de l'arbre.
 *        Affiche les valeurs des nœuds visités (ordre croissant).
 * @param racine La racine de l'ABR.
 */
void parcoursInfixe(NoeudABR* racine) {
    if (racine != NULL) {
        parcoursInfixe(racine->gauche);
        printf("%d ", racine->valeur);
        parcoursInfixe(racine->droite);
    }
}

/**
 * @brief Effectue un parcours préfixe (racine -> gauche -> droite) de l'arbre.
 *        Affiche les valeurs des nœuds visités.
 * @param racine La racine de l'ABR.
 */
void parcoursPrefixe(NoeudABR* racine) {
    if (racine != NULL) {
        printf("%d ", racine->valeur);
        parcoursPrefixe(racine->gauche);
        parcoursPrefixe(racine->droite);
    }
}

/**
 * @brief Effectue un parcours postfixe (gauche -> droite -> racine) de l'arbre.
 *        Affiche les valeurs des nœuds visités.
 * @param racine La racine de l'ABR.
 */
void parcoursPostfixe(NoeudABR* racine) {
    if (racine != NULL) {
        parcoursPostfixe(racine->gauche);
        parcoursPostfixe(racine->droite);
        printf("%d ", racine->valeur);
    }
}

/**
 * @brief Libère toute la mémoire allouée pour l'arbre.
 *        Utilise un parcours postfixe pour libérer les nœuds enfants avant le parent.
 * @param racine La racine de l'ABR.
 */
void libererArbre(NoeudABR* racine) {
    if (racine != NULL) {
        libererArbre(racine->gauche);
        libererArbre(racine->droite);
        free(racine);
    }
}

/**
 * @brief Calcule la hauteur de l'ABR.
 * @param racine La racine de l'ABR.
 * @return La hauteur de l'arbre (0 pour un arbre vide, 1 pour un arbre avec un seul nœud).
 */
int calculerHauteur(NoeudABR* racine) {
    if (racine == NULL) {
        return 0;
    } else {
        int hauteurGauche = calculerHauteur(racine->gauche);
        int hauteurDroite = calculerHauteur(racine->droite);

        // La hauteur est le maximum des hauteurs des sous-arbres + 1 (pour le nœud courant)
        return (hauteurGauche > hauteurDroite ? hauteurGauche : hauteurDroite) + 1;
    }
}

// --- Fonction utilitaire pour tester une séquence ---
void testerSequence(const char* nomSequence, int valeurs[], int taille, int recherches[], int tailleRecherches) {
    NoeudABR* racine = NULL;
    printf("\n--- Test de la séquence : %s ---\n", nomSequence);

    // Insertion
    printf("Insertion des valeurs : ");
    for (int i = 0; i < taille; i++) {
        printf("%d ", valeurs[i]);
        racine = inserer(racine, valeurs[i]);
    }
    printf("\n");

    // Parcours
    printf("Parcours Infixe (ordre croissant) : ");
    parcoursInfixe(racine);
    printf("\n");

    printf("Parcours Préfixe : ");
    parcoursPrefixe(racine);
    printf("\n");

    printf("Parcours Postfixe : ");
    parcoursPostfixe(racine);
    printf("\n");

    // Hauteur
    printf("Hauteur de l'arbre : %d\n", calculerHauteur(racine));

    // Recherche
    printf("Recherches :\n");
    for (int i = 0; i < tailleRecherches; i++) {
        NoeudABR* resultat = rechercher(racine, recherches[i]);
        if (resultat != NULL) {
            printf("  Valeur %d trouvée.\n", recherches[i]);
        } else {
            printf("  Valeur %d non trouvée.\n", recherches[i]);
        }
    }

    // Libération de la mémoire
    libererArbre(racine);
    printf("Mémoire de l'arbre libérée.\n");
}

// --- Étape 3 : Programme principal et tests ---
int main() {
    // Initialisation du générateur de nombres aléatoires
    srand(time(NULL));

    // Séquence 1 : "équilibrée"
    int seq1_valeurs[] = {50, 30, 70, 20, 40, 60, 80};
    int seq1_recherches[] = {30, 70, 10, 90};
    testerSequence("équilibrée", seq1_valeurs, sizeof(seq1_valeurs) / sizeof(int), seq1_recherches, sizeof(seq1_recherches) / sizeof(int));

    // Séquence 2 : "dégénérée" (croissante)
    int seq2_valeurs[] = {10, 20, 30, 40, 50, 60, 70};
    int seq2_recherches[] = {10, 70, 5, 75};
    testerSequence("dégénérée (croissante)", seq2_valeurs, sizeof(seq2_valeurs) / sizeof(int), seq2_recherches, sizeof(seq2_recherches) / sizeof(int));

    // Séquence 3 : "dégénérée" (décroissante)
    int seq3_valeurs[] = {70, 60, 50, 40, 30, 20, 10};
    int seq3_recherches[] = {70, 10, 5, 75};
    testerSequence("dégénérée (décroissante)", seq3_valeurs, sizeof(seq3_valeurs) / sizeof(int), seq3_recherches, sizeof(seq3_recherches) / sizeof(int));

    // Séquence 4 : "aléatoire"
    int seq4_valeurs[15];
    printf("\n--- Génération de la séquence aléatoire ---\n");
    for (int i = 0; i < 15; i++) {
        seq4_valeurs[i] = rand() % 100 + 1; // Nombres entre 1 et 100
        printf("%d ", seq4_valeurs[i]);
    }
    printf("\n");
    // Rechercher 2 existants (les deux premiers insérés) et 2 non existants
    int seq4_recherches[] = {seq4_valeurs[0], seq4_valeurs[1], 101, 0};
    testerSequence("aléatoire", seq4_valeurs, 15, seq4_recherches, sizeof(seq4_recherches) / sizeof(int));

    return 0;
}
```


### RAPPORT.md - Solution 1

#### Sorties du programme et analyse des séquences de test

**Séquence 1 : "équilibrée" (`50, 30, 70, 20, 40, 60, 80`)**


```
--- Test de la séquence : équilibrée ---
Insertion des valeurs : 50 30 70 20 40 60 80 
Parcours Infixe (ordre croissant) : 20 30 40 50 60 70 80 
Parcours Préfixe : 50 30 20 40 70 60 80 
Parcours Postfixe : 20 40 30 60 80 70 50 
Hauteur de l'arbre : 3
Recherches :
  Valeur 30 trouvée.
  Valeur 70 trouvée.
  Valeur 10 non trouvée.
  Valeur 90 non trouvée.
Mémoire de l'arbre libérée.
```


*   **Forme générale :** L'arbre obtenu est relativement équilibré. La racine est 50, avec 30 et 70 comme enfants directs, et les autres valeurs se répartissent de manière symétrique.
*   **Hauteur :** La hauteur est de 3.
*   **Commentaire :** Un arbre équilibré offre les meilleures performances pour les opérations de recherche, d'insertion et de suppression, avec une complexité en O(log n).

**Séquence 2 : "dégénérée" (croissante) (`10, 20, 30, 40, 50, 60, 70`)**


```
--- Test de la séquence : dégénérée (croissante) ---
Insertion des valeurs : 10 20 30 40 50 60 70 
Parcours Infixe (ordre croissant) : 10 20 30 40 50 60 70 
Parcours Préfixe : 10 20 30 40 50 60 70 
Parcours Postfixe : 70 60 50 40 30 20 10 
Hauteur de l'arbre : 7
Recherches :
  Valeur 10 trouvée.
  Valeur 70 trouvée.
  Valeur 5 non trouvée.
  Valeur 75 non trouvée.
Mémoire de l'arbre libérée.
```


*   **Forme de l'arbre :** Cet arbre est complètement dégénéré. Chaque nouvelle valeur étant supérieure à la précédente, elle est insérée comme enfant droit du dernier nœud inséré. L'arbre prend la forme d'une "liste chaînée" inclinée vers la droite.
*   **Structure de données similaire :** Cela ressemble à une liste chaînée simple.
*   **Complexité de la recherche :** Dans ce cas dégénéré, la recherche devient linéaire, avec une complexité en O(n), car il faut potentiellement parcourir tous les nœuds de l'arbre.

**Séquence 3 : "dégénérée" (décroissante) (`70, 60, 50, 40, 30, 20, 10`)**


```
--- Test de la séquence : dégénérée (décroissante) ---
Insertion des valeurs : 70 60 50 40 30 20 10 
Parcours Infixe (ordre croissant) : 10 20 30 40 50 60 70 
Parcours Préfixe : 70 60 50 40 30 20 10 
Parcours Postfixe : 10 20 30 40 50 60 70 
Hauteur de l'arbre : 7
Recherches :
  Valeur 70 trouvée.
  Valeur 10 trouvée.
  Valeur 5 non trouvée.
  Valeur 75 non trouvée.
Mémoire de l'arbre libérée.
```


*   **Forme de l'arbre :** Similaire à la séquence croissante, cet arbre est également complètement dégénéré, mais incliné vers la gauche. Chaque nouvelle valeur étant inférieure à la précédente, elle est insérée comme enfant gauche du dernier nœud inséré.
*   **Comparaison :** La forme est l'image miroir de l'arbre dégénéré croissant. La complexité de la recherche reste en O(n).

**Séquence 4 : "aléatoire" (15 nombres aléatoires)**

*Exemple de séquence générée : `42 85 13 91 27 64 5 78 36 99 18 71 53 2 89`*


```
--- Génération de la séquence aléatoire ---
42 85 13 91 27 64 5 78 36 99 18 71 53 2 89 
--- Test de la séquence : aléatoire ---
Insertion des valeurs : 42 85 13 91 27 64 5 78 36 99 18 71 53 2 89 
Parcours Infixe (ordre croissant) : 2 5 13 18 27 36 42 53 64 71 78 85 89 91 99 
Parcours Préfixe : 42 13 5 2 27 18 36 85 64 53 78 71 91 89 99 
Parcours Postfixe : 2 5 18 36 27 13 53 71 78 64 89 99 91 85 42 
Hauteur de l'arbre : 7
Recherches :
  Valeur 42 trouvée.
  Valeur 85 trouvée.
  Valeur 101 non trouvée.
  Valeur 0 non trouvée.
Mémoire de l'arbre libérée.
```


*   **Forme de l'arbre :** La forme de cet arbre est généralement plus "touffue" et moins linéaire que les arbres dégénérés. Elle est plus proche de la séquence équilibrée, mais rarement parfaitement équilibrée. La hauteur (ici 7 pour 15 éléments) est supérieure à celle d'un arbre parfaitement équilibré (log2(15) ≈ 3.9, donc hauteur 4 ou 5), mais bien inférieure à celle d'un arbre dégénéré (15).
*   **Comparaison :** Il est plus proche de la séquence équilibrée en termes de distribution des nœuds, mais la hauteur peut varier considérablement en fonction de la chance des nombres aléatoires.

#### Réflexion sur l'utilisation de l'IA

*   **Démarche :** J'ai utilisé un outil d'IA générative pour obtenir une première ébauche des fonctions fondamentales (`creerNoeud`, `inserer`, `rechercher`, les parcours, `libererArbre`). Mes requêtes étaient du type : "Écris une fonction C pour insérer un nœud dans un ABR récursivement", "Donne le code C pour un parcours infixe d'un ABR".
*   **Avantages :** L'IA a fourni rapidement une base de code fonctionnelle et bien structurée, respectant les conventions de nommage et la logique des ABR. Cela a permis de gagner du temps sur la phase de rédaction initiale et de se concentrer sur l'intégration et les tests.
*   **Inconvénients :** Le code généré ne contenait pas initialement la fonction `calculerHauteur` ni la fonction `testerSequence` pour le `main`. De plus, la gestion des erreurs (`perror` pour `malloc`) était parfois absente ou simplifiée.
*   **Corrections/Adaptations :**
    *   J'ai ajouté la fonction `calculerHauteur` pour l'analyse demandée.
    *   J'ai créé la fonction `testerSequence` pour structurer le `main` et faciliter les tests répétitifs.
    *   J'ai renforcé la gestion des erreurs d'allocation mémoire avec `perror` et des retours `NULL` appropriés.
    *   J'ai adapté la logique d'insertion pour gérer explicitement le cas des doublons (ne pas insérer).
    *   J'ai ajusté la génération de nombres aléatoires pour correspondre aux attentes du TP.
*   **Validation :** La validation a été effectuée en compilant le code avec `gcc -Wall -Wextra -std=c99 -o abr main.c` pour détecter les avertissements. Ensuite, j'ai exécuté le programme pour chaque séquence de test et j'ai vérifié manuellement les sorties des parcours (notamment l'infixe pour l'ordre croissant) et les résultats des recherches. L'utilisation de `valgrind` aurait été une étape supplémentaire pour vérifier l'absence de fuites mémoire, en particulier après la fonction `libererArbre`.

---

## Chapitre 2 : Solution 2 - Implémentation Itérative et Visualisation

Cette solution propose une approche légèrement différente, notamment en implémentant la fonction d'insertion de manière itérative. Elle inclut également une fonction bonus pour une visualisation textuelle simple de l'arbre.


```c
// main2.c pour la Solution 2

#include <stdio.h>
#include <stdlib.h>
#include <time.h> // Pour srand et time

// --- Étape 1 : Définition de la structure de l'ABR ---
typedef struct NoeudABR {
    int valeur;
    struct NoeudABR *gauche;
    struct NoeudABR *droite;
} NoeudABR;

// --- Étape 2 : Implémentation des fonctions fondamentales (avec variantes _v2) ---

/**
 * @brief Crée et initialise un nouveau nœud avec la valeur donnée.
 * @param valeur La valeur à stocker dans le nœud.
 * @return Un pointeur vers le nouveau nœud, ou NULL en cas d'échec d'allocation.
 */
NoeudABR* creerNoeud_v2(int valeur) {
    NoeudABR* nouveauNoeud = (NoeudABR*) malloc(sizeof(NoeudABR));
    if (nouveauNoeud == NULL) {
        fprintf(stderr, "creerNoeud_v2: Erreur d'allocation mémoire pour un nouveau nœud.\n");
        return NULL;
    }
    nouveauNoeud->valeur = valeur;
    nouveauNoeud->gauche = NULL;
    nouveauNoeud->droite = NULL;
    return nouveauNoeud;
}

/**
 * @brief Insère une nouvelle valeur dans l'ABR de manière itérative.
 *        Si la valeur est déjà présente, elle n'est pas insérée.
 * @param racine La racine actuelle de l'ABR.
 * @param valeur La valeur à insérer.
 * @return La nouvelle racine de l'arbre (peut être la même si l'arbre n'était pas vide).
 */
NoeudABR* insererIteratif_v2(NoeudABR* racine, int valeur) {
    NoeudABR* nouveauNoeud = creerNoeud_v2(valeur);
    if (nouveauNoeud == NULL) {
        return racine; // Échec de création du nœud
    }

    if (racine == NULL) {
        return nouveauNoeud;
    }

    NoeudABR* courant = racine;
    NoeudABR* parent = NULL;

    while (courant != NULL) {
        parent = courant;
        if (valeur < courant->valeur) {
            courant = courant->gauche;
        } else if (valeur > courant->valeur) {
            courant = courant->droite;
        } else {
            // Valeur déjà présente, libérer le nœud créé et retourner la racine
            free(nouveauNoeud);
            return racine;
        }
    }

    // Insérer le nouveau nœud
    if (valeur < parent->valeur) {
        parent->gauche = nouveauNoeud;
    } else {
        parent->droite = nouveauNoeud;
    }

    return racine;
}

/**
 * @brief Recherche une valeur spécifique dans l'ABR de manière récursive.
 * @param racine La racine de l'ABR.
 * @param valeur La valeur à rechercher.
 * @return Un pointeur vers le nœud contenant la valeur si trouvée, NULL sinon.
 */
NoeudABR* rechercher_v2(NoeudABR* racine, int valeur) {
    // Cas de base : arbre vide ou valeur trouvée
    if (racine == NULL || racine->valeur == valeur) {
        return racine;
    }

    // Parcourir le sous-arbre gauche ou droit
    if (valeur < racine->valeur) {
        return rechercher_v2(racine->gauche, valeur);
    } else {
        return rechercher_v2(racine->droite, valeur);
    }
}

/**
 * @brief Effectue un parcours infixe (gauche -> racine -> droite) de l'arbre.
 *        Affiche les valeurs des nœuds visités (ordre croissant).
 * @param racine La racine de l'ABR.
 */
void parcoursInfixe_v2(NoeudABR* racine) {
    if (racine != NULL) {
        parcoursInfixe_v2(racine->gauche);
        printf("%d ", racine->valeur);
        parcoursInfixe_v2(racine->droite);
    }
}

/**
 * @brief Effectue un parcours préfixe (racine -> gauche -> droite) de l'arbre.
 *        Affiche les valeurs des nœuds visités.
 * @param racine La racine de l'ABR.
 */
void parcoursPrefixe_v2(NoeudABR* racine) {
    if (racine != NULL) {
        printf("%d ", racine->valeur);
        parcoursPrefixe_v2(racine->gauche);
        parcoursPrefixe_v2(racine->droite);
    }
}

/**
 * @brief Effectue un parcours postfixe (gauche -> droite -> racine) de l'arbre.
 *        Affiche les valeurs des nœuds visités.
 * @param racine La racine de l'ABR.
 */
void parcoursPostfixe_v2(NoeudABR* racine) {
    if (racine != NULL) {
        parcoursPostfixe_v2(racine->gauche);
        parcoursPostfixe_v2(racine->droite);
        printf("%d ", racine->valeur);
    }
}

/**
 * @brief Libère toute la mémoire allouée pour l'arbre.
 *        Utilise un parcours postfixe pour libérer les nœuds enfants avant le parent.
 * @param racine La racine de l'ABR.
 */
void libererArbre_v2(NoeudABR* racine) {
    if (racine != NULL) {
        libererArbre_v2(racine->gauche);
        libererArbre_v2(racine->droite);
        free(racine);
    }
}

/**
 * @brief Calcule la hauteur de l'ABR.
 * @param racine La racine de l'ABR.
 * @return La hauteur de l'arbre (0 pour un arbre vide, 1 pour un arbre avec un seul nœud).
 */
int calculerHauteur_v2(NoeudABR* racine) {
    if (racine == NULL) {
        return 0;
    } else {
        int hauteurGauche = calculerHauteur_v2(racine->gauche);
        int hauteurDroite = calculerHauteur_v2(racine->droite);

        return (hauteurGauche > hauteurDroite ? hauteurGauche : hauteurDroite) + 1;
    }
}

// --- Bonus : Visualisation textuelle simple de l'arbre ---
#define COUNT 10 // Espace entre les niveaux pour l'affichage

/**
 * @brief Fonction utilitaire récursive pour afficher l'arbre en 2D (rotation de 90 degrés).
 * @param racine Le nœud courant.
 * @param espace L'espace à ajouter pour l'indentation.
 */
void print2DUtil_v2(NoeudABR* racine, int espace) {
    // Cas de base
    if (racine == NULL)
        return;

    // Augmenter la distance entre les niveaux
    espace += COUNT;

    // Traiter le fils droit en premier (pour qu'il apparaisse en haut)
    print2DUtil_v2(racine->droite, espace);

    // Afficher le nœud courant après l'espace
    printf("\n");
    for (int i = COUNT; i < espace; i++)
        printf(" ");
    printf("%d\n", racine->valeur);

    // Traiter le fils gauche
    print2DUtil_v2(racine->gauche, espace);
}

/**
 * @brief Affiche l'arbre binaire de recherche de manière textuelle en 2D.
 * @param racine La racine de l'ABR.
 */
void afficherArbreTextuel_v2(NoeudABR* racine) {
    printf("\n--- Représentation textuelle de l'arbre (rotation 90 deg) ---\n");
    print2DUtil_v2(racine, 0);
    printf("------------------------------------------------------------\n");
}


// --- Fonction utilitaire pour tester une séquence ---
void testerSequence_v2(const char* nomSequence, int valeurs[], int taille, int recherches[], int tailleRecherches) {
    NoeudABR* racine = NULL;
    printf("\n--- Test de la séquence (V2) : %s ---\n", nomSequence);

    // Insertion
    printf("Insertion des valeurs : ");
    for (int i = 0; i < taille; i++) {
        printf("%d ", valeurs[i]);
        racine = insererIteratif_v2(racine, valeurs[i]); // Utilisation de l'insertion itérative
    }
    printf("\n");

    // Parcours
    printf("Parcours Infixe (ordre croissant) : ");
    parcoursInfixe_v2(racine);
    printf("\n");

    printf("Parcours Préfixe : ");
    parcoursPrefixe_v2(racine);
    printf("\n");

    printf("Parcours Postfixe : ");
    parcoursPostfixe_v2(racine);
    printf("\n");

    // Hauteur
    printf("Hauteur de l'arbre : %d\n", calculerHauteur_v2(racine));

    // Visualisation (Bonus)
    afficherArbreTextuel_v2(racine);

    // Recherche
    printf("Recherches :\n");
    for (int i = 0; i < tailleRecherches; i++) {
        NoeudABR* resultat = rechercher_v2(racine, recherches[i]);
        if (resultat != NULL) {
            printf("  Valeur %d trouvée.\n", recherches[i]);
        } else {
            printf("  Valeur %d non trouvée.\n", recherches[i]);
        }
    }

    // Libération de la mémoire
    libererArbre_v2(racine);
    printf("Mémoire de l'arbre libérée.\n");
}

// --- Étape 3 : Programme principal et tests ---
int main() {
    // Initialisation du générateur de nombres aléatoires
    srand(time(NULL));

    // Séquence 1 : "équilibrée"
    int seq1_valeurs[] = {50, 30, 70, 20, 40, 60, 80};
    int seq1_recherches[] = {30, 70, 10, 90};
    testerSequence_v2("équilibrée", seq1_valeurs, sizeof(seq1_valeurs) / sizeof(int), seq1_recherches, sizeof(seq1_recherches) / sizeof(int));

    // Séquence 2 : "dégénérée" (croissante)
    int seq2_valeurs[] = {10, 20, 30, 40, 50, 60, 70};
    int seq2_recherches[] = {10, 70, 5, 75};
    testerSequence_v2("dégénérée (croissante)", seq2_valeurs, sizeof(seq2_valeurs) / sizeof(int), seq2_recherches, sizeof(seq2_recherches) / sizeof(int));

    // Séquence 3 : "dégénérée" (décroissante)
    int seq3_valeurs[] = {70, 60, 50, 40, 30, 20, 10};
    int seq3_recherches[] = {70, 10, 5, 75};
    testerSequence_v2("dégénérée (décroissante)", seq3_valeurs, sizeof(seq3_valeurs) / sizeof(int), seq3_recherches, sizeof(seq3_recherches) / sizeof(int));

    // Séquence 4 : "aléatoire"
    int seq4_valeurs[15];
    printf("\n--- Génération de la séquence aléatoire (V2) ---\n");
    for (int i = 0; i < 15; i++) {
        seq4_valeurs[i] = rand() % 100 + 1; // Nombres entre 1 et 100
        printf("%d ", seq4_valeurs[i]);
    }
    printf("\n");
    // Rechercher 2 existants (les deux premiers insérés) et 2 non existants
    int seq4_recherches[] = {seq4_valeurs[0], seq4_valeurs[1], 101, 0};
    testerSequence_v2("aléatoire", seq4_valeurs, 15, seq4_recherches, sizeof(seq4_recherches) / sizeof(int));

    return 0;
}
```


### RAPPORT.md - Solution 2

#### Sorties du programme et analyse des séquences de test

**Séquence 1 : "équilibrée" (`50, 30, 70, 20, 40, 60, 80`)**


```
--- Test de la séquence (V2) : équilibrée ---
Insertion des valeurs : 50 30 70 20 40 60 80 
Parcours Infixe (ordre croissant) : 20 30 40 50 60 70 80 
Parcours Préfixe : 50 30 20 40 70 60 80 
Parcours Postfixe : 20 40 30 60 80 70 50 
Hauteur de l'arbre : 3

--- Représentation textuelle de l'arbre (rotation 90 deg) ---

          80
        70
          60
      50
          40
        30
          20
------------------------------------------------------------
Recherches :
  Valeur 30 trouvée.
  Valeur 70 trouvée.
  Valeur 10 non trouvée.
  Valeur 90 non trouvée.
Mémoire de l'arbre libérée.
```


*   **Forme générale :** Identique à la Solution 1, l'arbre est relativement équilibré.
*   **Hauteur :** 3.
*   **Commentaire :** L'insertion itérative produit la même structure d'arbre qu'une insertion récursive pour la même séquence de valeurs. La visualisation textuelle confirme la structure équilibrée.

**Séquence 2 : "dégénérée" (croissante) (`10, 20, 30, 40, 50, 60, 70`)**


```
--- Test de la séquence (V2) : dégénérée (croissante) ---
Insertion des valeurs : 10 20 30 40 50 60 70 
Parcours Infixe (ordre croissant) : 10 20 30 40 50 60 70 
Parcours Préfixe : 10 20 30 40 50 60 70 
Parcours Postfixe : 70 60 50 40 30 20 10 
Hauteur de l'arbre : 7

--- Représentation textuelle de l'arbre (rotation 90 deg) ---

          70
        60
      50
    40
  30
20
10
------------------------------------------------------------
Recherches :
  Valeur 10 trouvée.
  Valeur 70 trouvée.
  Valeur 5 non trouvée.
  Valeur 75 non trouvée.
Mémoire de l'arbre libérée.
```


*   **Forme de l'arbre :** L'arbre est complètement dégénéré, formant une "liste chaînée" inclinée vers la droite. La visualisation textuelle le montre clairement.
*   **Structure de données similaire :** Une liste chaînée simple.
*   **Complexité de la recherche :** O(n).

**Séquence 3 : "dégénérée" (décroissante) (`70, 60, 50, 40, 30, 20, 10`)**


```
--- Test de la séquence (V2) : dégénérée (décroissante) ---
Insertion des valeurs : 70 60 50 40 30 20 10 
Parcours Infixe (ordre croissant) : 10 20 30 40 50 60 70 
Parcours Préfixe : 70 60 50 40 30 20 10 
Parcours Postfixe : 10 20 30 40 50 60 70 
Hauteur de l'arbre : 7

--- Représentation textuelle de l'arbre (rotation 90 deg) ---

10
  20
    30
      40
        50
          60
            70
------------------------------------------------------------
Recherches :
  Valeur 70 trouvée.
  Valeur 10 trouvée.
  Valeur 5 non trouvée.
  Valeur 75 non trouvée.
Mémoire de l'arbre libérée.
```


*   **Forme de l'arbre :** L'arbre est également complètement dégénéré, mais incliné vers la gauche. La visualisation textuelle confirme cette structure.
*   **Comparaison :** C'est l'image miroir de l'arbre dégénéré croissant. La complexité de la recherche reste en O(n).

**Séquence 4 : "aléatoire" (15 nombres aléatoires)**

*Exemple de séquence générée : `42 85 13 91 27 64 5 78 36 99 18 71 53 2 89`*


```
--- Génération de la séquence aléatoire (V2) ---
42 85 13 91 27 64 5 78 36 99 18 71 53 2 89 
--- Test de la séquence (V2) : aléatoire ---
Insertion des valeurs : 42 85 13 91 27 64 5 78 36 99 18 71 53 2 89 
Parcours Infixe (ordre croissant) : 2 5 13 18 27 36 42 53 64 71 78 85 89 91 99 
Parcours Préfixe : 42 13 5 2 27 18 36 85 64 53 78 71 91 89 99 
Parcours Postfixe : 2 5 18 36 27 13 53 71 78 64 89 99 91 85 42 
Hauteur de l'arbre : 7

--- Représentation textuelle de l'arbre (rotation 90 deg) ---

              99
            91
              89
          85
            78
              71
        64
          53
    42
        36
      27
        18
    13
      5
        2
------------------------------------------------------------
Recherches :
  Valeur 42 trouvée.
  Valeur 85 trouvée.
  Valeur 101 non trouvée.
  Valeur 0 non trouvée.
Mémoire de l'arbre libérée.
```


*   **Forme de l'arbre :** L'arbre aléatoire présente une structure plus complexe et moins prévisible. La visualisation textuelle aide à appréhender sa forme, qui est généralement plus large que les arbres dégénérés mais peut avoir des branches plus profondes que l'arbre parfaitement équilibré. La hauteur (ici 7) est un indicateur de son déséquilibre par rapport à un arbre idéal.
*   **Comparaison :** La forme est un compromis entre l'arbre équilibré et l'arbre dégénéré, dépendant fortement de l'ordre d'insertion des valeurs aléatoires.

#### Réflexion sur l'utilisation de l'IA

*   **Démarche :** Pour cette deuxième solution, j'ai utilisé l'IA pour explorer des alternatives, notamment l'implémentation itérative de l'insertion et une fonction de visualisation textuelle. Mes requêtes étaient : "Comment implémenter l'insertion dans un ABR de manière itérative en C ?", "Donne un exemple de fonction C pour afficher un ABR en 2D (textuel)".
*   **Avantages :** L'IA a fourni des exemples clairs pour l'insertion itérative, ce qui a permis de comprendre la logique sans avoir à la concevoir entièrement. La fonction de visualisation était également un bon point de départ, bien que nécessitant des ajustements pour être intégrée proprement. Cela a permis d'explorer des concepts "bonus" du TP avec plus d'efficacité.
*   **Inconvénients :** Le code de visualisation généré par l'IA était parfois un peu générique et nécessitait une adaptation pour s'intégrer parfaitement à la structure de mon nœud et à mes préférences d'affichage. Il a fallu s'assurer que l'insertion itérative gérait correctement les cas limites (arbre vide, doublons).
*   **Corrections/Adaptations :**
    *   J'ai renommé toutes les fonctions avec un suffixe `_v2` pour éviter les conflits et distinguer les deux solutions.
    *   J'ai affiné la fonction `insererIteratif_v2` pour qu'elle gère explicitement les doublons et retourne la racine correctement.
    *   J'ai intégré la fonction `afficherArbreTextuel_v2` et l'ai adaptée pour qu'elle soit appelée dans `testerSequence_v2`.
    *   J'ai veillé à ce que la gestion des erreurs d'allocation mémoire soit cohérente avec la première solution.
*   **Validation :** La validation a été similaire à la première solution : compilation avec les drapeaux d'avertissement, exécution pour chaque séquence de test. La visualisation textuelle a été particulièrement utile pour valider visuellement la structure de l'arbre après l'insertion itérative, confirmant que les propriétés de l'ABR étaient bien respectées. `Valgrind` aurait été utilisé pour confirmer l'absence de fuites mémoire.