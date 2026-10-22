# 🚀 Cours Complet ANSI SQL & Travaux Pratiques

Bienvenue dans ce dépôt d'apprentissage complet consacré à l'**ANSI SQL** (American National Standards Institute), la norme universelle d'interrogation et de gestion de bases de données relationnelles.

Ce dépôt a été conçu pour accompagner les débutants et intermédiaires étape par étape.

---

## 📂 Structure du Dépôt

```text
.
├── README.md                   # Présentation générale et guide de démarrage
├── cours-ansi-sql-complet.md   # Le cours complet structuré en 11 modules
├── EXERCICES.md                # 10 exercices pratiques progressifs avec corrigés
└── data/                       # Liens et scripts pour les bases de données d'exemple
    ├── chinook.db              # Base SQLite exemple (Magasin de musique)
    └── sakila.db               # Base SQLite exemple (Location de films)
```

---

## 📚 Sommaire du Cours

Le fichier [`cours-ansi-sql-complet.md`] couvre les 11 modules suivants :

1. **Introduction & Concept de Base** : SGBD vs SQL, outils visuels (DBeaver) et les 3 Règles d'Or.
2. **Module 1 : DDL, Types de Données & Contraintes** : `VARCHAR`, `INT`, `DECIMAL`, `PRIMARY KEY`, `FOREIGN KEY`, `UNIQUE`, `CHECK`.
3. **Module 2 : DML (Manipulation de Données)** : `SELECT`, `INSERT INTO`, `UPDATE`, `DELETE`.
4. **Module 3 : Filtrage, Tri et Agrégations** : `WHERE`, `ORDER BY`, `GROUP BY`, `HAVING`, `COUNT`, `SUM`, `AVG`.
5. **Module 4 : Les Jonctions ANSI (Joins)** : `INNER JOIN`, `LEFT JOIN`, `RIGHT JOIN`, `FULL JOIN`, `CROSS JOIN`, `SELF JOIN`.
6. **Module 5 : Sous-requêtes et Opérations d'Ensembles** : `IN`, `EXISTS`, `UNION ALL`, `INTERSECT`, `EXCEPT`.
7. **Module 6 : Fonctions Scalaires Standard** : Manipulation de texte (`CONCAT`, `SUBSTRING`), dates (`EXTRACT`) et logique (`CASE WHEN`, `COALESCE`).
8. **Module 7 : Contrôle des Transactions (TCL)** : `COMMIT`, `ROLLBACK`, `SAVEPOINT`.
9. **Module 8 : Contrôle des Accès (DCL)** : `GRANT`, `REVOKE`.
10. **Module 9 : Optimisation & Sécurité** : Bonnes pratiques d'écriture et prévention des injections SQL (`SQLi`).
11. **Module 10 & Liens de Téléchargement** : Liens officiels vers les jeux de données *Chinook* et *Sakila*.

---

## 🛠️ Installation et Préparation de l'Environnement

Pour exécuter les requêtes et réaliser les exercices :

### Option 1 : Sans installation (dans le navigateur)
* Rendez-vous sur **SQL-Practice Online** ou **W3Schools SQL Editor**.

### Option 2 : Sur votre ordinateur avec DBeaver & SQLite (Recommandé)
1. Téléchargez et installez **DBeaver Community** (gratuit et universel).
2. Téléchargez la base de données **Chinook** (fichier `Chinook_Sqlite.sqlite`) depuis le dépôt officiel GitHub (`lerocha/chinook-database`).
3. Ouvrez DBeaver > **Nouvelle connexion** > **SQLite** > Sélectionnez le fichier `Chinook_Sqlite.sqlite`.
4. Ouvrez un nouvel éditeur SQL (`Ctrl + Shift + E`) et commencez à pratiquer !

---

## 🏋️‍♂️ Exercices Pratiques

Consultez le fichier [`EXERCICES.md`] pour accéder aux travaux pratiques basés sur la base de données **Chinook**. Les exercices sont répartis en 3 niveaux :
* **Niveau 1 : Débutant** (Sélection, filtrage, tri)
* **Niveau 2 : Intermédiaire** (Agrégations, `GROUP BY`, Jonctions `JOIN`)
* **Niveau 3 : Avancé** (Sous-requêtes, `CASE WHEN`, Fenêtrage)

---

## 📜 Licence & Contribution

Ce projet est sous licence MIT. Les contributions (corrections, ajouts d'exercices) sont les bienvenues via Pull Request !
