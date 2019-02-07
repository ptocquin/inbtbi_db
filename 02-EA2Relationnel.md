---
title: Bases de donnees - Conception & Utilisation
author: Pierre Tocquin <ptocquin@ulg.ac.be>
date: Octobre 2015
---

## Restructuration, Normalisation

% L'établissement d'un schéma conceptuel de base de données par l'approche que nous avons vue au chapitre précédent permet d'établir la structure provisoire de la base. A ce stade, cette structure est rarement complètement optimale; l'application de quelques règles simples permet en général d'améliorer significativement le schéma.

------

### Simplification

L'analyse minutieuse du schéma provisoire conduit généralement à la détection de structures qui peuvent faire l'objet de simplifications immédiates. Deux cas *classiques* sont à mentionner:

- Lorsqu'un type d'entités à attribut unique est associé à un seul type d'entités dans une relation 1-N, il est logique d'intégrer cet attribut au type d'entités associé (Figure \ref{schema4}).

------

![**Simplification des entités à attribut unique**. \label{schema4}](FIGS/schema4.png)

------

- Lorsqu'un type d'entité sans attribut est en lien avec 2 autres, il est convertit en association.

------

### Elimination des redondances ou ??Normalisation??

On distingue jusqu'à 8 formes normales (FN) mais seules les 3 premières sont régulièrement utilisées. Elles visent essentiellement à supprimer les redondances au sein de la base et donc à limiter l'espace occupé par la base. Il s'agit également de limiter les incohérences de données qui peuvent rendre la base inutilisable.

% Evaluation de notre schéma par rapport aux définitions des formes normales 1, 2 et 3:

------

#### ??Première forme normale?? ??1FN?? 

On dit d'un schéma de base de données qu'il est en première forme normale lorsque les types d'entités qui le composent disposent d'attributs *atomiques* (non décomposables, Figures \ref{FA} et \ref{FAb})). 

------

![**Forme non normale**. L'attribut `Référence` est visiblement décomposable en `id`, `journal`, `éditeur` et `année`. L'attribut `Auteur` est multi-valué\label{FA}](FIGS/FA.png)

------

![**Vers la 1FN par la décomposition des attributs non-atomiques**. Les attributs multi-valués peuvent donner lieu à la création d'un nouveau type d'entités \label{FAb}](FIGS/FAb.png)

------

#### Deuxième et troisième forme normale ??2FN??, ??3FN??

Les deuxième et troisième formes normales concernent les types d'entités dont les clés sont composées de plusieurs attributs. **Pour être en deuxième forme normale**, le schéma soit être en 1FN *et* tous les attributs non-clés doivent dépendre de la clé entière (Figures \ref{2FN} et \ref{2FNb}).

------

![**1FN mais pas 2FN**. L'identifiant du type d'entités `Auteurs` pourrait être composé des attributs `Nom` et `Institution`. Dans le cas représenté, les attributs `Prénom de l'auteur` et `Pays de l'institution` ne sont alors dépendant chacun que d'une partie de la clé: `Prénom` est dépendant de `Nom` tandis que `Pays` est dépendant de `Institution`. \label{2FN}](FIGS/2FN.png)

------

![**Vers la 2FN**... En extrayant les attributs non reliés à l'identifiant pour en faire un nouveau type d'entités. \label{2FNb}](FIGS/2FNb.png)

------
   
 **Pour être en troisième forme normale**, le schéma doit être en 2FN *et* tous les attributs doivent être en dépendance **directe** avec la clé. Cela signifie qu'aucun attribut ne peut dépendre d'un attribut non clé.

------

![**2FN mais pas 3FN**. L'attribut `éditeur` n'est pas relié directement à la clé puisque que, fonctionnellement, il dépend en premier chef de l'attribut `journal`. \label{3FN}](FIGS/3FN.png)

------

![**Vers la 3FN**... en créant, à nouveau, un nouveau type d'entités. \label{3FNb}](FIGS/3FNb.png)

------

## Production du schéma

------

### Représentation des Entités (Figure \ref{schema1})

- Une Entité = une table 

------

### Représentation des Attributs (Figure \ref{schema1})

- Un Attribut devient une colonne de la table
- Définir un domaine de valeurs (num, char, ...)
- Mentionner les cardinalités (la cardinalité *obligatoire* [1-1] est implicite est n'est donc pas représentée)

------

![**Conversion des entités en tables et colonnes**. \label{schema1}](FIGS/schema1.png)

-----

### Représentation des Associations

- 1:N
    - Création d'une clé étrangère côté `1` (import de l'identifiant de l'autre entité) (Figure \ref{schema2} et \ref{schema3})

------

![**Représentation d'un type d'associations 1-N**. \label{schema2}](FIGS/schema2.png)

------

![**Traduction d'un type d'associations 1-N par la création d'une clé étrangère**. \label{schema3}](FIGS/schema3.png)

-----

- 1:1
    - Création d'une clé étrangère dans l'une des 2 tables, au choix si la relation est facultative des 2 côtés ou dans la table pour laquelle la relation est obligatoire.
    - Cette clé étrangère devient aussi un identifiant secondaire

------

- N:N
    - Création d'une nouvelle entité 1:N
    - Reprise des règles 1:N

