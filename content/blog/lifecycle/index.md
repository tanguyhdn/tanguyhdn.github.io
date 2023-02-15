---
author: Tanguy Hodonou
categories:
- data science
- data mining
- EDA

date: "2023-01-15"
draft: false
excerpt: Description des étapes d'un projet data.

layout: single
subtitle: Les étapes d'un projet data
title: Data science lifecycle
---



## DEFINITION DES OBJECTIFS

La première tâche d'un projet de science des données consiste à définir un objectif mesurable et quantifiable. A ce niveau, on doit notamment poser le problème pratique, le traduire en une problématique data et définir un plan d’utilisation des résultats. Pour cela, il faut réunir autour d’une même table les experts du data mining et les décideurs. Les principaux points à traiter sont les suivants :

### Définir les objectifs pratiques du projet ?

Dans la majorité des cas, la réponse à cette question doit être apportée par les responsables de la structure qui initie le projet. Ces objectifs peuvent par exemple être la conquête de nouveaux clients, la reconquête d’anciens clients, l’identification des clients à risque pour l’entreprise ou encore la fidélisation de segments de clients. 
Après la définition explicite des objectifs, il convient de définir la population cible et l’unité statistique. La population cible peut par exemple être constituée de l’ensemble des clients de l’entreprise et l’unité statistique est le client.

### Traduire les objectifs pratiques en une problématique de data mining

Après la définition des objectifs pratiques, ils doivent être traduits en une problématique de data mining. Cela peut par exemple être un problème de classification, de prédiction, de scoring, de détection de fraude, etc.

### Prévoir un plan d’utilisation des résultats

Le plan d’utilisation des résultats peut induire des changements dans la suite du processus de data mining et sur la forme de livraison des résultats. Par exemple :
-	La compagnie souhaite que la population cible soit divisée en deux groupes ; elle dispose de suffisamment de moyens pour offrir gratuitement le produit à tous les preneurs potentiels
-	La compagnie ne peut offrir qu’un nombre limité de produits et souhaite donc que la population cible soit ordonnée en fonction de la probabilité qu’à chaque individu de prendre le produit à la fin de la campagne promotionnelle.
Le premier scénario pose un problème de discrimination et le second, un problème de scoring.

## INVENTAIRE DES DONNEES EXPLOITABLES

Les données sont la matière première de la data science. Ce sont elles qui détiennent les informations que l’on cherche. Ainsi, pour avoir la bonne information, il faut avoir les bonnes données. L’inventaire des données consiste en un recensement des données utiles, exploitables et suffisamment à jour sur les caractéristiques et le comportement des individus étudiés. Cela nécessite une bonne compréhension des objectifs pratiques et de la problématique.
Dans la pratique, certaines entreprises disposent de dictionnaires de données pouvant rendre l’inventaire des données exploitables moins difficile.
Il peut également arriver que certaines données utiles soient disponibles en dehors du système d’informations de l’entreprise (par exemple dans le cas de l’achat de données à une entreprise extérieure ou encore de données collectées par un autre service/filiale). Dans ces cas, une étape préliminaire sera la collecte, l’intégration et la consolidation des données. Cette tache pourra être plus ou moins laborieuse en fonction de la compatibilité des différentes sources de ces données.
Il peut également arriver que certaines données nécessaires ne soient pas disponibles ; soit parce que l’entreprise ne les collecte pas, soit à cause du problématique qui est étudiée. Cette dernière situation arrive souvent lorsqu’on souhaite mettre un nouveau produit sur le marché ; dans ce cas, il convient de procéder à une enquête auprès d’un échantillon de client.


## EXPLORATION ET PREPARATION DES DONNEES

Cette étape contribue significativement à la réussite d’un projet data science. Elle permet d’avoir une connaissance plus avancée des données et de les nettoyer. Elle pourra aussi permettre de se séparer de certaines variables ; soit parce qu’elles contiennent une proportion très élevée de valeurs manquantes/ aberrantes, soit parce qu’elles sont fortement corrélées avec d’autres. Les opérations suivantes sont importantes (voire nécessaires) au cours de cette étape.

### Statistiques descriptives

Cette analyse préliminaire, permet d’avoir un aperçu général et rapide des données et d’en juger la fiabilité. Cette opération consiste à :
-	Prendre connaissance avec les types de variables (quantitative continue, quantitative discrète, qualitative) et leurs différents codages ;
-	Examiner la distribution des variables ;
-	Examiner les ordres de grandeurs des variables ;
-	Détecter les valeurs manquantes/aberrantes ;
-	Transformer et coder certaines variables ;

### Traitements des valeurs manquantes

Les valeurs manquantes surviennent notamment dans les questionnaires, où les variables déclaratives facultatives sont souvent non renseignées ou encore lors d’enquêtes, où des questions jugées indiscrètes sont laissées sans réponses.
On parle de **valeur manquante totale**, lorsqu’aucune valeur n’a été observée sur un individu (cette situation n’est pas très fréquente en entreprise) et de **valeur manquante partielle** lorsque certaines variables (pas toutes) n’ont pas été observées. La présence de valeurs manquantes est un handicap, car la plupart des méthodes de data mining ne les intègrent pas. L’idée naïve serait de retirer de la base tous les individus ayant au moins une valeur manquante. En règle générale, cette méthode n’est pas conseillée. En effet, avec un taux de 1% de valeurs manquantes, plus de la moitié de l’échantillon peut être perdu. Deux procédures sont généralement conseillées :
-	Ne pas utiliser la variable concernée si sa contribution à l’analyse du problème ne paraît pas essentielle ou la remplacer par une variable proche sans valeurs manquantes.
-	Remplacer la valeur manquante par une valeur déterminée soit statistiquement, soit par la connaissance que l’on a des données, soit par une source externe.
Cette deuxième procédure s’appelle l’imputation. Elle déconseillée lorsque le taux valeurs manquantes est supérieur à 10%. Les méthodes d’imputation les plus simples (et les plus connues) sont : l’imputation par la moyenne (variable numérique), l’imputation par le mode (variable qualitative) et l’imputation multiple. Cette dernière consiste à remplacer la valeur manquante par plusieurs valeurs plausibles et de combiner les résultats obtenus à partir des différentes bases complétées.

### Traitements des valeurs aberrantes

Une valeur aberrante est une valeur erronée correspondant à une mauvaise mesure, une erreur de calcul, une erreur de saisie ou une fausse déclaration. Par exemple, la variable « genre » qui a plus de deux modalités, un individu qui déclare n’avoir pas fait les études supérieures et à le Master comme dernier diplôme. 
La détection des valeurs aberrantes est une tâche délicate et nécessite une bonne connaissance des données. La statistique descriptive permet généralement d’en détecter un bon nombre. Pour leurs traitements, si la valeur aberrante est compatible avec la variable, on peut la conserver tout en acceptant une marge d’erreur sur les résultats. On peut aussi considérer cette valeur comme manquante et recourir aux procédures utilisées pour le traitement des valeurs manquantes.

### Réduction du nombre de variable.

La base de données utilisée pour un projet de data science peut être de très grande dimension (par exemple plus de 1000 variables). Certaines de ces variables peuvent être corrélées entre elles ou non pertinentes par rapport aux objectifs à atteindre (bien qu’ayant été collectées au départ). Il convient donc :
-	D’ignorer certaines variables significativement corrélées entre elles dont l’utilisation simultanée rendrait la procédure plus complexe tout en diminuant son efficacité ;
-	D’ignorer certaines variables non pertinentes par rapport aux objectifs à atteindre,
-	D’agréger certaines variables en une seule ;
-	De transformer au moyen de l’analyse factorielle (telle que l’analyse en composante principale) certaines variables initiales en des variables moins nombreuses. 
Pour les problématiques de machine learning, les données consolidées et prêtes à l’emploi doivent être aléatoirement divisées en deux parties : échantillon d’apprentissage et échantillon test (2/3 et 1/3 par exemple). Le premier servira à la construction des modèles et la seconde à l’évaluation. Pour certains problèmes, et suivant les méthodes utilisées, on divisera la base initiale en trois parties : échantillon d’apprentissage, validation et test. L’échantillon de validation servant à l’évaluation de la procédure tout en ajustant certains paramètres du modèle.

## ELABORATION ET EVALUATION DES MODELES

A cette étape, les données sont prêtes à êtres utilisées et les méthodes admissibles à la résolution du problème sont connues. On peut alors lancer la construction des modèles en utilisant les données de modélisation. Cette construction se fait avec l’aide de logiciels de programmations (R, Python, etc.). Cette étape qui constitue le cœur de l’activité de la data science, est symboliquement la plus importante (car c’est ici que les produits sortent des modules) et n’est pas forcément la plus longue. Dans la pratique, il arrive que l’on se heurte à des problèmes numériques (tel que la convergence des algorithmes, …), qui peuvent généralement se résoudre en jouant sur les paramètres des algorithmes, ou en considérant des variantes des algorithmes.
Pour des problématiques de prédictions (faisant appel à des techniques d’apprentissage supervisé), on obtient les règles d’appartenance aux différentes classes, suivie d’une évaluation de la procédure sur l’échantillon test. Cette évaluation permet de mesurer la performance (l’erreur de classement) de la méthode de data science utilisée. Elle permet à toute l’équipe du projet (décideurs et experts) de juger de la pertinence du produit en fonction des objectifs fixés et éventuellement donner un accord pour le déploiement des modèles.


## DEPLOIEMENT DES MODELES

Le déploiement consiste à déplacer les modèles de l’environnement data science, vers les services de l’entreprise qui les utiliseront. Même si les meilleurs modèles reconnus ont été utilisés, le projet sera un échec commercial si le déploiement n’est pas réussi. Il peut arriver (bien que rarement), que les algorithmes programmés dans un environnement particulier, ne soient pas exécutables sur la plate-forme informatique de l’entreprise. Dans ce cas, une traduction dans un langage adapté sera donc nécessaire. Un autre problème rencontré fréquemment est que les modèles utilisent des variables d’entrées qui ne sont pas des données originales. Ce problème se résout facilement puisque les entrées des modèles sont issues des données initiales. Dans tous les cas, le déploiement consiste (en partie) à l’intégration des programmes des modèles dans le système de l’entreprise et de les mettre à disposition des utilisateurs. La question de la mise à jour périodique des modèles est également posée à cette étape.


