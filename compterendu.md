# Administration SQL
###### Compte-rendu ETTOUIL Adel - BTS SIO 1 - Groupe A

## Mise en place de l'environnement
### Installation de MySQL
**Sur le serveur :**        
Afin d'installer MySQL sur mon serveur, j'ai effectué cette commande :      
`apt install mysql-server mysql-client`     
**En local :**      
Afin de l'installer sur mon ordinateur personnel, je me suis rendu sur [le site officiel de MySQL](https://dev.mysql.com/downloads/windows/installer/8.0.html) afin de télécharger la dernière version.     
Lors de l'installation, j'ai selectionné "Full" afin d'avoir les produits MySQL client & serveur.        
Après avoir installé les "requirements", 
J'ai configuré le serveur MySQL (j'ai tout laissé par défaut, et j'ai mis un mot de passe).

### Connection & deconnection.
Je souhaite travailler sur le serveur, donc, afin de me connecter j'effectue la commande `mysql`, et pour me deconnecter, j'effectue la commande `exit`

### Gestion du SGBDR avec une interface graphique
Afin de gérer ma base de données avec une interface graphique, je décide d'utiliser phpmyadmin.     
Pour se faire, j'effectue la commande `apt install phpmyadmin`. Lors de l'installation, j'ai selectionné le serveur web "Apache2", et j'ai selectionné "Yes" lorsqu'il m'a demandé si je souhaité configurer le base de données pour phpmyadmin avec "dbconfig-common".     
J'ai ensuite mis un mot de passe pour phpmyadmin.

**Connexion**       
Afin de pouvoir me connecter à phpmyadmin, j'effectue les commandes suivantes sur mysql :       
```sql
CREATE USER '20ettouil'@'localhost' IDENTIFIED BY 'student';
GRANT ALL ON *.* TO '20ettouil'@'localhost' WITH GRANT OPTION;
FLUSH PRIVILEGES;
QUIT;
```

## Base de données
Afin de créer une base de données "test", j'effectue la commande suivante : 
```sql
CREATE DATABASE test;
```

### PROJET
Afin de sauvegarder les données du formulaire de selection, je vais utiliser MySQL, je crée donc une nouvelle table que je vais nommer "selection"
```sql
CREATE DATABASE selection DEFAULT CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
```
Si jamais je dois supprimer cette table, j'effectuerais la commande suivante :
```sql
DROP DATABASE selection;
```

## Tables
Voici le différents type de données que l'on peut trouver dans des tables

| Colonnes numériques    | Colonnes de textes | Colonnes temporelles |
| ---------------------- | ------------------ | -------------------- |
| TINYINT                | CHAR               | DATE                 |
| SMALLINT               | VARCHAR            | DATETIME             |
| MEDIUMINT              | TINYTEXT, TINYBLOB | TIMESTAMP            |
| INT, INTEGER           | TEXT, BLOB         | TIME                 |
| BIGINT                 | LONGTEXT, LONGBLOB | YEAR                 |
| FLOAT                  | ENUM               |                      |
| DOUBLE PRECISION, REAL | SET                |                      |
| DECIMAL                |                    |                      |

### Préparation de la requête pour la création de la table
Afin de définir les colonnes nécessaires et leurs type, j'effectue la commande suivante :
```SQL
CREATE TABLE Animal (
    id SMALLINT UNSIGNED NOT NULL AUTO_INCREMENT,
    espece VARCHAR(40) NOT NULL,
    sexe CHAR(1),
    date_naissance DATETIME NOT NULL,
    nom VARCHAR(30),
    commentaires TEXT,
    PRIMARY KEY (id)
)
ENGINE=INNODB;
```

Si je prends la deuxième ligne `id SMALLINT UNSIGNED NOT NULL AUTO_INCREMENT,` :        
La première valeur `id` définit le nom de la table, la seconde valeur `SMALLINT` définit le type de table, la troisième valeur `UNSIGNED` dis que la valeur ne peux pas être négative, la quatrième valeur `NOT NULL` dis que la valeure par défaut ne sera pas nulle, et `AUTO_INCREMENT` définis une auto-incrémentation donc la valeur id sera automatique 1, 2, 3, 4 et ça sera compté automatiquement.

A l'avant dernière ligne, on peux voir `PRIMARY KEY (id)` le fait de définir une clé primaire permet de définir une contrainte d'uincité (pas deux fois la même valeur), dans le cas suivant, cela permet de faire en sorte de ne pas avoir deux fois la même id.

Afin de vérifier que la base de données a bien été modifiée, je peux effectuer les commandes suivantes : 
```sql
SHOW TABLES; -- permet de voir les tables dans la base de données
-- qui me renvoie :
+----------------+
| Tables_in_test |
+----------------+
| Animal         |
+----------------+

DESCRIBE Animal; -- permet de voir la table
-- qui me renvoie :
+----------------+-------------------+------+-----+---------+----------------+
| Field          | Type              | Null | Key | Default | Extra          |
+----------------+-------------------+------+-----+---------+----------------+
| id             | smallint unsigned | NO   | PRI | NULL    | auto_increment |
| espece         | varchar(40)       | NO   |     | NULL    |                |
| sexe           | char(1)           | YES  |     | NULL    |                |
| date_naissance | datetime          | NO   |     | NULL    |                |
| nom            | varchar(30)       | YES  |     | NULL    |                |
| commentaires   | text              | YES  |     | NULL    |                |
+----------------+-------------------+------+-----+---------+----------------+
```
### PROJET
Après une étude du cahier des charges, je crée mes tables :     
Pour le portail d'identification :
```SQL
USE selection;

CREATE TABLE `users` (
    `id` SMALLINT UNSIGNED NOT NULL AUTO_INCREMENT,
    `username` VARCHAR(40) NOT NULL,
    `passwd` VARCHAR(40) NOT NULL,
    `accounttype` VARCHAR(40) NOT NULL,
    PRIMARY KEY (`id`)
    UNIQUE(`username`);
)
ENGINE=INNODB;
```

Ensuite, pour l'espace des évaluateurs :
```SQL
USE selection;

CREATE TABLE `candidat` (
    `id` SMALLINT NOT NULL AUTO_INCREMENT,
    `numerocandidat` MEDIUMINT NULL DEFAULT NULL,
    `prenom` TEXT NULL DEFAULT NULL,
    `nom` TEXT NULL DEFAULT NULL,
    `typebac` TEXT NULL DEFAULT NULL,
    `travailserieux` BOOLEAN NULL DEFAULT NULL,
    `absence` BOOLEAN NULL DEFAULT NULL,
    `attitude` BOOLEAN NULL DEFAULT NULL,
    `etudesup` BOOLEAN NULL DEFAULT NULL,
    `avispp` TEXT NULL DEFAULT NULL,
    `avisproviseur` TEXT NULL DEFAULT NULL,
    `lettremotiv` TEXT NULL DEFAULT NULL,
    `remarques` LONGTEXT NULL DEFAULT NULL,
    `notefinale` TINYINT(20) NULL DEFAULT NULL,
    `etatdossier` TEXT NULL DEFAULT NULL,
    `datecreation` DATETIME NULL DEFAULT NULL,
    PRIMARY KEY (`id`),
    UNIQUE (`numerocandidat`)
)
ENGINE=INNODB;
```

La commande pour supprimer une table est : 
```sql
DROP TABLE `formulaire`;
```

### Requête sur table existante :
Afin d'ajouter une colonne, j'effectuerais la commande suivante :

```sql
USE test;

ALTER TABLE `Animal`
ADD COLUMN `datetest` DATE NOT NULL;
```

Afin de modifier une colonne, j'effectuerais la commande suivante : 
```sql
USE test;

ALTER TABLE `Animal`
CHANGE `datetest` `datetest2` VARCHAR(10) NOT NULL;
```

Afin de supprimer une colonne, j'effectuerais la commande suivante :
```sql
ALTER TABLE `Animal`
DROP COLUMN `datetest2`
```

## Commandes de descriptions
Voici une liste de commandes de description, avec leur rôle.

`SHOW DATABASES` : Permet d'afficher les bases de données présentes sur le SGBDR        
`SHOW TABLES` : Permet d'afficher les tables présente dans une base de données      
`SHOW CHARCTER SET` : Permet d'afficher le jeu de caractère d'une table    
`SHOW GRANTS [FOR utilisateur]` : Permet d'afficher les droits pour `utilisateur`       
`SHOW WARNINGS` : Permet d'afficher les avertissements de MySQL     
`DESCRIBE nom_table` : Affiche le contenu du'une table      
`SHOW CREATE type_objet nom_objet \G` : Permet de vérifier si la syntaxe est correcte

## Données création
Afin d'insérer des données, on peux le faire sans définir chaque colonne, voici la commande qui sera effectuée :

```sql
USE test;

INSERT INTO Animal VALUES
    (1, 'chien', 'M', '2010-04-05 13:43:00', 'Rox', 'Mordille beaucoup');
```


Ensuite, en définissant les colonnes, (cela permet de supprimer une colonne, ou de changer l'ordre)
```sql
USE test;

INSERT INTO Animal (espece, sexe, date_naissance) VALUES
    ('tortue', 'F', '2009-08-03 05:12:00');
```

On peux égalment insérer plusieurs valeurs en une seule commande :
```sql
USE test;

INSERT INTO Animal (espece, sexe, date_naissance, nom) VALUES
    ('chien', 'F', '2008-12-06 05:18:00', 'Caroline'),
    ('chat', 'M', '2008-09-11 15:38:00', 'Bagherra'),
    ('tortue', NULL, '2010-08-23 05:18:00', NULL);```
```

Pour effectuer de grosses requête, on peux executer un ficheir externe, pour se faire, on utilise la commande :
```sql
SOURCE /home/monfichier.sql
```

### PROJET
Afin d'importer des données à partir d'un fichier .csv, il faut effectuer la commande suivante
```sql
LOAD DATA LOCAL INFILE 'animal.csv'
INTO TABLE Animal
FIELDS TERMINATED BY ';' ENCLOSED BY '"'
LINES TERMINATED BY '\n' -- ou '\r\n' selon l'ordinateur et le programme utilisés pour créer le fichier
(espece, sexe, date_naissance, nom, commentaires);
```

Et voici le contenu du fichier `animal.csv` :
```csv
"chat";"M";"2009-05-14 06:42:00";"Boucan";NULL
"chat";"F";"2006-05-19 16:06:00";"Callune";NULL
"chat";"F";"2009-05-14 06:45:00";"Boule";NULL
"chat";"F";"2008-04-20 03:26:00";"Zara";NULL
"chat";"F";"2007-03-12 12:00:00";"Milla";NULL
"chat";"F";"2006-05-19 15:59:00";"Feta";NULL
"chat";"F";"2008-04-20 03:20:00";"Bilba";"Sourde de l'oreille droite à 80%"
"chat";"F";"2007-03-12 11:54:00";"Cracotte";NULL
"chat";"F";"2006-05-19 16:16:00";"Cawette";NULL
"tortue";"F";"2007-04-01 18:17:00";"Nikki";NULL
"tortue";"F";"2009-03-24 08:23:00";"Tortilla";NULL
"tortue";"F";"2009-03-26 01:24:00";"Scroupy";NULL
"tortue";"F";"2006-03-15 14:56:00";"Lulla";NULL
"tortue";"F";"2008-03-15 12:02:00";"Dana";NULL
"tortue";"F";"2009-05-25 19:57:00";"Cheli";NULL
"tortue";"F";"2007-04-01 03:54:00";"Chicaca";NULL
"tortue";"F";"2006-03-15 14:26:00";"Redbul";"Insomniaque"
"tortue";"M";"2007-04-02 01:45:00";"Spoutnik";NULL
"tortue";"M";"2008-03-16 08:20:00";"Bubulle";NULL
"tortue";"M";"2008-03-15 18:45:00";"Relou";"Surpoids"
"tortue";"M";"2009-05-25 18:54:00";"Bulbizard";NULL
"perroquet";"M";"2007-03-04 19:36:00";"Safran";NULL
"perroquet";"M";"2008-02-20 02:50:00";"Gingko";NULL
"perroquet";"M";"2009-03-26 08:28:00";"Bavard";NULL
"perroquet";"F";"2009-03-26 07:55:00";"Parlotte";NULL
```

## Sélection
Pour sélectionner des données, on utilise la commande `SELECT` on peux selectionne une, ou plusieurs colonnes avec la commande : 
```sql
SELECT espece, sexe FROM Animal
```

Ou bien sélectionner toutes les colonnes avec la commande : 
```sql
SELECT * FROM Animal
```

### WHERE
On peux également restreindre les critères de recherche, par exemple si je veux ne selectionner que les chiens :
```sql
SELECT * FROM Animal WHERE espece = "chien";
```

### OPERATEURS DE SELECTION
On peux également utiliser des opérateurs de sélection, par exemple :
```sql
SELECT * FROM Animal WHERE date_naissance < '2008-01-01'; -- tout les animaux nés avant 2008
SELECT * FROM Animal WHERE espece != "chien"; -- tout les animaux saufs les chiens
```

### COMBINAISONS DE CRITERES
On peux également combiner les critères avec `AND`, `OR`, `XOR` ou `NOT`. Par exemple :
```sql
SELECT * FROM Animal WHERE espece = "chat" AND sexe = "F"; -- selectionne les chats femelles
SELECT * FROM Animal WHERE espece = "chat" OR espece = "chien"; -- selcetionne les chats & chiens
SELECT * FROM Animal WHERE sexe = "F" AND NOT espece = "chien"; -- selectionne toutes les femmelles sauf les chiennes
SELECT * FROM Animal WHERE sexe = "M" XOR espece = "perroquet"; -- selcetionne les animaux qui sont soit des mâles, soit des perroquets (mais pas les deux)
```

### NULL
On peux également demander les valeurs qui sont nulles :
```sql
SELECT * FROM Animal WHERE sexe IS NULL; -- selectionne tout les animaux où le sexe n'a pas été renseignés
```

### LIKE
`LIKE` permet de faire des recherches approximatives, par exemple           
`b%`  cherchera toutes les chaînes de caractères commençant par 'b'  ("brocoli", "bouli", "b").         
`b_`  cherchera toutes les chaînes de caractères contenant deux lettres dont la première est 'b'  ("ba", "bf", "b8").           
`%ch%ne`  cherchera toutes les chaînes de caractères contenant 'ch'  et finissant par 'ne'  ("chne", "chine", "échine", "le pays le plus peuplé du monde est la Chine").            
`p_rl_`  cherchera toutes les chaînes de caractères commençant par un "p" suivi d'un caractère, puis de "rl" et enfin se terminant par un caractère ("parle", "perla", "perle").            

### ORDER BY
On peux trier les données, dans un ordre ascendant (`ASC`) ou descendant (`DESC`), par exemple :
```sql
SELECT * FROM Animal WHERE espece = "chien"
ORDER BY date_naissance ASC;
```

On peux également trier sur plusieurs colonnes, par exemple :
```sql
SELECT * FROM Animal
ORDER BY date_naissance, espece ASC;
```
Le tri se fera d'abord sur le première colonne, puis sur la seconde.

### DISTINCT
Distinct permet de supprimer les doublons, par exemple :
```sql
SELECT DISTINCT espece FROM Animal;
```

### LIMIT
Permet de limiter les données selectionner :
```sql
SELECT * FROM Animal LIMIT 10;
```
Il ne selectionnera que 10 lignes.

On peux également définir un point de départ avec `OFFSET`, par exemple : 
```sql
SELECT * FROM Animal LIMIT 5 OFFSET 2;
```
Ici, il selectionnera 5 lignes après de la ligne 2

## Supprimer et modifier les données
Pour supprimer une ligne, on peux effectuer la commande `DELETE FROM`, si on veux par exemple supprimer l'animal "Zoulou" de la base de données, on effectuera la commande :
```sql
DELETE FROM Animal WHERE nom = "Zoulou"
```

Pour modifier une ligne, on va utiliser `UPDATE` et `SET`, par exemple si on veux modifier le sexe de "Pataud", on effectuera la commande :
```sql
UPDATE Animal SET sexe = "F" WHERE nom = "Pataud";
```

## Sauvegarde et restauration
Afin de sauvegarder une bdd, il faut utiliser `mysqldump`
### PROJET
Afin de sauvegarder ma bdd, j'effectue la commande :        
`mysqldump -u root -p --opt test > save.sql`

Ensuite, pour restaure la bdd, j'effectue la commande : `mysql test < save.sql`.

## Index
Un index est une structure qui reprend la liste ordonnée des valeurs auxquelles il se rapporte.     
Les index sont utilisés pour accélérer les requêtes (notamment les requêtes impliquant plusieurs tables, ou les requêtes de recherche), et sont indispensables à la création de clés, étrangères et primaires, qui permettent de garantir l'intégrité des données de la base et dont nous parlerons au chapitre suivant.

## Clés
Il y a des clés primaires et uniques dans la base de donnée "selection", dans la table "formulaire"

Pour supprimer une clé primaire, j'effectue la requête
```sql
ALTER TABLE `users` DROP PRIMARY KEY;
```

## Jointures

## Utilisateurs
Afin de créer un utilisateur qui a tout les droits, j'ai effectuer les requêtes SQL suivantes :
```sql
CREATE USER '20ettouil'@'localhost' IDENTIFIED BY 'student';
GRANT ALL ON *.* TO '20ettouil'@'localhost' WITH GRANT OPTION;
FLUSH PRIVILEGES;
```
