Voici deux solutions possibles pour le TP, présentées sous forme de chapitres Markdown.

---

## Chapitre 1 : Solution 1 - Implémentation Standard

Cette solution suit directement les consignes du TP, en mettant l'accent sur la clarté et la conformité aux exigences.


```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h> // Pour strcpy, strcmp
#include <ctype.h>  // Pour isspace

// --- Définitions des structures ---

// Structure pour stocker les informations d'un contact
typedef struct Contact {
    char nom[50];
    char telephone[20];
} Contact;

// Structure pour un nœud de la liste doublement chaînée
typedef struct Node {
    Contact data;
    struct Node *prev;
    struct Node *next;
} Node;

// Définition de la taille de la table de hachage
#define TAILLE_TABLE 10

// Structure pour la table de hachage
typedef struct HashTable {
    Node** buckets; // Tableau de pointeurs vers des têtes de LDC
    int size;       // Taille de la table
} HashTable;

// --- Fonctions utilitaires pour la saisie ---

// Fonction pour nettoyer le buffer d'entrée (après fgets)
void clean_stdin_buffer() {
    int c;
    while ((c = getchar()) != '\n' && c != EOF);
}

// Fonction pour supprimer le '\n' de la fin d'une chaîne lue par fgets
void remove_newline(char *str) {
    size_t len = strlen(str);
    if (len > 0 && str[len - 1] == '\n') {
        str[len - 1] = '\0';
    }
}

// --- Partie 1 : Implémentation d'une Liste Doublement Chaînée (LDC) ---

/**
 * @brief Crée et alloue dynamiquement un nouveau contact.
 * @param nom Le nom du contact.
 * @param telephone Le numéro de téléphone du contact.
 * @return Un pointeur vers le Contact alloué, ou NULL en cas d'échec.
 */
Contact* creerContact(const char* nom, const char* telephone) {
    Contact* nouveauContact = (Contact*) malloc(sizeof(Contact));
    if (nouveauContact == NULL) {
        perror("Erreur d'allocation pour Contact");
        return NULL;
    }
    strncpy(nouveauContact->nom, nom, sizeof(nouveauContact->nom) - 1);
    nouveauContact->nom[sizeof(nouveauContact->nom) - 1] = '\0'; // Assurer la terminaison
    strncpy(nouveauContact->telephone, telephone, sizeof(nouveauContact->telephone) - 1);
    nouveauContact->telephone[sizeof(nouveauContact->telephone) - 1] = '\0'; // Assurer la terminaison
    return nouveauContact;
}

/**
 * @brief Crée et alloue dynamiquement un nouveau nœud pour la LDC.
 * @param contact Un pointeur vers les données Contact à copier dans le nœud.
 * @return Un pointeur vers le Node alloué, ou NULL en cas d'échec.
 */
Node* creerNoeud(Contact* contact) {
    if (contact == NULL) return NULL;

    Node* nouveauNoeud = (Node*) malloc(sizeof(Node));
    if (nouveauNoeud == NULL) {
        perror("Erreur d'allocation pour Node");
        return NULL;
    }
    nouveauNoeud->data = *contact; // Copie des données du contact
    nouveauNoeud->prev = NULL;
    nouveauNoeud->next = NULL;
    return nouveauNoeud;
}

/**
 * @brief Insère un nouveau nœud en tête de la liste doublement chaînée.
 * @param head Pointeur vers le pointeur de tête de la liste.
 * @param nouveauNoeud Le nœud à insérer.
 */
void insererEnTete(Node** head, Node* nouveauNoeud) {
    if (nouveauNoeud == NULL) return;

    nouveauNoeud->next = *head;
    if (*head != NULL) {
        (*head)->prev = nouveauNoeud;
    }
    *head = nouveauNoeud;
}

/**
 * @brief Insère un nouveau nœud en queue de la liste doublement chaînée.
 * @param head Pointeur vers le pointeur de tête de la liste.
 * @param nouveauNoeud Le nœud à insérer.
 */
void insererEnQueue(Node** head, Node* nouveauNoeud) {
    if (nouveauNoeud == NULL) return;

    if (*head == NULL) {
        *head = nouveauNoeud;
        return;
    }

    Node* current = *head;
    while (current->next != NULL) {
        current = current->next;
    }
    current->next = nouveauNoeud;
    nouveauNoeud->prev = current;
}

/**
 * @brief Supprime un nœud de la LDC par le nom du contact.
 * @param head Pointeur vers le pointeur de tête de la liste.
 * @param nom Le nom du contact à supprimer.
 */
void supprimerNoeud(Node** head, const char* nom) {
    if (*head == NULL || nom == NULL) return;

    Node* current = *head;
    while (current != NULL && strcmp(current->data.nom, nom) != 0) {
        current = current->next;
    }

    if (current == NULL) { // Contact non trouvé
        return;
    }

    // Le nœud est trouvé, on le supprime
    if (current->prev != NULL) {
        current->prev->next = current->next;
    } else { // C'est la tête
        *head = current->next;
    }

    if (current->next != NULL) {
        current->next->prev = current->prev;
    }

    free(current); // Libère la mémoire du nœud
}

/**
 * @brief Recherche un contact dans la LDC par son nom.
 * @param head Pointeur vers la tête de la liste.
 * @param nom Le nom du contact à rechercher.
 * @return Un pointeur vers le Contact si trouvé, NULL sinon.
 */
Contact* rechercherContactLDC(Node* head, const char* nom) {
    Node* current = head;
    while (current != NULL) {
        if (strcmp(current->data.nom, nom) == 0) {
            return &(current->data); // Retourne un pointeur vers les données du contact
        }
        current = current->next;
    }
    return NULL; // Contact non trouvé
}

/**
 * @brief Affiche tous les contacts de la LDC.
 * @param head Pointeur vers la tête de la liste.
 */
void afficherLDC(Node* head) {
    if (head == NULL) {
        printf("  (Liste vide)\n");
        return;
    }
    Node* current = head;
    while (current != NULL) {
        printf("    Nom: %s, Tel: %s\n", current->data.nom, current->data.telephone);
        current = current->next;
    }
}

/**
 * @brief Libère toute la mémoire allouée pour la LDC.
 * @param head Pointeur vers le pointeur de tête de la liste.
 */
void libererLDC(Node** head) {
    Node* current = *head;
    Node* nextNode;
    while (current != NULL) {
        nextNode = current->next;
        free(current);
        current = nextNode;
    }
    *head = NULL; // La liste est maintenant vide
}

// --- Partie 2 : Implémentation d'une Table de Hachage avec Chaînage Séparé ---

/**
 * @brief Fonction de hachage simple.
 * @param cle La chaîne de caractères (nom du contact) à hacher.
 * @param tailleTable La taille de la table de hachage.
 * @return L'index du bucket dans la table de hachage.
 */
unsigned int fonctionDeHachage(const char* cle, int tailleTable) {
    unsigned int hash = 0;
    for (int i = 0; cle[i] != '\0'; i++) {
        hash = hash + cle[i]; // Somme des valeurs ASCII
    }
    return hash % tailleTable;
}

/**
 * @brief Crée et alloue dynamiquement une table de hachage.
 * @param taille La taille désirée pour la table de hachage.
 * @return Un pointeur vers la HashTable allouée, ou NULL en cas d'échec.
 */
HashTable* creerTableDeHachage(int taille) {
    HashTable* ht = (HashTable*) malloc(sizeof(HashTable));
    if (ht == NULL) {
        perror("Erreur d'allocation pour HashTable");
        return NULL;
    }
    ht->size = taille;
    ht->buckets = (Node**) calloc(taille, sizeof(Node*)); // calloc initialise à NULL
    if (ht->buckets == NULL) {
        perror("Erreur d'allocation pour les buckets de HashTable");
        free(ht);
        return NULL;
    }
    return ht;
}

/**
 * @brief Insère un contact dans la table de hachage.
 * @param ht Pointeur vers la table de hachage.
 * @param contact Pointeur vers le Contact à insérer.
 */
void insererContactHT(HashTable* ht, Contact* contact) {
    if (ht == NULL || contact == NULL) return;

    // Vérifier si le contact existe déjà (le TP demande des noms uniques)
    if (rechercherContactHT(ht, contact->nom) != NULL) {
        printf("Le contact '%s' existe déjà. Opération ignorée.\n", contact->nom);
        return;
    }

    unsigned int index = fonctionDeHachage(contact->nom, ht->size);
    Node* nouveauNoeud = creerNoeud(contact);
    if (nouveauNoeud == NULL) {
        fprintf(stderr, "Impossible de créer le nœud pour le contact %s.\n", contact->nom);
        return;
    }
    insererEnQueue(&(ht->buckets[index]), nouveauNoeud); // Insérer en queue du bucket
}

/**
 * @brief Recherche un contact dans la table de hachage par son nom.
 * @param ht Pointeur vers la table de hachage.
 * @param nom Le nom du contact à rechercher.
 * @return Un pointeur vers le Contact si trouvé, NULL sinon.
 */
Contact* rechercherContactHT(HashTable* ht, const char* nom) {
    if (ht == NULL || nom == NULL) return NULL;

    unsigned int index = fonctionDeHachage(nom, ht->size);
    return rechercherContactLDC(ht->buckets[index], nom);
}

/**
 * @brief Supprime un contact de la table de hachage par son nom.
 * @param ht Pointeur vers la table de hachage.
 * @param nom Le nom du contact à supprimer.
 */
void supprimerContactHT(HashTable* ht, const char* nom) {
    if (ht == NULL || nom == NULL) return;

    unsigned int index = fonctionDeHachage(nom, ht->size);
    supprimerNoeud(&(ht->buckets[index]), nom);
}

/**
 * @brief Affiche tous les contacts de la table de hachage.
 * @param ht Pointeur vers la table de hachage.
 */
void afficherTableDeHachage(HashTable* ht) {
    if (ht == NULL) {
        printf("Table de hachage non initialisée.\n");
        return;
    }
    printf("\n--- Contenu de la Table de Hachage ---\n");
    for (int i = 0; i < ht->size; i++) {
        printf("Bucket %d:\n", i);
        afficherLDC(ht->buckets[i]);
    }
    printf("--------------------------------------\n");
}

/**
 * @brief Libère toute la mémoire allouée pour la table de hachage.
 * @param ht Pointeur vers la table de hachage.
 */
void libererTableDeHachage(HashTable* ht) {
    if (ht == NULL) return;

    for (int i = 0; i < ht->size; i++) {
        libererLDC(&(ht->buckets[i])); // Libère chaque liste chaînée
    }
    free(ht->buckets); // Libère le tableau de pointeurs
    free(ht);           // Libère la structure HashTable elle-même
}

// --- Partie 3 : Intégration et Interface Utilisateur (main) ---

int main() {
    HashTable* gestionnaireContacts = creerTableDeHachage(TAILLE_TABLE);
    if (gestionnaireContacts == NULL) {
        fprintf(stderr, "Échec de l'initialisation du gestionnaire de contacts.\n");
        return EXIT_FAILURE;
    }

    int choix;
    char nom[50];
    char telephone[20];
    Contact* tempContact;
    Contact* foundContact;

    do {
        printf("\n--- Gestionnaire de Contacts ---\n");
        printf("1. Ajouter un contact\n");
        printf("2. Rechercher un contact\n");
        printf("3. Supprimer un contact\n");
        printf("4. Afficher tous les contacts\n");
        printf("5. Quitter\n");
        printf("Votre choix : ");
        if (scanf("%d", &choix) != 1) {
            printf("Saisie invalide. Veuillez entrer un nombre.\n");
            clean_stdin_buffer(); // Nettoyer le buffer en cas de saisie non numérique
            continue;
        }
        clean_stdin_buffer(); // Nettoyer le buffer après scanf pour le '\n'

        switch (choix) {
            case 1:
                printf("Nom du contact : ");
                fgets(nom, sizeof(nom), stdin);
                remove_newline(nom);

                printf("Numéro de téléphone : ");
                fgets(telephone, sizeof(telephone), stdin);
                remove_newline(telephone);

                tempContact = creerContact(nom, telephone);
                if (tempContact != NULL) {
                    insererContactHT(gestionnaireContacts, tempContact);
                    // Le contact est copié dans le nœud, donc on peut libérer tempContact
                    free(tempContact);
                    printf("Contact ajouté.\n");
                } else {
                    printf("Échec de la création du contact.\n");
                }
                break;
            case 2:
                printf("Nom du contact à rechercher : ");
                fgets(nom, sizeof(nom), stdin);
                remove_newline(nom);

                foundContact = rechercherContactHT(gestionnaireContacts, nom);
                if (foundContact != NULL) {
                    printf("Contact trouvé : Nom: %s, Tel: %s\n", foundContact->nom, foundContact->telephone);
                } else {
                    printf("Contact '%s' non trouvé.\n", nom);
                }
                break;
            case 3:
                printf("Nom du contact à supprimer : ");
                fgets(nom, sizeof(nom), stdin);
                remove_newline(nom);

                // Vérifier si le contact existe avant de tenter de le supprimer
                if (rechercherContactHT(gestionnaireContacts, nom) != NULL) {
                    supprimerContactHT(gestionnaireContacts, nom);
                    printf("Contact '%s' supprimé.\n", nom);
                } else {
                    printf("Contact '%s' non trouvé, impossible de supprimer.\n", nom);
                }
                break;
            case 4:
                afficherTableDeHachage(gestionnaireContacts);
                break;
            case 5:
                printf("Quitter le gestionnaire de contacts.\n");
                break;
            default:
                printf("Choix invalide. Veuillez réessayer.\n");
        }
    } while (choix != 5);

    libererTableDeHachage(gestionnaireContacts); // Libérer toute la mémoire avant de quitter
    return EXIT_SUCCESS;
}

```


### Explications et Commentaires

*   **Gestion de la Saisie (`fgets`, `remove_newline`, `clean_stdin_buffer`)** : L'utilisation de `fgets` est préférée à `scanf` pour la lecture de chaînes afin d'éviter les débordements de tampon. Les fonctions `remove_newline` et `clean_stdin_buffer` sont essentielles pour gérer le caractère de nouvelle ligne (`\n`) laissé par `fgets` ou `scanf`, qui peut perturber les lectures ultérieures.
*   **Allocation et Libération de Mémoire** : Chaque `malloc` est systématiquement vérifié et un `free` correspondant est effectué. Pour la LDC, `libererLDC` parcourt et libère chaque nœud. Pour la `HashTable`, `libererTableDeHachage` appelle `libererLDC` sur chaque `bucket` avant de libérer le tableau de `buckets` et la structure `HashTable` elle-même.
*   **`creerContact` et `creerNoeud`** : `creerContact` alloue la structure `Contact` et `creerNoeud` alloue la structure `Node` et y copie les données du `Contact`. Il est important de noter que `creerContact` alloue un `Contact` temporaire qui est ensuite copié dans le `Node`. Ce `Contact` temporaire doit être libéré après l'insertion dans la table de hachage.
*   **`strncpy`** : Utilisé pour copier les chaînes de caractères avec une limite de taille, ce qui est une bonne pratique pour prévenir les débordements de tampon. Le caractère nul (`\0`) est explicitement ajouté pour garantir la terminaison de la chaîne.
*   **Fonction de Hachage** : Une fonction de hachage simple est implémentée en sommant les valeurs ASCII des caractères du nom et en prenant le modulo de la taille de la table. Bien que simple, elle est fonctionnelle pour ce TP.
*   **Gestion des Duplicatas** : Dans `insererContactHT`, une vérification est ajoutée pour s'assurer qu'un contact avec le même nom n'est pas inséré deux fois, conformément à l'hypothèse que les noms sont uniques.
*   **Robustesse** : Les pointeurs `NULL` sont vérifiés à l'entrée de presque toutes les fonctions pour éviter les déréférencements de pointeurs nuls.

---

## Chapitre 2 : Solution 2 - Implémentation Raffinée

Cette solution propose quelques raffinements par rapport à la première, notamment une fonction de hachage légèrement plus sophistiquée et une gestion des erreurs plus explicite.


```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h> // Pour strcpy, strcmp
#include <ctype.h>  // Pour isspace

// --- Définitions des structures ---

// Structure pour stocker les informations d'un contact
typedef struct Contact {
    char nom[50];
    char telephone[20];
} Contact;

// Structure pour un nœud de la liste doublement chaînée
typedef struct Node {
    Contact data;
    struct Node *prev;
    struct Node *next;
} Node;

// Définition de la taille de la table de hachage
#define TAILLE_TABLE_V2 13 // Une taille première peut améliorer la distribution

// Structure pour la table de hachage
typedef struct HashTable {
    Node** buckets; // Tableau de pointeurs vers des têtes de LDC
    int size;       // Taille de la table
} HashTable;

// --- Fonctions utilitaires pour la saisie ---

// Fonction pour nettoyer le buffer d'entrée (après fgets ou scanf)
void v2_clean_stdin_buffer() {
    int c;
    while ((c = getchar()) != '\n' && c != EOF);
}

// Fonction pour supprimer le '\n' de la fin d'une chaîne lue par fgets
void v2_remove_newline(char *str) {
    size_t len = strlen(str);
    if (len > 0 && str[len - 1] == '\n') {
        str[len - 1] = '\0';
    }
}

// --- Partie 1 : Implémentation d'une Liste Doublement Chaînée (LDC) ---

/**
 * @brief Crée et alloue dynamiquement un nouveau contact.
 * @param nom Le nom du contact.
 * @param telephone Le numéro de téléphone du contact.
 * @return Un pointeur vers le Contact alloué, ou NULL en cas d'échec.
 */
Contact* v2_creerContact(const char* nom, const char* telephone) {
    Contact* nouveauContact = (Contact*) malloc(sizeof(Contact));
    if (nouveauContact == NULL) {
        perror("v2_creerContact: Erreur d'allocation pour Contact");
        return NULL;
    }
    strncpy(nouveauContact->nom, nom, sizeof(nouveauContact->nom) - 1);
    nouveauContact->nom[sizeof(nouveauContact->nom) - 1] = '\0';
    strncpy(nouveauContact->telephone, telephone, sizeof(nouveauContact->telephone) - 1);
    nouveauContact->telephone[sizeof(nouveauContact->telephone) - 1] = '\0';
    return nouveauContact;
}

/**
 * @brief Crée et alloue dynamiquement un nouveau nœud pour la LDC.
 * @param contact Un pointeur vers les données Contact à copier dans le nœud.
 * @return Un pointeur vers le Node alloué, ou NULL en cas d'échec.
 */
Node* v2_creerNoeud(Contact* contact) {
    if (contact == NULL) {
        fprintf(stderr, "v2_creerNoeud: Contact fourni est NULL.\n");
        return NULL;
    }

    Node* nouveauNoeud = (Node*) malloc(sizeof(Node));
    if (nouveauNoeud == NULL) {
        perror("v2_creerNoeud: Erreur d'allocation pour Node");
        return NULL;
    }
    nouveauNoeud->data = *contact;
    nouveauNoeud->prev = NULL;
    nouveauNoeud->next = NULL;
    return nouveauNoeud;
}

/**
 * @brief Insère un nouveau nœud en tête de la liste doublement chaînée.
 * @param head Pointeur vers le pointeur de tête de la liste.
 * @param nouveauNoeud Le nœud à insérer.
 */
void v2_insererEnTete(Node** head, Node* nouveauNoeud) {
    if (nouveauNoeud == NULL) {
        fprintf(stderr, "v2_insererEnTete: Le nœud à insérer est NULL.\n");
        return;
    }

    nouveauNoeud->next = *head;
    if (*head != NULL) {
        (*head)->prev = nouveauNoeud;
    }
    *head = nouveauNoeud;
}

/**
 * @brief Insère un nouveau nœud en queue de la liste doublement chaînée.
 * @param head Pointeur vers le pointeur de tête de la liste.
 * @param nouveauNoeud Le nœud à insérer.
 */
void v2_insererEnQueue(Node** head, Node* nouveauNoeud) {
    if (nouveauNoeud == NULL) {
        fprintf(stderr, "v2_insererEnQueue: Le nœud à insérer est NULL.\n");
        return;
    }

    if (*head == NULL) {
        *head = nouveauNoeud;
        return;
    }

    Node* current = *head;
    while (current->next != NULL) {
        current = current->next;
    }
    current->next = nouveauNoeud;
    nouveauNoeud->prev = current;
}

/**
 * @brief Supprime un nœud de la LDC par le nom du contact.
 * @param head Pointeur vers le pointeur de tête de la liste.
 * @param nom Le nom du contact à supprimer.
 * @return 1 si le nœud a été supprimé, 0 sinon.
 */
int v2_supprimerNoeud(Node** head, const char* nom) {
    if (*head == NULL || nom == NULL) return 0;

    Node* current = *head;
    while (current != NULL && strcmp(current->data.nom, nom) != 0) {
        current = current->next;
    }

    if (current == NULL) { // Contact non trouvé
        return 0;
    }

    if (current->prev != NULL) {
        current->prev->next = current->next;
    } else { // C'est la tête
        *head = current->next;
    }

    if (current->next != NULL) {
        current->next->prev = current->prev;
    }

    free(current);
    return 1; // Nœud supprimé avec succès
}

/**
 * @brief Recherche un contact dans la LDC par son nom.
 * @param head Pointeur vers la tête de la liste.
 * @param nom Le nom du contact à rechercher.
 * @return Un pointeur vers le Contact si trouvé, NULL sinon.
 */
Contact* v2_rechercherContactLDC(Node* head, const char* nom) {
    Node* current = head;
    while (current != NULL) {
        if (strcmp(current->data.nom, nom) == 0) {
            return &(current->data);
        }
        current = current->next;
    }
    return NULL;
}

/**
 * @brief Affiche tous les contacts de la LDC.
 * @param head Pointeur vers la tête de la liste.
 */
void v2_afficherLDC(Node* head) {
    if (head == NULL) {
        printf("  (Liste vide)\n");
        return;
    }
    Node* current = head;
    while (current != NULL) {
        printf("    Nom: %s, Tel: %s\n", current->data.nom, current->data.telephone);
        current = current->next;
    }
}

/**
 * @brief Libère toute la mémoire allouée pour la LDC.
 * @param head Pointeur vers le pointeur de tête de la liste.
 */
void v2_libererLDC(Node** head) {
    Node* current = *head;
    Node* nextNode;
    while (current != NULL) {
        nextNode = current->next;
        free(current);
        current = nextNode;
    }
    *head = NULL;
}

// --- Partie 2 : Implémentation d'une Table de Hachage avec Chaînage Séparé ---

/**
 * @brief Fonction de hachage DJB2 (plus robuste que la somme simple).
 * @param cle La chaîne de caractères (nom du contact) à hacher.
 * @param tailleTable La taille de la table de hachage.
 * @return L'index du bucket dans la table de hachage.
 */
unsigned int v2_fonctionDeHachage(const char* cle, int tailleTable) {
    unsigned int hash = 5381; // Constante initiale
    int c;
    while ((c = *cle++)) {
        hash = ((hash << 5) + hash) + c; // hash * 33 + c
    }
    return hash % tailleTable;
}

/**
 * @brief Crée et alloue dynamiquement une table de hachage.
 * @param taille La taille désirée pour la table de hachage.
 * @return Un pointeur vers la HashTable allouée, ou NULL en cas d'échec.
 */
HashTable* v2_creerTableDeHachage(int taille) {
    HashTable* ht = (HashTable*) malloc(sizeof(HashTable));
    if (ht == NULL) {
        perror("v2_creerTableDeHachage: Erreur d'allocation pour HashTable");
        return NULL;
    }
    ht->size = taille;
    ht->buckets = (Node**) calloc(taille, sizeof(Node*));
    if (ht->buckets == NULL) {
        perror("v2_creerTableDeHachage: Erreur d'allocation pour les buckets de HashTable");
        free(ht); // Libérer la structure HashTable si l'allocation des buckets échoue
        return NULL;
    }
    return ht;
}

/**
 * @brief Insère un contact dans la table de hachage.
 * @param ht Pointeur vers la table de hachage.
 * @param contact Pointeur vers le Contact à insérer.
 * @return 1 si le contact a été inséré, 0 si un contact avec le même nom existait déjà.
 */
int v2_insererContactHT(HashTable* ht, Contact* contact) {
    if (ht == NULL || contact == NULL) return 0;

    // Vérifier si le contact existe déjà (le TP demande des noms uniques)
    if (v2_rechercherContactHT(ht, contact->nom) != NULL) {
        printf("Le contact '%s' existe déjà. Opération ignorée.\n", contact->nom);
        return 0;
    }

    unsigned int index = v2_fonctionDeHachage(contact->nom, ht->size);
    Node* nouveauNoeud = v2_creerNoeud(contact);
    if (nouveauNoeud == NULL) {
        fprintf(stderr, "v2_insererContactHT: Impossible de créer le nœud pour le contact %s.\n", contact->nom);
        return 0;
    }
    v2_insererEnQueue(&(ht->buckets[index]), nouveauNoeud);
    return 1;
}

/**
 * @brief Recherche un contact dans la table de hachage par son nom.
 * @param ht Pointeur vers la table de hachage.
 * @param nom Le nom du contact à rechercher.
 * @return Un pointeur vers le Contact si trouvé, NULL sinon.
 */
Contact* v2_rechercherContactHT(HashTable* ht, const char* nom) {
    if (ht == NULL || nom == NULL) return NULL;

    unsigned int index = v2_fonctionDeHachage(nom, ht->size);
    return v2_rechercherContactLDC(ht->buckets[index], nom);
}

/**
 * @brief Supprime un contact de la table de hachage par son nom.
 * @param ht Pointeur vers la table de hachage.
 * @param nom Le nom du contact à supprimer.
 * @return 1 si le contact a été supprimé, 0 sinon.
 */
int v2_supprimerContactHT(HashTable* ht, const char* nom) {
    if (ht == NULL || nom == NULL) return 0;

    unsigned int index = v2_fonctionDeHachage(nom, ht->size);
    return v2_supprimerNoeud(&(ht->buckets[index]), nom);
}

/**
 * @brief Affiche tous les contacts de la table de hachage.
 * @param ht Pointeur vers la table de hachage.
 */
void v2_afficherTableDeHachage(HashTable* ht) {
    if (ht == NULL) {
        printf("Table de hachage non initialisée.\n");
        return;
    }
    printf("\n--- Contenu de la Table de Hachage (V2) ---\n");
    for (int i = 0; i < ht->size; i++) {
        printf("Bucket %d:\n", i);
        v2_afficherLDC(ht->buckets[i]);
    }
    printf("-------------------------------------------\n");
}

/**
 * @brief Libère toute la mémoire allouée pour la table de hachage.
 * @param ht Pointeur vers la table de hachage.
 */
void v2_libererTableDeHachage(HashTable* ht) {
    if (ht == NULL) return;

    for (int i = 0; i < ht->size; i++) {
        v2_libererLDC(&(ht->buckets[i]));
    }
    free(ht->buckets);
    free(ht);
}

// --- Partie 3 : Intégration et Interface Utilisateur (main) ---

int main() {
    HashTable* gestionnaireContacts = v2_creerTableDeHachage(TAILLE_TABLE_V2);
    if (gestionnaireContacts == NULL) {
        fprintf(stderr, "Échec de l'initialisation du gestionnaire de contacts (V2).\n");
        return EXIT_FAILURE;
    }

    int choix;
    char nom[50];
    char telephone[20];
    Contact* tempContact;
    Contact* foundContact;

    do {
        printf("\n--- Gestionnaire de Contacts (V2) ---\n");
        printf("1. Ajouter un contact\n");
        printf("2. Rechercher un contact\n");
        printf("3. Supprimer un contact\n");
        printf("4. Afficher tous les contacts\n");
        printf("5. Quitter\n");
        printf("Votre choix : ");
        if (scanf("%d", &choix) != 1) {
            printf("Saisie invalide. Veuillez entrer un nombre.\n");
            v2_clean_stdin_buffer();
            continue;
        }
        v2_clean_stdin_buffer();

        switch (choix) {
            case 1:
                printf("Nom du contact : ");
                fgets(nom, sizeof(nom), stdin);
                v2_remove_newline(nom);

                printf("Numéro de téléphone : ");
                fgets(telephone, sizeof(telephone), stdin);
                v2_remove_newline(telephone);

                tempContact = v2_creerContact(nom, telephone);
                if (tempContact != NULL) {
                    if (v2_insererContactHT(gestionnaireContacts, tempContact)) {
                        printf("Contact ajouté.\n");
                    }
                    free(tempContact); // Libérer le contact temporaire
                } else {
                    printf("Échec de la création du contact.\n");
                }
                break;
            case 2:
                printf("Nom du contact à rechercher : ");
                fgets(nom, sizeof(nom), stdin);
                v2_remove_newline(nom);

                foundContact = v2_rechercherContactHT(gestionnaireContacts, nom);
                if (foundContact != NULL) {
                    printf("Contact trouvé : Nom: %s, Tel: %s\n", foundContact->nom, foundContact->telephone);
                } else {
                    printf("Contact '%s' non trouvé.\n", nom);
                }
                break;
            case 3:
                printf("Nom du contact à supprimer : ");
                fgets(nom, sizeof(nom), stdin);
                v2_remove_newline(nom);

                if (v2_supprimerContactHT(gestionnaireContacts, nom)) {
                    printf("Contact '%s' supprimé.\n", nom);
                } else {
                    printf("Contact '%s' non trouvé ou échec de la suppression.\n", nom);
                }
                break;
            case 4:
                v2_afficherTableDeHachage(gestionnaireContacts);
                break;
            case 5:
                printf("Quitter le gestionnaire de contacts.\n");
                break;
            default:
                printf("Choix invalide. Veuillez réessayer.\n");
        }
    } while (choix != 5);

    v2_libererTableDeHachage(gestionnaireContacts);
    return EXIT_SUCCESS;
}
```


### Explications et Commentaires

*   **Noms de Fonctions Préfixés** : Toutes les fonctions de cette solution sont préfixées par `v2_` pour éviter les conflits de noms si les deux solutions étaient compilées ensemble (bien que le TP demande un seul fichier compilable).
*   **Taille de la Table de Hachage** : La constante `TAILLE_TABLE_V2` est définie à 13, un nombre premier. L'utilisation d'une taille première pour la table de hachage est une bonne pratique pour améliorer la distribution des clés et réduire les collisions, en particulier avec certaines fonctions de hachage.
*   **Fonction de Hachage DJB2** : La fonction `v2_fonctionDeHachage` utilise l'algorithme DJB2. C'est une fonction de hachage plus robuste et couramment utilisée que la simple somme des valeurs ASCII, offrant une meilleure distribution des clés et réduisant ainsi la probabilité de collisions.
*   **Retours de Fonctions Améliorés** :
    *   `v2_supprimerNoeud` et `v2_insererContactHT` retournent un `int` (1 pour succès, 0 pour échec). Cela permet à la fonction appelante (`main` ou d'autres fonctions de la table de hachage) de savoir si l'opération a réussi et d'afficher un message plus précis à l'utilisateur.
    *   Les messages d'erreur (`perror`, `fprintf(stderr, ...)`) sont plus spécifiques en incluant le nom de la fonction où l'erreur s'est produite.
*   **Gestion des Erreurs d'Allocation** : Dans `v2_creerTableDeHachage`, si l'allocation des `buckets` échoue, la structure `HashTable` déjà allouée est également libérée pour éviter une fuite mémoire partielle.
*   **Clarté du Code** : Les commentaires sont maintenus et les noms de variables sont explicites.
*   **`main` plus Réactif** : La fonction `main` utilise les retours des fonctions pour donner un feedback plus précis à l'utilisateur sur le succès ou l'échec des opérations.

Ces deux solutions offrent des approches complètes et robustes pour le TP, avec la deuxième solution présentant des améliorations en termes de performance potentielle (grâce à la fonction de hachage) et de gestion des retours de fonctions.