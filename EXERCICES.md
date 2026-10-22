# 🏋️‍♂️ Travaux Pratiques ANSI SQL : Base de Données Chinook

Ce document contient **10 exercices pratiques progressifs** basés sur la base de données relationnelle **Chinook** (gestion d'un magasin de musique numérique).

---

## 📊 Schéma Simplifié de la Base Chinook

* **`Artist`** (`ArtistId`, `Name`)
* **`Album`** (`AlbumId`, `Title`, `ArtistId`)
* **`Track`** (`TrackId`, `Name`, `AlbumId`, `MediaTypeId`, `GenreId`, `Composer`, `Milliseconds`, `Bytes`, `UnitPrice`)
* **`Genre`** (`GenreId`, `Name`)
* **`Customer`** (`CustomerId`, `FirstName`, `LastName`, `Company`, `City`, `State`, `Country`, `Email`)
* **`Invoice`** (`InvoiceId`, `CustomerId`, `InvoiceDate`, `BillingCountry`, `Total`)
* **`InvoiceLine`** (`InvoiceLineId`, `InvoiceId`, `TrackId`, `UnitPrice`, `Quantity`)

---

## 🟢 Niveau 1 : Débutant (DML, Filtrage, Tri)

### Exercice 1.1 : Liste des clients français
**Objectif** : Sélectionner le prénom, le nom, la ville et l'adresse e-mail de tous les clients résidant en France (`Country = 'France'`).

<details>
<summary>💡 Voir la solution</summary>

```sql
SELECT FirstName, LastName, City, Email
FROM Customer
WHERE Country = 'France'
ORDER BY LastName ASC;
```
</details>

---

### Exercice 1.2 : Les 10 pistes les plus longues
**Objectif** : Afficher le nom de la piste (`Name`), le nom du compositeur (`Composer`) et la durée en millisecondes (`Milliseconds`) des 10 pistes les plus longues du catalogue.

<details>
<summary>💡 Voir la solution</summary>

```sql
SELECT Name, Composer, Milliseconds
FROM Track
ORDER BY Milliseconds DESC
FETCH FIRST 10 ROWS ONLY; -- Ou LIMIT 10 selon le SGBDR
```
</details>

---

### Exercice 1.3 : Recherche de pistes par motif
**Objectif** : Lister toutes les pistes dont le nom contient le mot `"Love"` (indépendamment de la casse).

<details>
<summary>💡 Voir la solution</summary>

```sql
SELECT TrackId, Name, UnitPrice
FROM Track
WHERE UPPER(Name) LIKE '%LOVE%';
```
</details>

---

## 🟡 Niveau 2 : Intermédiaire (Agrégations & Jonctions)

### Exercice 2.1 : Nombre de pistes par genre
**Objectif** : Compter le nombre total de pistes pour chaque genre de musique. Afficher le nom du genre et le nombre de pistes, trié du genre le plus représenté au moins représenté.

<details>
<summary>💡 Voir la solution</summary>

```sql
SELECT g.Name AS Genre, COUNT(t.TrackId) AS TotalTracks
FROM Genre g
INNER JOIN Track t ON g.GenreId = t.GenreId
GROUP BY g.GenreId, g.Name
ORDER BY TotalTracks DESC;
```
</details>

---

### Exercice 2.2 : Factures avec informations clients
**Objectif** : Afficher l'identifiant de la facture (`InvoiceId`), la date (`InvoiceDate`), le montant total (`Total`), ainsi que le prénom et le nom du client associé.

<details>
<summary>💡 Voir la solution</summary>

```sql
SELECT i.InvoiceId, i.InvoiceDate, i.Total, c.FirstName, c.LastName
FROM Invoice i
INNER JOIN Customer c ON i.CustomerId = c.CustomerId
ORDER BY i.InvoiceDate DESC;
```
</details>

---

### Exercice 2.3 : Chiffre d'affaires par pays
**Objectif** : Calculer la somme totale des ventes (`SUM(Total)`) par pays de facturation (`BillingCountry`), pour les pays ayant généré plus de 100 $ de chiffre d'affaires.

<details>
<summary>💡 Voir la solution</summary>

```sql
SELECT BillingCountry, SUM(Total) AS ChiffreAffaires
FROM Invoice
GROUP BY BillingCountry
HAVING SUM(Total) > 100
ORDER BY ChiffreAffaires DESC;
```
</details>

---

### Exercice 2.4 : Clients sans employé de support
**Objectif** : Lister tous les clients qui n'ont pas d'employé de support attribué (`SupportRepId IS NULL`).

<details>
<summary>💡 Voir la solution</summary>

```sql
SELECT CustomerId, FirstName, LastName, Email
FROM Customer
WHERE SupportRepId IS NULL;
```
</details>

---

## 🔴 Niveau 3 : Avancé (Sous-requêtes, CASE & Fenêtrage)

### Exercice 3.1 : Pistes plus longues que la moyenne
**Objectif** : Sélectionner le nom et la durée des pistes dont la durée (`Milliseconds`) est supérieure à la durée moyenne de toutes les pistes du catalogue.

<details>
<summary>💡 Voir la solution</summary>

```sql
SELECT Name, Milliseconds
FROM Track
WHERE Milliseconds > (
    SELECT AVG(Milliseconds) 
    FROM Track
)
ORDER BY Milliseconds DESC;
```
</details>

---

### Exercice 3.2 : Classification des factures par montant
**Objectif** : Catégoriser chaque facture selon son montant `Total` :
* `'Petite'` si Total < 5 $
* `'Moyenne'` si Total entre 5 $ et 15 $
* `'Grande'` si Total > 15 $

<details>
<summary>💡 Voir la solution</summary>

```sql
SELECT InvoiceId, CustomerId, Total,
    CASE 
        WHEN Total < 5 THEN 'Petite'
        WHEN Total BETWEEN 5 AND 15 THEN 'Moyenne'
        ELSE 'Grande'
    END AS CategorieFacture
FROM Invoice
ORDER BY Total DESC;
```
</details>

---

### Exercice 3.3 : Classement des pistes au sein de chaque album
**Objectif** : Assigner un numéro de rang (`ROW_NUMBER`) à chaque piste au sein de son album, ordonné par durée décroissante.

<details>
<summary>💡 Voir la solution</summary>

```sql
SELECT AlbumId, Name, Milliseconds,
       ROW_NUMBER() OVER(PARTITION BY AlbumId ORDER BY Milliseconds DESC) AS RangDuree
FROM Track;
```
</details>

---

