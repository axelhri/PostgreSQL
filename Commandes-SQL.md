# Commandes SQL

## Création & gestion des rôles

**Création de rôle**

```SQL
CREATE ROLE username WITH LOGIN PASSWORD 'ton_mot_de_passe' SUPERUSER CREATEDB;
```

_A la suite de cela, si vous voulez vérifier que vous avez votre rôle bien attribuer, executer dans psql_ `\du`

**Modification d'un rôle**

```SQL
ALTER ROLE username WITH CREATEROLE;
```

_ou_

```SQL
ALTER ROLE username WITH REPLICATION;
```

_Il y a pleins d'autres rôle disponible dans psql_ (voir documentation officielle : [Différents rôles psql](https://docs.postgresql.fr/10/sql-createrole.html))

## Gestion des bases de données

**Création de base de données :**

```SQL
CREATE DATABASE nom_de_bdd;
```

## Gestion des tables

**Création de table :**

```SQL
CREATE TABLE "User"(
   user_id UUID PRIMARY KEY,
   user_name VARCHAR(20) NOT NULL UNIQUE,
   user_mail VARCHAR(64) NOT NULL UNIQUE,
   user_password VARCHAR(128) NOT NULL
);
```

## Insertion de données

**Insérer dans une table :**

```SQL
INSERT INTO "User" (user_id, user_name, user_mail, user_password)
VALUES (
   gen_random_uuid(),
   'axel',
   'axel@mail.com',
   'mypassword'
);
```

**Vérifier les éléments dans une table :**

```SQL
SELECT * FROM "User";
```

**Modifier des éléments dans une table :**

```SQL
UPDATE "User" SET user_mail = 'newmail@mail.com' WHERE user_name = 'axel';
```

**Supprimer une colonne précise :**

```SQL
DELETE FROM "User" WHERE user_name = 'axel';
```

**Supprimer une table :**

```SQL
DROP TABLE "User";
```

**Clé étrangère :**

```SQL
CREATE TABLE "Post"(
   post_id UUID PRIMARY KEY,
   post_title VARCHAR(50) NOT NULL,
   post_description VARCHAR(250) NOT NULL,
   user_id UUID NOT NULL UNIQUE,
   FOREIGN KEY (user_id) REFERENCES User(user_id)
);
```

_ou_

```SQL
ALTER TABLE "Post" ADD CONSTRAINT user_id FOREIGN KEY (user_id) REFERENCES "User"(user_id);
```

**Création d'index :**

```SQL
CREATE INDEX index_name ON table_name (column_name);
```

**INNER JOIN :**

```SQL
SELECT * FROM customer INNER JOIN transactions ON customer.customer_id = customers.customer_id;
```

**JOIN :**

```SQL
SELECT * FROM customer LEFT JOIN transactions ON customer.customer_id = transactions.customer_id;
```

Types de join :

- INNER JOIN
- LEFT JOIN
- RIGHT JOIN
- FULL JOIN

**Suppression d'une base de données**

```SQL
DROP DATABASE nom_de_bdd;
```

**Ajouter une colonne**

```SQL
ALTER TABLE "User" ADD COLUMN age INT;
```

**Supprimer une colonne**

```SQL
ALTER TABLE "User" DROP COLUMN age;
```

**Utilisation de BETWEEN pour une plage de valeurs**

```SQL
SELECT * FROM "User" WHERE age BETWEEN 18 AND 30;
```

**Utiliser LIKE pour une recherche partielle**

```SQL
SELECT * FROM "User" WHERE user_name LIKE 'ax%';
```

_ici 'ax%' cible les noms qui commenceront par ax_

**Ordre croissant (ASC) ou décroissant (DESC)**

```SQL
SELECT * FROM "User" ORDER BY user_name ASC;
```

```SQL
SELECT * FROM "User" ORDER BY age DESC;
```

**Limiter les résultats**

```SQL
SELECT * FROM "User" LIMIT 5;
```

**Ignorer les premiers résultats**

```SQL
SELECT * FROM "User" OFFSET 5 LIMIT 5;
```

**Supprimer une ligne spécifique**

```SQL
DELETE FROM "User" WHERE user_name = 'axel';
```

**Supprimer toutes les lignes d'une table**

```SQL
DELETE FROM "User";
```

## Agrégats & calculs

**Compter le nombre d'éléments**

```SQL
SELECT COUNT(*) FROM "User";
```

**Valeur maximum/minimum**

```SQL
SELECT MAX(age) FROM "User";
```

```SQL
SELECT MIN(age) FROM "User";
```

**Calcul de la moyenne**

```SQL
SELECT AVG(age) FROM "User";
```

**Calcul de la somme**

```SQL
SELECT SUM(age) FROM "User";
```

**Groupement des données**

```SQL
SELECT city, COUNT(*) FROM "User";
GROUP BY city;
```

**Groupement avec condition**

```SQL
SELECT city, COUNT(*) FROM "User";
GROUP BY city;
HAVING COUNT(*) > 2;
```

## Jointures

**Jointure entre deux tables**

_exemple entre les deux tables "User" et "Post"_

```SQL
SELECT "User".user_name, "Post".post_title
FROM "User"
JOIN "Post" ON "User".user_id = "Post".user_id;
```

**LEFT JOIN**

```SQL
SELECT "User".user_name, "Post".post_title
FROM "User"
LEFT JOIN "Post" ON "User".user_id = "Post".user_id;
```

## Alias

**Création d'alias**

```SQL
SELECT user_name AS nom_utilisateur FROM "User";
```

## Contraintes

**Contrainte CHECK**

```SQL
ALTER TABLE "User"
ADD CONSTRAINT age_check CHECK (age BETWEEN 18 AND 99);
```

## Dates

**Requêter par date**

```SQL
SELECT * FROM "User"
WHERE created_at >= '2024-01-01' AND created_at <= '2024-12-31';
```

## Tri multiple

**Trier plusieurs tables**

```SQL
SELECT * FROM "User"
ORDER BY age DESC, user_name ASC;
```

## Tri d'une valeur unique

**Récupérer chaque ville unique depuis la table User**

```SQL
SELECT DISTINCT city FROM "User";
```

_cela retournera une seule fois chaque valeur de city, même si plusieurs utilisateurs vivent dans la même ville_

## Filtre d'attributs

**Compter le nombre de personnes qui a exactement les mêmes valeurs**

_exemple : nombre de personnes qui ont la même date de naissance_

```SQL
SELECT customer_birth FROM customer GROUP BY customer_birth HAVING COUNT(*) > 1
```

Cette requête renverra les dates de naissance pour lesquelles il y a **plus d'une personne** _(c'est-à-dire des dates partagées par plusieurs individus)_. Si aucune date de naissance n'est partagée, la requête retournera un résultat **vide**. De plus, si le **COUNT** est supérieur à 2, mais qu'il y a **uniquement** 2 personnes avec la même date de naissance, la requête retournera également un résultat **vide**.

**Retrouver quelles sont les personnes qui ont les mêmes date de naissance**

```SQL
SELECT * FROM customer WHERE customer_birth IN (SELECT customer_birth FROM customer GROUP BY customer_birth HAVING COUNT(*) > 1)
```

## Mise à jour

**Mise à jour simple**

```SQL
UPDATE customer
SET name = 'Jean Dupont'
WHERE id = 1
```

**Avec conditions WHERE**

```SQL
UPDATE customer
SET status = 'inactive'
WHERE last_login < '2023-01-01'
```

**Avec jointures**

```SQL
UPDATE customer
SET email = e.new_email
FROM new_emails e
WHERE customer.id = e.customer_id
```

## Suppression

⚠️ **ATTENTION**, sans `WHERE` **TOUT est supprimé**. Très dangereux en cas d'oubli !

**Suppression simple**

```SQL
DELETE FROM customer
WHERE id = 5
```

**Avec condition WHERE**

```SQL
DELETE FROM customer
WHERE last_login < '2022-01-01'
```

**Avec jointures**

```SQL
DELETE FROM customer
USING blacklist
WHERE customer.email = blacklist.email
```

✅ `USING` = l'équivalent de `JOIN` pour les requêtes `DELETE`

**TRUNCATE en PostgreSQL**

Différence avec `DELETE`

| **DELETE**                         | **TRUNCATE**                         |
| ---------------------------------- | ------------------------------------ |
| Peut cibler des lignes précises    | Supprime **toutes les lignes**       |
| Peut être conditionnel (**WHERE**) | Pas de **WHERE**, tout est supprimé  |
| Déclenche les **triggers**         | Ne déclenche **pas** les triggers    |
| Plus lent, ligne par ligne         | Très rapide (suppression en masse)   |
| Peut être rollbacké                | Oui, si utilisé dans une transaction |
