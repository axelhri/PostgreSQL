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

`CREATE OR REPLACE FUNCTION` : Crée une nouvelle fonction appelée get_director_movie, ou la remplace si elle existe déjà.

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
