Bonjour à toutes et à tous,

Ce TP est une opportunité d'approfondir votre maîtrise des structures de données dynamiques en C, des outils essentiels en algorithmique. Nous allons construire ensemble un petit gestionnaire de contacts, ce qui nous permettra de manipuler concrètement une liste doublement chaînée et une table de hachage.

---

### **TP : Structures Dynamiques Avancées - Gestionnaire de Contacts en C**

**Objectif du TP :**
Mettre en pratique l'implémentation de structures de données dynamiques fondamentales : la liste doublement chaînée (LDC) et la table de hachage avec chaînage séparé.

**Contexte / Mini-Projet :**
Nous allons développer un gestionnaire de contacts simplifié. Chaque contact sera défini par un **nom** (qui servira de clé unique) et un **numéro de téléphone**. Ce projet nous permettra d'explorer comment ces structures s'articulent pour créer des applications efficaces pour l'ajout, la recherche et la suppression de données.

**Prérequis :**
*   Maîtrise du langage C (pointeurs, allocation dynamique de mémoire avec `malloc`/`free`, structures).
*   Compréhension des concepts d'algorithmique de base.

---

### **Partie 1 : Implémentation d'une Liste Doublement Chaînée (LDC)**

Une liste doublement chaînée permet une navigation bidirectionnelle et une suppression plus efficace des nœuds.

1.  **Définition des Structures :**
    *   Créez une structure `Contact` pour stocker les informations d'un contact :
        ```c
        typedef struct Contact {
            char nom[50];
            char telephone[20];
        } Contact;
        ```
    *   Créez une structure `Node` pour représenter un nœud de la liste doublement chaînée. Chaque nœud contiendra un `Contact` et des pointeurs vers le nœud précédent (`prev`) et le nœud suivant (`next`).
        ```c
        typedef struct Node {
            Contact data;
            struct Node *prev;
            struct Node *next;
        } Node;
        ```

2.  **Fonctions de Base de la LDC :**
    Implémentez les fonctions suivantes pour manipuler votre liste doublement chaînée :

    *   `Contact* creerContact(const char* nom, const char* telephone)` :
        *   Alloue dynamiquement un `Contact`, initialise ses champs et le retourne. Gérer les erreurs d'allocation.
    *   `Node* creerNoeud(Contact* contact)` :
        *   Alloue dynamiquement un `Node`, y copie les données du `Contact` fourni et initialise `prev` et `next` à `NULL`. Gérer les erreurs d'allocation.
    *   `void insererEnTete(Node** head, Node* nouveauNoeud)` :
        *   Insère `nouveauNoeud` au début de la liste.
    *   `void insererEnQueue(Node** head, Node* nouveauNoeud)` :
        *   Insère `nouveauNoeud` à la fin de la liste.
    *   `void supprimerNoeud(Node** head, const char* nom)` :
        *   Recherche un contact par son `nom` et supprime le nœud correspondant de la liste. Gérer les cas où le nœud est la tête, la queue ou au milieu. N'oubliez pas de libérer la mémoire du nœud supprimé.
    *   `Contact* rechercherContactLDC(Node* head, const char* nom)` :
        *   Recherche un contact par son `nom` et retourne un pointeur vers les données `Contact` si trouvé, `NULL` sinon.
    *   `void afficherLDC(Node* head)` :
        *   Parcourt la liste et affiche les informations de chaque contact.
    *   `void libererLDC(Node** head)` :
        *   Libère toute la mémoire allouée pour la liste.

**Tests de la Partie 1 :**
Dans votre fonction `main`, créez une LDC, ajoutez quelques contacts, affichez-les, supprimez-en un, recherchez-en un autre, puis libérez la liste. Vérifiez que tout fonctionne comme attendu.

---

### **Partie 2 : Implémentation d'une Table de Hachage avec Chaînage Séparé**

Nous allons maintenant construire une table de hachage qui utilisera les LDC implémentées précédemment pour gérer les collisions.

1.  **Définition de la Structure :**
    *   Définissez une constante pour la taille de votre table de hachage (par exemple, `#define TAILLE_TABLE 10`).
    *   Créez une structure `HashTable` :
        ```c
        typedef struct HashTable {
            Node** buckets; // Un tableau de pointeurs vers des têtes de LDC
            int size;       // Taille de la table
        } HashTable;
        ```

2.  **Fonction de Hachage :**
    *   `unsigned int fonctionDeHachage(const char* cle, int tailleTable)` :
        *   Implémentez une fonction de hachage simple. Une approche courante est de sommer les valeurs ASCII des caractères de la clé et de prendre le modulo de cette somme par la `tailleTable`.
        *   Exemple : `(somme_des_ascii) % tailleTable`.

3.  **Fonctions de Base de la Table de Hachage :**
    Implémentez les fonctions suivantes :

    *   `HashTable* creerTableDeHachage(int taille)` :
        *   Alloue dynamiquement une `HashTable`, initialise sa taille et alloue le tableau de `buckets`. Chaque `bucket` doit être initialisé à `NULL` (liste vide). Gérer les erreurs d'allocation.
    *   `void insererContactHT(HashTable* ht, Contact* contact)` :
        *   Calcule l'index de hachage pour le `nom` du contact.
        *   Crée un `Node` à partir du `contact`.
        *   Insère ce `Node` dans la LDC correspondante au `bucket` calculé. Vous pouvez choisir d'insérer en tête ou en queue de la LDC.
        *   *Attention :* Vérifiez si un contact avec le même nom existe déjà dans le bucket. Si oui, vous pouvez choisir de ne pas l'insérer ou de mettre à jour son numéro de téléphone. Pour ce TP, nous allons considérer que les noms sont uniques.
    *   `Contact* rechercherContactHT(HashTable* ht, const char* nom)` :
        *   Calcule l'index de hachage pour le `nom`.
        *   Recherche le contact dans la LDC du `bucket` correspondant en utilisant `rechercherContactLDC`. Retourne le `Contact*` si trouvé, `NULL` sinon.
    *   `void supprimerContactHT(HashTable* ht, const char* nom)` :
        *   Calcule l'index de hachage pour le `nom`.
        *   Appelle `supprimerNoeud` sur la LDC du `bucket` correspondant.
    *   `void afficherTableDeHachage(HashTable* ht)` :
        *   Parcourt tous les `buckets` de la table de hachage. Pour chaque `bucket` non vide, affiche son index et les contacts qu'il contient en utilisant `afficherLDC`.
    *   `void libererTableDeHachage(HashTable* ht)` :
        *   Parcourt tous les `buckets` et appelle `libererLDC` sur chaque liste. Ensuite, libère le tableau de `buckets` lui-même, puis la structure `HashTable`.

**Tests de la Partie 2 :**
Dans votre `main`, créez une `HashTable`, ajoutez plusieurs contacts (certains devraient provoquer des collisions pour tester le chaînage), affichez la table, recherchez des contacts existants et non existants, supprimez-en, puis libérez la table.

---

### **Partie 3 : Intégration et Interface Utilisateur (main)**

Développez une fonction `main` qui offre un menu simple à l'utilisateur pour interagir avec votre gestionnaire de contacts via la table de hachage :

1.  **Initialisation :** Créez une `HashTable`.
2.  **Boucle de Menu :**
    *   Afficher les options : Ajouter un contact, Rechercher un contact, Supprimer un contact, Afficher tous les contacts, Quitter.
    *   Lire le choix de l'utilisateur.
    *   Exécuter l'action correspondante en appelant les fonctions de la table de hachage.
3.  **Gestion de la Saisie :** Utilisez `fgets` pour lire les chaînes de caractères afin d'éviter les problèmes de débordement de tampon et de caractères résiduels dans le buffer d'entrée. N'oubliez pas de supprimer le `\n` final si `fgets` l'ajoute.
4.  **Nettoyage :** Avant de quitter le programme, assurez-vous de libérer toute la mémoire allouée.

---

### **Conseils et Bonnes Pratiques :**

*   **Gestion de la mémoire :** C'est un point crucial. Chaque `malloc` doit avoir son `free` correspondant. Soyez rigoureux pour éviter les fuites de mémoire.
*   **Gestion des erreurs :** Vérifiez toujours les retours de `malloc`. Si une allocation échoue, le programme doit gérer cette erreur proprement (par exemple, afficher un message et quitter).
*   **Robustesse :** Utilisez `const` pour les paramètres de fonctions qui ne sont pas modifiés (ex: `const char* nom`).
*   **Utilisation de l'IA :** L'utilisation d'outils d'IA pour vous aider dans la rédaction de code, la compréhension d'erreurs ou l'exploration d'approches est tout à fait encouragée. L'objectif est d'apprendre et de maîtriser les concepts, pas de réinventer la roue ou de lutter inutilement. Soyez transparent sur votre démarche si vous le souhaitez, et surtout, assurez-vous de *comprendre* le code que vous produisez ou adaptez. Votre capacité à expliquer et à défendre vos choix sera évaluée.
*   **Clarté du code :** Nommez vos variables et fonctions de manière explicite. Commentez les parties complexes de votre code.

---

**Rendu :**
Le code source complet de votre projet (`.c` et `.h` si vous en utilisez), compilable et exécutable.

Bon courage à toutes et à tous ! N'hésitez pas à poser des questions si vous rencontrez des difficultés.