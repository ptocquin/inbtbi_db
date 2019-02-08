---
title: Bases de donnees - Conception & Utilisation
author: Pierre Tocquin <ptocquin@uliege.be>
date: Février 2019
---

# Data Definition Language (DML)

Comme évoqué au chapitre précédent, le ??DDL?? comprend un ensemble de commandes ou déclarations qui permettent d'agir sur la structure de la base de données. En particulier, il sera utilisé pour créer ou supprimer les tables, les colonnes des tables, ajouter ou modifier des contraintes.

## Création d'une table

% La commande suivante crée une table portant le nom `MY_TABLE` et 5 colonnes, chacune imposant un type de donnée différent.

```sql
sqlite> create table MY_TABLE ( COL1    char(10),
                                COL2    decimal(9,2),
                                COL3    text(),
                                COL4    varchar(255),
                                COL5    int()
                                );
```

% Le langage SQL est capable de prendre en considération un grand nombre de types de données. Cependant, chaque SGBD définit son propre jeu de contraintes en ce qui concerne les types de données qu'il est capable de gérer. Dans le cas de SQLite et contrairement à d'autres SGBD tels que MySQL, la définition des types de colonne n'est pas contraignante, mais s'apparente plutôt à fixer une *préférence* ou *affinité* quant aux types de données qu'elles contiennent.

SQLite considère uniquement 6 *affinités* de types de données: `TEXT`, `INTEGER`, `FLOAT`, `NUMERIC` et `BLOB`. Ces affinités sont déduites des types de valeurs spécifiées dans la déclaration de création de la table. Par exemple, toute colonne dont le type données contient la chaine de caractères "char" recevra l'affinité `TEXT`. Pour plus de précision, consultez [https://www.sqlite.org/datatype3.html](https://www.sqlite.org/datatype3.html).

------

### Les identifiants primaire et secondaires

% Pour rappel, un identifiant est utilisé pour *identifier* de manière non équivoque chaque élément d'une table. Par principe, tout identifiant est donc unique dans la table dans laquelle il joue ce rôle. La définition de l'identifiant primaire s'obtient simplement de la manière suivante:

```sql
sqlite> create table MY_TABLE ( COL1    char(10),
                                COL2    decimal(9,2),
                                COL3    text(),
                                COL4    varchar(255),
                                COL5    int(),
                                primary key (COL1)
                                );
```

% Si la table contient un ou des identifiants secondaires (c'est à dire d'autres colonnes contenant des valeurs uniques), ils pourront être référencés à l'aide de la clause `unique()`:

```sql
sqlite> create table MY_TABLE ( COL1    char(10),
                                COL2    decimal(9,2),
                                COL3    text(),
                                COL4    varchar(255),
                                COL5    int(),
                                primary key (COL1),
                                unique (COL5)
                                );
```

------

### Les clés étrangères

% Les clés étrangères sont créées en utilisant la clause `foreign key` tel qu'illustré ci-dessous:

```sql
sqlite> create table publication   (pubmed_id   INTEGER,
                                    title       TEXT,
                                    journal     TEXT,
                                    year        TEXT,
                                    primary key (pubmed_id)
                                );
sqlite> create table authors       (first_name  TEXT,
                                    last_name   TEXT,
                                    institution TEXT,
                                    pub_id      INTEGER,
                                    primary key (first_name, last_name, institution),
                                    foreign key (pub_id) references publication
                                );
```

% La déclaration `foreign key (pub_id) references publication` signifie que la colonne `pub_id` de la table `authors` contiendra des références à la clé primaire (soit la colonne `pubmed_id`) de la table qu'elle cible, soit `publication`.

------

### Forme synthétique des contraintes

% Il est possible de définir des contraintes directement sur la ligne de définition de la colonne. C'est la cas pour la définition des clés primaire, secondaires et étrangères (à condition qu'elles ne soient pas composées de plusieurs colonne). Par ailleurs, d'autres contraintes peuvent être spécifiées sur chaque ligne: il s'agit en particulier du caractère obligatoire de la donnée qui s'obtient en ajoutant la clause `not null`. **Avec SQLite, on ajoutera sytématiquement cette clause à la colonne clé primaire car elle n'est pas implicite**. 

% Il est également possible d'imposer une valeur par défaut à la colonne dans le cas où lors de la création de l'enregistrement la valeur de la colonne en question est omise. Cela s'obtient avec la clause en ligne `default` suivie de la valeur à assigner.

% Ainsi l'exemple précédent pourrait être ré-écrit comme suit:

```sql
sqlite> create table publication   (pubmed_id   INTEGER not null primary key,
                                    title       TEXT not null,
                                    journal     TEXT,
                                    year        TEXT
                                );
sqlite> create table authors       (first_name  TEXT not null,
                                    last_name   TEXT not null,
                                    institution TEXT not null default 'Liège Université',
                                    pub_id      INTEGER references publication,
                                    primary key (first_name, last_name, institution)
                                );
```

------

## Suppression d'une table

% Pour supprimer une table et son contenu, il suffit d'utiliser la commande suivante:

```sql
sqlite> drop table authors;

```

% Cette commande différe de la suivante qui, elle, ne supprime que le contenu et qui peut être complétée par une clause `WHERE` :

```sql
sqlite> delete from authors where institution = 'Liège Université';
sqlite> delete from authors;

```


