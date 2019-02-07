---
title: Bases de donnees - Conception & Utilisation
subtitle: Formation INBTBI 2016-2017
author: Pierre Tocquin <ptocquin@ulg.ac.be>
date: Février 2017
logo: FIGS/logo1.jpg
---

# Introduction et concepts de base

------

## Définitions

Une *??donnée??* est une information (nom, âge, ...), mais elle peut aussi être une **??relation??** entre plusieurs informations.

% On comprend aisément que l'identifiant d'un gène et d'une protéine sont des informations/données. Il en va cependant de même pour la relation qui lie un gène et une protéine *qu'il code*.

Une *??base de données??* est donc un ensemble (volumineux) d'informations **structurées** et mémorisées sur un support physique.

------

## Structure d'une base de données

Il est courant de regrouper des données sous forme d'un tableau et d'utiliser  un *??tableur??* comme interface de création et de gestion de *bases de données*.

% Dans ces tableaux, chaque colonne correspond généralement à *un type de donnée* et chaque ligne à *un ??enregistrement??*. Il est relativement facile de créer ce type tableau via le *tableur* ou de manière programmatique tel que nous l'avons expérimenté lors du module d'Introduction à Linux et à la ligne de commande. Prenons l'exemple d'un tableau créé avec les données extraites de fichiers de séquences au format `genbank` (Figure \ref{db2}):

------

![**Base de données structurée créée dans un `tableur` sous forme d'une table unique**. \label{db2}](FIGS/db2.png)

------

% Si ce type de structure est conforme à la définition d'une base de données, elle accumule pourtant un nombre important de défauts qui rendent son emploi peu recommandable:

- **Lourdeur de l'accès aux données et de leur manipulation**

% Imaginez en effet un tableau qui compterait plusieurs milliers, voire des millions de lignes ? Comment facilement retrouver une donnée, comment en ajouter en évitant les doublons, comment modifier une information qui concernerait de nombreuses lignes, ... ?

- **Sécurité**

% Une structure aussi simple peut être facilement corrompue. Supprimer une ligne, une partie de ligne ou une colonne peut conduire à la destruction pure et simple de la totalité de l'information contenue dans la base.

- **Impossibilité de travailler à plusieurs simultanément sur la base de données (concurrence d'accès)**

% Ces défauts majeurs impliquent d'avoir recours à des logiciels spécialisés pour la gestion de données, les **SGBD** (Systèmes de Gestion de Bases de Données).

------

## SGBD

Un *??SGBD??* est chargé de gérer les fichiers constituant une base de données, de prendre en charge les fonctionnalités de protection et de sécurité, et de fournir les différents types d’interfaces nécessaires à l’accès aux données.

% De manière concrète, le SGBD a notamment pour fonction de :

- Organiser les données et vérifier leur qualité (unicité, respect des contraintes)
- Autoriser l'ajout et la modification des données tout en maintenant la qualité de la base.
- Permettre l'accès aux données et l'interrogation de la base via un langage intuitif.
- Protéger les données contre leur perte ou dégradation.

------

L'utilisation d'un SGBD implique de comprendre :

- la manière dont la base de données est structurée.

% ^[ On parle de la définition du schéma de données qui fait appel à différents modèles de données, tel que le ??modèle Entité-Association?? que nous découvrirons plus loin.]

- Les opérations qui peuvent être appliquées à la base de données: **création, modification, suppression et recherche**.

% ^[L'opération la plus complexe est la *recherche* de données car elle peut faire intervenir de nombreux critères en interaction les uns avec les autres. Elle nécessite par ailleurs un *langage* adapté. Le langage le plus répandu est le `??SQL??` (**S**tructured **Q**uery **L**anguage); c'est lui que nous utiliserons pour interroger nos bases de données.]

------

# Modèles de données

## Pourquoi structurer une base de données ?

------

% Afin d'illustrer pourquoi il est essentiel de structurer correctement une base de données, reprenons notre exemple de tableau et envisageons les risques encourus lors de différentes opérations.

### Insertion

Les erreurs de typographie ou des conventions d'écriture constituent une source importante de problèmes dans l'utilisation d'une base de données. Imaginez qu'en fonction de l'utilisateur, le nom d'une même espèce soit orthographié différemment: 

- Arabidopsis thaliana, A. thaliana, A thaliana, Arabidospis, ...

% Chacun des enregistrements contenant une variante de ce nom d'espèce sera considéré comme appartenant à une espèce différente. La recherche de données concernant cette espèce sera donc largement biaisée.

------

### Modification

A supposer que nous voulions ré-orthographier un nom d'espèce, il est, dans la même logique, nécessaire de modifier chaque enregistrement qui concerne l'espèce en question et cela sans introduire d'erreurs de typographie.

------

### Suppression

Si un seul gène est répertorié pour une espèce particulière dans notre base de données, la suppression de l'enregistrement relatif à ce gène conduira également à la suppression d'autres informations concernant cette espèce: taxon, ...

------

## Comment structurer une base de données ?

% La solution pour éviter ces problèmes repose sur 3 concepts :

- Séparer les informations dans des tableaux différents afin qu'une action sur l'une n'entraîne pas forcément une action sur l'autre (Figure \ref{sgdb1}).
- Définir une méthode d'identification des éléments d'un tableau afin de s'assurer de leur unicité.
- Préserver les liens qui existent entre les différents tableaux ainsi créés (Figure \ref{sgdb2}).

------

![**Séparation des informations et création d'un identifiant unique**. Cette tranformation permet de supprimer ou de limiter la répétition des mêmes informations sur plusieurs lignes. L'unicité de chaque ligne (ou enregistrement) qui en résulte conduit fréquemment à l'ajout d'une étiquette numérique (ou identifiant) qui facilite le référencement des données sous la forme 'tableau > numéro de ligne'.  \label{sgdb1}](FIGS/sgdb1.png)

------

![**Préservation des liens**. La structuration des données sous la forme de plusieurs tableaux indépendants implique que le lien qui existait au sein d'une même ligne dans le tableau global est rompu. Il est donc nécessaire de recréer ce lien en utilisant le référencement 'tableau > numéro de ligne'. \label{sgdb2}](FIGS/sgdb2.png)

------

## Le ??modèle Entité-Association??

% La modélisation utilisant le concept d'entités et d'associations est un outil permettant de concevoir la structure d'une base de données afin d'intégrer, dès l'origine, les bons principes énoncés ci-dessus.

Ce modèle fournit le moyen de définir un schéma conceptuel de la base de données que l'on souhaite construire. Il permet de décrire plus naturellement les informations et les relations entre celles-ci sans se préoccuper initialement de la manière dont elles seront structurées en tables et colonnes.

------

### Les ??types d'entités??

Il s'agit de tout groupe d'éléments identifiable du domaine d'application (contexte) pour lequel on souhaite établir une base de données (Figure \ref{entites}). Dans notre exemple, il peut s'agir des Organismes, des Gènes, des Protéines, ... Chaque élément particulier appartenant à un type d'entités est appelé *entité*.

------

![**Représentation symbolique des types d'entités**. \label{entites}](FIGS/entites.png)

------

### Les ??Attributs??

Les entités sont caractérisées par des propriétés (Figure \ref{EA2}). Il s'agit par exemple du nom et du numéro du taxon pour le type d'entités `Organismes`. Les attributs se définissent en fonction de leur *type* (numérique, caractère, date, ...) et leur *cardinalité* (faculatif [0-1], obligatoire [1-1])

------

![**Représentation des types d'entités et des attributs à l'aide du système de modélisation ??UML?? (Unified Modeling Language)**.\label{EA2}](FIGS/EA2.png)

------

### Les ??Identifiant??s

Un *identifiant*, ou ??clé??, est un attribut ou un ensemble d'attributs qui permet(tent) d'identifier de manière unique un élément d'un type entités.
% Si aucun des attributs (seul ou en combinaison avec d'autres) ne suffit à distinguer chaque élément d'un type d'entités, il est nécessaire de créer un attribut dont la fonction sera de servir de *clé*.

% Il peut arriver qu'un même type d'entités possède plus d'un identifiant. Dans ce cas, l'un d'entre eux est désigné comme **primaire** et les autres *secondaires*. Notez que les identifiants primaires ont naturellement un caractère obligatoire.

% Nous reviendrons ultérieurement sur cette notion d'identifiant et la manière de les définir.

------

### Les ??types d'Associations??

Les relations qui existent entre différentes entités sont qualifiées d'*associations* (Figure \ref{EA1}). Ces associations se traduisent généralement intuitivement par des actions *menées* ou *subies* par une entité. Dans notre exemple, on peut par exemple définir les associations suivantes :

- Un organisme **possède** un gène
- Un gène **code pour** une Protéine

------

![**Représentation UML de 2 types d'entités reliés par un type d'associations**. \label{EA1}](FIGS/EA1.png)

------

Tout comme pour les attributs, on distingue plusieurs types d'associations en fonction de leur classe fonctionnelle et de leur caractère obligatoire ou non. Il existe 3 classes fonctionnelles de types d'associations:

- un-à-plusieurs
- un-à-un
- plusieurs-à-plusieurs

------

### Type d'associations un-à-plusieurs (1:N)

Cela peut être le cas de l'association qui lie les `Organismes` aux `Gènes`: un organisme possède plusieurs gènes, mais chaque gène n'existe que chez un seul organisme (tout dépend bien sûr de l'information contenue dans le type d'entités `Gènes`) (Figures \ref{entites2} et \ref{EA3}).

------

![**Illustration d'un type d'associations un-à-plusieurs**. Un organisme possède des (N) gènes, tandis qu'un gène est possédé par un seul (1) organisme. \label{entites2}](FIGS/entites2.png)

------

![**Représentation UML d'un type d'associations un-à-plusieurs**. La représentation du caractère quantitatif d'un type d'association (voir plus loin, *??cardinalité??*) peut sembler contre-intuitive: la logique suivie est qu'on indique sur chaque branche du type d'associations, le nombre maximum d'associations dans lesquelles une entité peut apparaître (en d'autres termes, le nombre de flèches que l'on peut tracer au départ d'un élément de ce type d'entités) \label{EA3}](FIGS/EA3.png)

------

### Type d'associations un-à-un (1:1)

Si on considère la relation entre un type d'entités `mRNA` et un type d'entités `Protéines`, l'association **traduction** est, en simplifiant, de type un-à-un (Figure \ref{entites3}).

------

![**Illustration d'un type d'associations un-à-un**. \label{entites3}](FIGS/entites3.png)

------

### Type d'associations plusieurs-à-plusieurs (N:N)

% Prenons l'exemple de la relation existant entre des publications et des gènes auxquelles elles font référence. Il est imaginable qu'une publication puisse faire référence à plusieurs gènes et, à l'inverse, qu'un seul gène puisse être cité dans plusieurs publications (Figures \ref{sgdb3} et \ref{EA4}).

------

![**Illustration d'un type d'associations plusieurs-à-plusieurs**. \label{sgdb3}](FIGS/sgdb3.png)

------

![**Représentation UML d'un type d'associations plusieurs-à-plusieurs et de sa cardinalité**. \label{EA4}](FIGS/EA4.png)

------

### Caractère facultatif ou obligatoire d'une association

On peut définir que certaines entités **doivent** obligatoirement participer à une association. C'est par exemple le cas de la relation **possède** qui relie `Organismes` à `Gènes`: tout organisme possède forcément, au minimum, **un** gène. Ce caractère obligatoire sera traduit par une ??cardinalité?? composée d'un couple *min*-*max* du type **[1-N]** ou **[1-1]** (Figure \ref{EA5}).

------

![**Représentation UML de la cardinalité d'un type d'associations obligatoire**. \label{EA5}](FIGS/EA5.png)

------

A l'inverse, lorsqu'une relation est facultative, un gène n'est pas forcément traduit en protéine, la composante *min* de la cardinalité prendra la valeur 0 et sera du type **[0-N]** ou **[0-1]** (Figure \ref{EA6}).

------

![**Représentation UML de la cardinalité d'un type d'associations facultative**. \label{EA6}](FIGS/EA6.png)

------

# Du modèle E-A au ??modèle relationnel??

## Elaboration d'un ??schéma conceptuel??

------

%  Cette partie du développement d'une base de données se base uniquement sur la **description** ou formulation du contexte (domaine d'application). Bien que cela puisse paraître *simpliste*, c'est une étape essentielle qui permet généralement d'éviter bon nombre d'erreurs. En outre, elle a le mérite d'être peu technique: c'est donc cette approche que l'on peut recommander au novice. La **conceptualisation** du contexte passe par 3 étapes:

### Traduction du contexte en **propositions élémentaires**

Dans les exemples rencontrés précédemment, nous avons déjà rencontré des propositions élémentaires telles que *un organisme possède un gène* ou *un mRNA code pour une protéine*. Ces propositions sont qualifiées de *binaires* car elles sont constituées de 2 éléments (sujet et objet) reliés entre eux par un verbe (*sujet* **verbe** *objet*)
	
On comprend aisément que la traduction de ces propositions binaires dans un schéma de type E-A sera facilement réalisée en convertissant le sujet et l'objet en types d'entités et le verbe en un type d'associations. Pour que cela soit réalisable, il faut cependant s'assurer que le propositions soient *générales* et non *particulières*: *un organisme possède un gène* est une proposition générale tandis que *Arabidopsis thaliana possède le gène AtFT* est une proposition particulière qui ne sera pas en mesure de rendre du caractère générique que doivent prendre les types d'entités et les types d'associations sous-jacents.

------

### Traduction des cardinalités

Les cardinalités qui régissent des types d'association peuvent être déduites en posant les 2 question:

- Pour un *sujet*, combien trouve-t-on d'exemplaires d'*objets*, au minimum et au maximum.
- Pour un *objet*, combien trouve-t-on d'exemplaires de *sujets*, au minimum et au maximum.

`Tout organisme possède au minimum 1 gène; Tout gène appartient à un seul organisme`

------

### Traduction des attributs

Les attributs peuvent être déduits de manière tout aussi simple en définissant ce que possède (verbe avoir) les entités:

- *sujet* **a** *objet* >> *entité* **a** *attribut*

------

## Dessiner un schéma conceptuel

### Outils

- La distribution Ubuntu/Bio-Linux dispose, dans son catalogue de logiciels, d'un petit utilitaire permettant de concevoir différentes sortes de schémas: `??Dia??`. Il contient un module d'aide à la conception d'organigrammes de type `??UML??` (Unified Modeling Language) qui s'adapte très bien à la réalisation d'un schéma Entités-Associations.
- Le logiciel ??DB-MAIN?? dispose quant à lui non seulement des outils d'aide à la conception des schémas mais il permet de traduire automatiquement ces schémas en code ??SQL?? permettant la création de la base de données. Il est téléchargeable gratuitement sur le site [www.db-main.be](http://www.db-main.be).

% ^[Après avoir décompressé le fichier *dbm-92b-linux-setup*, il est nécessaire de configurer certaines variables d'environnement. Pour ce faire, suivez les indications mentionnées dans le fichier *readme.html*.]

------

### Exercices

------

1. Traduisez le descriptif suivant en propositions binaires, puis réalisez-en le schéma:
  - Des individus appartenant à une espèce d'oiseaux invasive ont été bagués afin d'en étudier le comportement. Lorsqu'un oiseau est bagué, les données suivantes sont enregistrées: le code de sa bague, son sexe, son année de naissance lorsqu'elle est connue et l'identité de son partenaire s'il est connu.
  - Plusieurs personnes de différents pays composent l'équipe en charge de leur observation.
  - Celle-ci s'étendra sur plusieurs années afin de pouvoir multiplier les observations concernant les mêmes individus en différents lieux.
  - Lorsqu'un oiseau est observé, l'identité de l'observateur, le lieu (localité, pays) et la date de l'observation sont enregistrées.
  
------

2. Sur base des énoncés suivants, produisez le schéma de la base de données.

  - Les gènes appartiennent à un organisme
  - Les gènes sont référencés par des publications
  - Un gène peut être transcrit en différents RNAs
  - Chaque RNA est éventuellement traduit en protéine. 
  - Les gènes possèdent un numéro d'accession unique
  - Les gènes ont une séquence qui peut être de type génomique ou mRNA
  
------

  - Les organismes dispose d'un numéro de taxon, d'un nom et d'un nom commun uniques
  - Les organismes possède un parent direct dans la classification taxonomique
  - Les publications sont référencées par un numéro pubmed unique
  - Les publications ont une source (journal), une date, un auteur (premier auteur) et une url
  - Les auteurs ont un nom
  - Les protéines ont un nom, un numéro d'accession et une séquence uniques


