# Commandes SQL avancées

## Fonction

Une fonction PostgreSQL est un bloc de code SQL ou PL/pgSQL qui peut être stocké et exécuté sur le serveur de base de données. Elle permet de regrouper des instructions réutilisables pour effectuer des calculs, manipuler des données ou réaliser des opérations complexes. Les fonctions peuvent être appelées directement ou dans des requêtes SQL.

### Créer

```SQL
CREATE OR REPLACE FUNCTION get_director_movie(director_id UUID)
RETURNS TABLE(director_firstname VARCHAR, director_lastname VARCHAR ,movie_title VARCHAR, movie_release_date DATE) AS
$$
BEGIN
    RETURN QUERY
    SELECT d.director_firstname, d.director_lastname, m.movie_title, m.movie_release_date
    FROM director d
    JOIN movie m ON d.director_id = m.direcor_id
    WHERE d.director_id = p.director_id;
END;
$$ LANGUAGE plpgsql;
```

`CREATE OR REPLACE FUNCTION` : Crée une nouvelle fonction ou la remplace si elle existe déjà.

`get_director_movie` : Nom de la fonction.

`(director_id UUID)` : Nom de la colonne est son type.

`RETURNS TABLE` : Retourne une table avec les colonne indiqué entre les parentheses.

`$$` : Délimiteur pour indiquer le début du corps de la fonction.

`BEGIN` : Début du bloc d'instructions.

`RETURN QUERY` : Signifie que la fonction exécutera une requête SQL et renverra directement les résultats.

`END` : Fin du bloc BEGIN...END.

`$$ LANGUAGE plpgsql;` : LANGUAGE plpgsql indique que la fonction est écrite en langage procédural PostgreSQL (PL/pgSQL).

### Executer

```SQL
SELECT * FROM get_director_movie('uuid_du_realisateur');
```

## Procédure

Une procédure PostgreSQL est similaire à une fonction, mais elle ne retourne pas de valeur. Elle permet d'exécuter un ensemble d'instructions SQL ou PL/pgSQL pour réaliser des actions comme des mises à jour ou des manipulations de données sans renvoyer de résultat. Elle est utilisée principalement pour des opérations administratives ou des traitements complexes.

### Créer

```SQL
CREATE OR REPLACE PROCEDURE supprimer_utilisateur(utilisateur_id INT)
AS $$
BEGIN
    DELETE FROM utilisateurs WHERE id = utilisateur_id;
END;
$$ LANGUAGE plpgsql;
```

`CREATE OR REPLACE PROCEDURE` : Crée une nouvelle procédure ou la remplace si elle existe déjà.

`supprimer_utilisateur` : Nom de la procédure.

`(utilisateur_id INT)` : Nom de la colonne est son type.

`$$` : Délimiteur pour indiquer le début du corps de la fonction.

`BEGIN` : Début du bloc d'instructions.

`END` : Fin du bloc BEGIN...END.

`$$ LANGUAGE plpgsql;` : LANGUAGE plpgsql indique que la fonction est écrite en langage procédural PostgreSQL (PL/pgSQL).

### Executer

```SQL
CALL supprimer_utilisateur(5);
```

## Trigger

Une procédure PostgreSQL est similaire à une fonction, mais elle ne retourne pas de valeur. Elle permet d'exécuter un ensemble d'instructions SQL ou PL/pgSQL pour réaliser des actions comme des mises à jour ou des manipulations de données sans renvoyer de résultat. Elle est utilisée principalement pour des opérations administratives ou des traitements complexes.

### Créer

```SQL
CREATE OR REPLACE FUNCTION maj_solde_apres_transaction()
RETURNS TRIGGER AS $$
BEGIN
    UPDATE comptes
    SET solde = solde + NEW.montant
    WHERE id = NEW.compte_id;

    RETURN NEW;
END;
$$ LANGUAGE plpgsql;
```

`CREATE OR REPLACE FUCNTION` : Crée une nouvelle procédure ou la remplace si elle existe déjà.

`maj_solde_apres_transaction` : Nom de la fonction.

`RETURN TRIGGER` : Cette fonction retourne un TRIGGER, ce qui signifie qu'elle est conçue pour être utilisée avec un déclencheur (trigger).

`$$` : Délimiteur pour indiquer le début du corps de la fonction.

`BEGIN` : Début du bloc d'instructions.

`RETURN NEW` : La fonction retourne la nouvelle ligne insérée dans la table transactions (représentée par NEW), ce qui est nécessaire pour un trigger de type AFTER INSERT pour continuer le processus d'insertion.

`END` : Fin du bloc BEGIN...END.

`$$ LANGUAGE plpgsql;` : LANGUAGE plpgsql indique que la fonction est écrite en langage procédural PostgreSQL (PL/pgSQL).

```SQL
CREATE TRIGGER trigger_maj_solde
AFTER INSERT ON transactions
FOR EACH ROW
EXECUTE FUNCTION maj_solde_apres_transaction();
```

`CREATE TRIGGER` : Crée un nouveau trigger (déclencheur).

`trigger_maj_solde` : Nom du trigger.

`AFTER INSERT ON transactions ` : Le trigger se déclenche après qu'une insertion a été effectuée dans la table transactions.

`FOR EACH ROW` : Le trigger se déclenche pour chaque ligne insérée dans la table transactions. Cela signifie que pour chaque enregistrement inséré, la fonction maj_solde_apres_transaction sera appelée.

`EXECUTE FUNCTION maj_solde_apres_transaction()` : Cette ligne appelle la fonction maj_solde_apres_transaction, qui met à jour le solde du compte dans la table comptes en fonction des données de la nouvelle transaction insérée.
