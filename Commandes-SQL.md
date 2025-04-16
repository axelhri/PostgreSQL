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
