# Cours Complet ANSI SQL : Du Débutant à l'Expert

Ce document est un guide pratique, visuel et synthétique sur la norme **ANSI SQL**. Il est conçu pour être facile à lire, direct et immédiatement applicable sur n'importe quel système de gestion de bases de données (PostgreSQL, MySQL, SQLite, SQL Server, Oracle).

---

## Sommaire
1. [Introduction & Fondations pour Débutants](#1-introduction--fondations-pour-débutants)
2. [Module 1 : DDL, Types de Données & Contraintes](#2-module-1--ddl-types-de-données--contraintes)
3. [Module 2 : DML (Manipulation des Données)](#3-module-2--dml-manipulation-des-données)
4. [Module 3 : Filtrage, Tri et Agrégations](#4-module-3--filtrage-tri-et-agrégations)
5. [Module 4 : Les Jonctions Normées ANSI (Joins)](#5-module-4--les-jonctions-normées-ansi-joins)
6. [Module 5 : Sous-requêtes et Opérations d'Ensembles](#6-module-5--sous-requêtes-et-opérations-densembles)
7. [Module 6 : Fonctions Scalaires (Texte, Date, Logique)](#7-module-6--fonctions-scalaires-texte-date-logique)
8. [Module 7 : Contrôle des Transactions (TCL)](#8-module-7--contrôle-des-transactions-tcl)
9. [Module 8 : Contrôle des Accès (DCL)](#9-module-8--contrôle-des-accès-dcl)
10. [Module 9 : Optimisation et Sécurité](#10-module-9--optimisation-et-sécurité)
11. [Module 10 : Plateformes d'Entraînement](#11-module-10--plateformes-dentraînement)

---

## 1. Introduction & Fondations pour Débutants

### 1.1 Le Standard ANSI SQL
Le **SQL** (*Structured Query Language*) est le langage universel des bases de données relationnelles.
* **Portabilité** : Une requête conforme ANSI SQL fonctionne de la même manière sur PostgreSQL, MySQL, SQLite ou SQL Server.

---

### 1.2 Distinction : Langage (SQL) vs Moteur (SGBD)
* **SGBD (Moteur)** : Le serveur/logiciel qui stocke physiquement les données et exécute les calculs (ex: PostgreSQL, MySQL, SQLite).
* **SQL (Langage)** : La langue que vous utilisez pour donner des ordres au SGBD.
> 💡 **Analogie** : Le SGBD est la voiture ; le SQL est le volant et les pédales.

---

### 1.3 Outils Visuels (GUI) Indispensables pour Débuter
Pour éviter d'écrire des commandes dans un terminal noir, utilisez un logiciel d'interface graphique (GUI) :
* **DBeaver** *(Recommandé)* : Outil gratuit et universel, compatible avec toutes les bases de données.
* **SQL-Practice Online** & **W3Schools** : Idéaux pour tester du SQL dans votre navigateur sans rien installer.
* **pgAdmin** / **MySQL Workbench** : Interfaces dédiées respectivement à PostgreSQL et MySQL.

---

### 1.4 Les 3 Règles d'Or du Débutant

1. **🔒 Toujours tester avec `SELECT` avant un `UPDATE` ou `DELETE`**
   * *Règle* : Avant de supprimer ou modifier des lignes, vérifiez la liste exacte des données ciblées.
   ```sql
   -- 1. Étape de vérification
   SELECT * FROM clients WHERE age > 60;
   
   -- 2. Étape d'exécution (une fois confirmé)
   DELETE FROM clients WHERE age > 60;
   ```

2. **🧩 Principe d'Atomicité (Une seule valeur par case)**
   * *Règle* : Ne jamais stocker plusieurs données dans une seule case (ex: pas de `"Jean, Pierre, Marie"` dans une cellule).
   * *Bonne pratique* : Créer une ligne distincte par donnée.

3. **🏋️ S'entraîner sur des jeux de données d'exemple**
   * Au lieu d'une base vide, téléchargez des bases de démonstration gratuites (ex: *Sakila* pour la location de films, *Chinook* pour la musique).

---

## 2. Module 1 : DDL, Types de Données & Contraintes

Le **DDL** (*Data Definition Language*) permet de créer, modifier et structurer les tables.

### 2.1 Les Types de Données Standard ANSI

| Type ANSI SQL | Description | Exemple de valeur | Cas d'usage typique |
| :--- | :--- | :--- | :--- |
| **`VARCHAR(n)`** | Texte à longueur variable (jusqu'à `n` caractères). Économe en mémoire. | `'Alice'`, `'alice@email.com'` | Noms, prénoms, e-mails, adresses. |
| **`CHAR(n)`** | Texte à longueur fixe `n` caractères. | `'FR'`, `'USD'`, `'75001'` | Codes pays, devises, codes fixes. |
| **`INT`** | Nombre entier (positif ou négatif). | `1`, `42`, `-100` | Identifiants, âges, quantités. |
| **`DECIMAL(p,s)`** | Nombre à virgule fixe (`p` chiffres au total, `s` après la virgule). | `99.99`, `1250.50` | Prix, salaires, montants monétaires. |
| **`DATE`** | Date au format `AAAA-MM-JJ`. | `'2026-10-22'` | Dates de naissance, dates d'achat. |
| **`TIMESTAMP`** | Date et heure précises. | `'2026-10-22 14:30:00'` | Horodatage de commandes, logs. |
| **`BOOLEAN`** | Valeur logique binaire. | `TRUE` ou `FALSE` | Statut actif/inactif, choix binaire. |

---

### 2.2 Les Contraintes d'Intégrité et les Clés

* **`PRIMARY KEY` (Clé Primaire)** : Identifiant unique de chaque ligne. Interdit les doublons et les cases vides.
* **`FOREIGN KEY` (Clé Étrangère)** : Lien qui pointe vers la clé primaire d'une autre table (garantit la cohérence des relations).
* **`NOT NULL`** : Champ obligatoire (interdit les valeurs vides).
* **`UNIQUE`** : Empêche les doublons dans une colonne (ex: e-mail unique).
* **`CHECK`** : Règle de validation personnalisée (ex: `CHECK (age >= 18)`).
* **`DEFAULT`** : Valeur attribuée automatiquement si le champ est laissé vide.

---

### 2.3 Exemple : Création de Tables Liées

```sql
-- 1. Table des départements
CREATE TABLE departements (
    departement_id INT PRIMARY KEY,
    nom_departement VARCHAR(50) NOT NULL UNIQUE,
    code_departement CHAR(3) NOT NULL UNIQUE
);

-- 2. Table des employés (avec clé étrangère et contraintes)
CREATE TABLE employes (
    employe_id INT PRIMARY KEY,
    prenom VARCHAR(50) NOT NULL,
    nom VARCHAR(50) NOT NULL,
    email VARCHAR(100) NOT NULL UNIQUE,
    age INT CHECK (age >= 18),
    salaire DECIMAL(10,2) CHECK (salaire > 0),
    date_embauche DATE DEFAULT CURRENT_DATE,
    departement_id INT,
    
    FOREIGN KEY (departement_id) REFERENCES departements(departement_id)
);
```

---

### 2.4 Modification et Suppression de Structure
```sql
-- Ajouter une colonne
ALTER TABLE employes ADD telephone VARCHAR(20);

-- Supprimer une colonne
ALTER TABLE employes DROP COLUMN telephone;

-- Supprimer définitivement la table
DROP TABLE employes;

-- Vider les données en gardant la structure
TRUNCATE TABLE employes;
```

---

## 3. Module 2 : DML (Manipulation des Données)

Le **DML** (*Data Manipulation Language*) regroupe les commandes de gestion des enregistrements.

### 3.1 `INSERT INTO` (Ajouter des lignes)
```sql
INSERT INTO departements (departement_id, nom_departement, code_departement) 
VALUES 
    (10, 'Finance', 'FIN'),
    (20, 'Technologies', 'TEC');

INSERT INTO employes (employe_id, prenom, nom, email, age, salaire, departement_id)
VALUES 
    (101, 'Alice', 'Bernard', 'alice@company.com', 29, 65000.00, 10),
    (102, 'Bob', 'Lemoine', 'bob@company.com', 42, 82000.00, 20);
```

### 3.2 `SELECT` (Lire les données)
```sql
-- Sélectionner toutes les colonnes
SELECT * FROM employes;

-- Sélectionner des colonnes spécifiques
SELECT prenom, nom, salaire FROM employes;
```

### 3.3 `UPDATE` (Modifier des lignes)
```sql
UPDATE employes 
SET salaire = 70000.00, departement_id = 20 
WHERE employe_id = 101;
```

### 3.4 `DELETE` (Supprimer des lignes)
```sql
DELETE FROM employes 
WHERE employe_id = 101;
```

---

## 4. Module 3 : Filtrage, Tri et Agrégations

### 4.1 Clause `WHERE` & Opérateurs

```sql
-- Filtrage numérique et logique
SELECT * FROM employes WHERE age >= 30 AND salaire > 50000;

-- Intervalle (BETWEEN)
SELECT * FROM employes WHERE age BETWEEN 25 AND 40;

-- Liste de valeurs (IN)
SELECT * FROM employes WHERE departement_id IN (10, 20);

-- Recherche de motif textuel (LIKE)
-- % = plusieurs caractères | _ = un seul caractère
SELECT * FROM employes WHERE nom LIKE 'B%';
```

---

### 4.2 Clause `ORDER BY` (Trier)
```sql
-- Tri décroissant sur le salaire, puis croissant sur le nom
SELECT prenom, nom, salaire 
FROM employes 
ORDER BY salaire DESC, nom ASC;
```

---

### 4.3 Fonctions d'Agrégation
* **`COUNT()`** : Compter le nombre de lignes.
* **`SUM()`** : Sommer des valeurs.
* **`AVG()`** : Calculer la moyenne.
* **`MIN()` / `MAX()`** : Trouver la valeur minimale / maximale.

```sql
SELECT 
    COUNT(*) AS total_employes,
    AVG(salaire) AS salaire_moyen,
    MAX(salaire) AS salaire_max
FROM employes;
```

---

### 4.4 `GROUP BY` et `HAVING`
* **`GROUP BY`** : Regroupe les lignes pour effectuer des calculs par catégorie.
* **`HAVING`** : Filtre les résultats **après** le calcul d'agrégation.

```sql
SELECT 
    departement_id, 
    COUNT(*) AS nb_employes, 
    AVG(salaire) AS salaire_moyen
FROM employes
WHERE age > 25
GROUP BY departement_id
HAVING COUNT(*) >= 1;
```

---

## 5. Module 4 : Les Jonctions Normées ANSI (Joins)

Les jonctions assemblent les données de plusieurs tables grâce à une colonne commune (ex: `departement_id`).

| Type de Jonction | Description synthétique |
| :--- | :--- |
| **`INNER JOIN`** | Garde uniquement les lignes qui ont une correspondance dans **les 2 tables**. |
| **`LEFT JOIN`** | Garde **TOUTES** les lignes de la table de gauche + les correspondances à droite. |
| **`RIGHT JOIN`** | Garde **TOUTES** les lignes de la table de droite + les correspondances à gauche. |
| **`FULL JOIN`** | Garde la totalité des lignes des 2 tables (avec `NULL` si pas de correspondance). |
| **`CROSS JOIN`** | Produit cartésien (chaque ligne de A est combinée avec chaque ligne de B). |

---

### Exemples de Syntaxe ANSI :

```sql
-- 1. INNER JOIN
SELECT e.prenom, e.nom, d.nom_departement
FROM employes e
INNER JOIN departements d ON e.departement_id = d.departement_id;

-- 2. LEFT JOIN
SELECT e.prenom, e.nom, d.nom_departement
FROM employes e
LEFT JOIN departements d ON e.departement_id = d.departement_id;

-- 3. SELF JOIN (Jonction d'une table avec elle-même)
SELECT e.prenom AS employe, m.prenom AS manager
FROM employes e
INNER JOIN employes m ON e.manager_id = m.employe_id;
```

---

## 6. Module 5 : Sous-requêtes et Opérations d'Ensembles

### 6.1 Sous-requêtes (Subqueries)
Une requête `SELECT` imbriquée à l'intérieur d'une autre :

```sql
-- Sous-requête avec IN
SELECT * FROM employes 
WHERE departement_id IN (
    SELECT departement_id FROM departements WHERE code_departement = 'TEC'
);

-- Sous-requête avec EXISTS
SELECT * FROM departements d
WHERE EXISTS (
    SELECT 1 FROM employes e WHERE e.departement_id = d.departement_id
);
```

---

### 6.2 Opérations d'Ensembles
Combinaisons de plusieurs requêtes `SELECT` :
* **`UNION`** : Fusionne les résultats en **supprimant les doublons**.
* **`UNION ALL`** : Fusionne les résultats en **conservant les doublons** *(plus rapide)*.
* **`INTERSECT`** : Conserve uniquement les lignes communes aux deux requêtes.
* **`EXCEPT`** : Extrait les lignes de la 1ère requête absentes de la 2nde.

```sql
SELECT email FROM clients
UNION ALL
SELECT email FROM employes;
```

---

## 7. Module 6 : Fonctions Scalaires (Texte, Date, Logique)

### 7.1 Manipulation de Texte
* **`CONCAT(a, b)`** : Assembler du texte. Ex: `CONCAT(prenom, ' ', nom)`.
* **`SUBSTRING(str FROM pos FOR len)`** : Extraire un morceau de texte.
* **`UPPER(str)` / `LOWER(str)`** : Majuscules / Minuscules.
* **`TRIM(str)`** : Supprimer les espaces inutiles.

---

### 7.2 Manipulation de Dates
* **`CURRENT_DATE`** : Date du jour (`AAAA-MM-JJ`).
* **`EXTRACT(PART FROM date)`** : Extraire l'année ou le mois. Ex: `EXTRACT(YEAR FROM date_embauche)`.

---

### 7.3 Logique Conditionnelle (`CASE WHEN`)

```sql
SELECT prenom, salaire,
    CASE 
        WHEN salaire >= 80000 THEN 'Cadre Supérieur'
        WHEN salaire >= 60000 THEN 'Cadre'
        ELSE 'Employé'
    END AS categorie_salaire
FROM employes;

-- COALESCE : Remplacer les valeurs vides (NULL)
SELECT prenom, COALESCE(telephone, 'Non renseigné') AS contact 
FROM employes;
```

---

## 8. Module 7 : Contrôle des Transactions (TCL)

Le **TCL** (*Transaction Control Language*) permet de grouper des opérations pour qu'elles réussissent ou échouent toutes ensemble (Principe d'ACIDité).

```sql
-- Démarrer une transaction
BEGIN TRANSACTION;

-- Opération 1
UPDATE comptes SET solde = solde - 100 WHERE compte_id = 1;

-- Point de sauvegarde
SAVEPOINT transfert_fait;

-- Opération 2
UPDATE comptes SET solde = solde + 100 WHERE compte_id = 2;

-- Valider définitivement
COMMIT;

-- En cas d'erreur : Annuler tout
-- ROLLBACK;
```

---

## 9. Module 8 : Contrôle des Accès (DCL)

Le **DCL** (*Data Control Language*) gère les permissions des utilisateurs :

```sql
-- Accorder le droit de lecture et d'insertion
GRANT SELECT, INSERT ON employes TO utilisateur_comptable;

-- Retirer le droit d'insertion
REVOKE INSERT ON employes FROM utilisateur_comptable;
```

---

## 10. Module 9 : Optimisation et Sécurité

### 10.1 Bonnes Pratiques d'Optimisation
1. **Lister les colonnes au lieu de `SELECT *`** : Évite de transférer des données inutiles.
2. **Filtrer avec `WHERE` plutôt qu'avec `HAVING`** : Filtre avant d'effectuer les calculs d'agrégation.
3. **Préférer `UNION ALL` à `UNION`** : Évite le tri coûteux de dédoublonnage si les doublons n'existent pas ou sont acceptés.
4. **Utiliser des Index** : Accélère considérablement les recherches sur les colonnes fréquemment utilisées dans les clauses `WHERE` et `JOIN`.

---

### 10.2 Sécurité : Prévenir les Injections de Code
* **Principe** : Un utilisateur malveillant tente d'insérer des requêtes non autorisées via des formulaires Web.
* **Solution** : Utiliser des **requêtes préparées** (*Prepared Statements*) avec des paramètres sécurisés au lieu d'assembler directement des chaînes de texte.

```sql
-- Requête préparée avec paramètre sécurisé
SELECT * FROM users WHERE email = ?;
```

---

## 11. Module 10 : Plateformes d'Entraînement

Pour pratiquer immédiatement sans rien installer :
* **Débutants** : SQL-Practice Online, W3Schools SQL Tutorial, SQLZoo.
* **Intermédiaire & Entretiens** : LeetCode, HackerRank, DataLemur.

---
*Document créé pour Gemini Notebook — Guide Pratique ANSI SQL.*

---

## 12. Jeux de Données d'Entraînement Recommandés (Sakila & Chinook)

Pour pratiquer directement sans avoir à installer ou configurer un serveur complexe, voici les liens vers les deux bases d'exemples les plus populaires :

### 🎵 1. Chinook Database (Magasin de Musique)
Simule la gestion d'un magasin de musique en ligne (artistes, albums, pistes, clients, factures).
* **Dépôt officiel GitHub** : [lerocha/chinook-database](https://github.com/lerocha/chinook-database)
* **Releases & Fichiers (.sqlite, .sql)** : [Téléchargements Chinook](https://github.com/lerocha/chinook-database/releases)

### 🎬 2. Sakila / Pagila Database (Location de Films DVD)
Simule une chaîne de magasins de location de films (acteurs, films, catégories, inventaire, clients).
* **Version SQLite (prête à l'emploi)** : [bradleygrant/sakila-sqlite3](https://github.com/bradleygrant/sakila-sqlite3)
* **Version PostgreSQL (Pagila)** : [devrimgunduz/pagila](https://github.com/devrimgunduz/pagila)
* **Version MySQL officielle** : [Archives MySQL Sakila](https://dev.mysql.com/doc/index-other.html)

---