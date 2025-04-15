## **Commandes SQL**

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

**Création de base de données :**

```SQL
CREATE DATABASE nom_de_bdd;
```

s
**Création de table :**

```SQL
CREATE TABLE "User"(
   user_id UUID PRIMARY KEY,
   user_name VARCHAR(20) NOT NULL UNIQUE,
   user_mail VARCHAR(64) NOT NULL UNIQUE,
   user_password VARCHAR(128) NOT NULL
);
```

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
