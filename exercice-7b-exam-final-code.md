# Résolveur de Sudoku par Diviser pour Régner & Backtracking

**Matière :** Algorithmique Avancée & Langage C
**Durée :** 2 heures

## 1. Contexte et Objectifs pédagogiques

Le Sudoku est un casse-tête numérique classique. Bien qu'il existe de nombreuses manières de le résoudre (heuristiques humaines, programmation par contraintes), la résolution informatique d'une grille vide ou partiellement remplie se prête naturellement à la recherche exhaustive optimisée, s'apparentant aux paradigmes de recherche arborescente et aux stratégies de type **Diviser pour Régner** (exploration par branches et élagage).

## 2. Consignes et Modalités de Rendu

À la fin de la séance (ou au plus tard à la date/heure limite fixée), vous devez livrer un dossier contenant :

* `sudoku.c` : Le code source principal contenant votre implémentation.

* `sudoku.h` : Le fichier d'en-tête (prototypes de fonctions).

* `USAGE-IA.md` (ou `USAGE-IA.txt`) : Le journal de bord de vos interactions avec l'IA (voir section 5).

## 3. Étape par Étape : Guide de Réalisation

### Étape 1 : Modélisation et Affichage (15 minutes)

Définissez une grille de Sudoku $9 \times 9$ à l'aide d'un tableau statique ou dynamique en C. Représentez les cases vides par la valeur `0`.

* **À faire :**

  * Créez le type de données ou la constante `N = 9`.

  * Écrivez une fonction `afficherGrille(int grille[9][9])` qui affiche proprement la grille dans la console avec des séparateurs visuels pour les sous-grilles $3 \times 3$.

  * Intégrez une grille de test valide mais non triviale (grille "facile" ou "moyenne") dans votre fonction `main`.

### Étape 2 : Validation des Contraintes (25 minutes)

Avant de placer un chiffre dans une case, l'algorithme doit vérifier si ce choix respecte les règles du Sudoku :

1. Le chiffre ne doit pas déjà être présent sur la même **ligne**.

2. Le chiffre ne doit pas déjà être présent sur la même **colonne**.

3. Le chiffre ne doit pas déjà être présent dans le **bloc** $3 \times 3$ correspondant.

* **À faire :**

  * Écrire une fonction de validation :

    ```
    int estValide(int grille[9][9], int ligne, int colonne, int chiffre);
    
    ```

  * La fonction doit retourner `1` (vrai) si le placement est valide, et `0` (faux) sinon.

  * *Astuce mathématique pour les blocs :* L'indice de début du bloc $3 \times 3$ pour une ligne donnée s'obtient par `(ligne / 3) * 3`.

### Étape 3 : Le Cœur Algorithmique – Résolution par Backtracking (50 minutes)

Le Backtracking est une technique algorithmique qui consiste à tester des solutions de manière récursive. Si une impasse est atteinte (aucun chiffre valide ne peut être placé), l'algorithme revient en arrière (*backtrack*) pour tester une autre branche.

* **À faire :**

  * Écrire la fonction récursive principale :

    ```
    int resoudreSudoku(int grille[9][9]);
    
    ```

  * **Logique de l'algorithme :**

    1. Parcourir la grille pour trouver la première case vide (contenant `0`).

    2. Si aucune case vide n'est trouvée, le Sudoku est résolu ! Retourner `1`.

    3. Pour chaque chiffre de $1$ à $9$ :

       * Vérifier si le chiffre est valide dans cette case via `estValide`.

       * Si oui, placer provisoirement le chiffre dans la grille.

       * Appeler récursivement `resoudreSudoku(grille)`.

       * Si l'appel récursif réussit, retourner `1`.

       * Sinon, annuler le placement (remettre `0`, c'est l'étape de retour en arrière) et tester le chiffre suivant.

    4. Si aucun chiffre ne fonctionne, retourner `0` (déclenche le retour en arrière de l'appel précédent).

### Étape 4 : Tests, Robustesse et Main (20 minutes)

* **À faire :**

  * Dans votre fonction `main`, initialisez une grille de test, affichez-la, appelez `resoudreSudoku`, puis affichez la grille résolue.

  * Testez avec une grille impossible pour vérifier que votre programme gère correctement l'absence de solution (affichage d'un message d'erreur clair).

  * Assurez-vous qu'il n'y a pas de fuites mémoire si vous utilisez des allocations dynamiques, et compilez sans avertissements (`warnings`).

## 4. Barème indicatif

* **Structure et propreté du code (Modularité, noms de variables, commentaires) :** 4 points

* **Fonction d'affichage et de validation des contraintes :** 4 points

* **Implémentation correcte du Backtracking (Diviser pour Régner) :** 7 points

* **Fichier `USAGE-IA.md` (Qualité de la démarche critique et honnêteté) :** 5 points

## 5. Spécifications du fichier `USAGE-IA.md`

L'utilisation d'intelligences artificielles génératrices de code (ChatGPT, Copilot, Gemini, etc.) est **totalement autorisée et encouragée**. Cependant, en tant qu'ingénieurs en formation, vous devez être capables d'analyser, de comprendre et de valider ce que vous produisez.

Votre fichier `USAGE-IA.md` doit contenir un tableau ou une liste structurée répondant aux trois critères suivants pour chaque interaction notable avec l'IA :

| **Prompt / Demande faite à l'IA** | **Ce que l'IA a proposé (Résumé)** | **Décision (Accepté / Modifié / Refusé) et Justification** | 
| *Ex: "Comment calculer l'indice du bloc 3x3 en C ?"* | *L'IA a proposé une formule avec des divisions entières.* | **Accepté.** *Compris et intégré car la division entière `(ligne / 3) * 3` donne directement le coin supérieur gauche du bloc.* | 
| *Ex: "Écris-moi tout le code du backtracking du Sudoku."* | *L'IA a généré un bloc de 60 lignes de code complet.* | **Refusé / Modifié.** *Refusé en bloc pour l'écrire par nous-mêmes, mais utilisé comme source d'inspiration pour la structure de la récursion.* | 

*Soyez transparents : un usage critique, même s'il montre des erreurs corrigées grâce à l'IA, sera toujours mieux évalué qu'un code parfait mais incompréhensible.*