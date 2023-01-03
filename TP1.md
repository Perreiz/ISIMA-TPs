# Code bar analyser

## 1 - Objectifs du TP

L'objectif de ce TP est de réaliser une application java classique permettant d'obtenir des informations nutritionnelles de différents produits en fonction de leur code barre.

Cette application permettra aux utilisateurs de connaitre les performances nutritionnelles d'un produit. Pour cela, elle utilisera des données d'Open Food Facts, une API publique fournissant les informations de bases. A partir de ces données l'application effectuera des calculs permettant de faire l'analyse nutritionnelle du produit.

Vous devrez donc faire une application java dans laquelle un utilisateur pourra donner un code barre dans le terminal, L'application demandera à l'api d'Open Food Facts des informations sur votre produit, puis effectuera quelques calculs.

Pour ce TP vous n'aurez pas besoin de stocker des informations en bases de données, les calculs se feront via les informations donnée en Annexe 1.

Il faudra utiliser Google Guice en tant que framework d'injection de dépendances afin d'affecter les implémentations de vos services, et ainsi éviter un couplage trop fort.

## 2 - User stories attendues

Essayez de faire un tag GIT pour identifier l'implémentation des User stories. Dans tous les cas un commit concernant la US1 doit avoir la reference de cette user story (ex git commit -m 'US1 : initial commit')

### US 1 : Récuperer les informations nutritionnelles d'un produit

> Je souhaite pouvoir disposer des informations d'Open Food Fact sur un produit afin de pouvoir faire des choix de consomation éclairés.

Votre application devra donc utiliser l'API d'Open Food Fact afin de récupérer les informations d'un produit donné (via son code barre).

L'API est disponible à l'adresse suivante :  
https://fr.openfoodfacts.org/data  
Exemple pour le code barre 7622210449283 : https://fr.openfoodfacts.org/api/v0/produit/7622210449283.json  
La documentation de l'API est diponible ici : https://en.wiki.openfoodfacts.org/API

Vous avez le choix du client HTTP (OkHttp, Retrofit, Apache HttpClient, client natif Java 11) ainsi que n'importe quelle librairie JSON (Json-B, Jackson, Genson, Gson). Dans tout les cas pensez aux tests.

Vous n'utiliserez pas forcèment toutes les informations exposées par l'API Open Food Fact mais uniquement ce qui vous semble pertinent. (Adaptez vos objets en conséquence)

Pour info Open Food Facts et un service gratuit, pour l'instant. (donc pas de compte ni d'abonnement à faire)

### US 2 : Score nutritionel

> Je souhaite avoir un score nutritionnel afin de pouvoir faire des choix de consomation éclairés.

Dans un deuxieme temps, l'application calculera le nutriscore du produit donné et l'affichera en log de l'application.

##### Calcul du Score nutritionnel

Basé sur les infos de l'API d'Open Food Fact pour un produit donné, calculez la composante N (Négative) du score nutrionnel.  
Calculez la composante P (Positive) du score nutrionnel.  
Basé sur les composantes N et P, votre application calculera un score nutritionnel en accord avec les spécifications données dans l'annexe 1.

### US 3 : Classement de l'aliment (Optionnel)

> Je souhaite pouvoir disposer d'indicateurs simples me permettant de facilement évaluer le produit afin de pouvoir faire des choix de consomation éclairés.

Faites evoluer votre application pour intégrer la classe de l'aliment et le code couleur associé. Vous trouverez les détails dans l'annexe 2.

On rajoutera donc au log de l'application la "couleur" et la "classe" du produit.

## 3 - Contraintes techniques

Votre application doit tourner dans un terminal ou dans votre IDE, elle devra demander à l'utilisateur de renseigner un code barre, et affichera un log avec les informations demandés dans les différentes user stories.

Vous n'avez pas à sauvegarder quoi que ce soit en base de donnée.

L'utilisation de Google Guice permettra d'injecter les services, donc il n'y aura pas d'instanciation manuelle de service dans les constructeurs.
Google Guice pourra aussi permettre d'avoir une implémentation d'un service de Mock à la place de votre service dédié aux appels à Open Food Facts (Pour les TUs notammenet).

Enfin, aucune contrainte n'est imposé sur les choix des librairies à utilisé dans vore application (à part Google Guice), ce qui veut dire que vous pouvez utiliser n'importe quelle librairie qui vous facilitera la vie.

L'utilisation de Maven est obligatoire. La version de Java est à votre discretion. La LTS la plus récente est la 21, mais la 11 reste encore la plus répandue.

## 6 - Evaluation

L'application à réaliser étant assez simple, la complétude sera évalué. Mais réaliser entièrement l'application ne sera pas suffisant puisque d'autres points seront évalués :

-   **Le niveau de test** : Une application non testé ne peut pas être déployée en production sans prendre de risques importants. Et une application qui n'est pas en production est une application qui ne sert à rien. Donc tests unitaires et bonne couverture de test **obligatoire**.
-   L'organisation du code : Nous n'en sommes qu'au début, mais pensez déjà à la modularité de votre code. Identifier les acteurs et les responsabilités afin d'anticiper les raisons qui pousserait votre code à changer (raisons techniques, fonctionnelles, légales...). Faite cela pour chaque chaque module, classe et fonction. Répéter ce processus de manière itérative, en remodifiant votre code pour obtenir une meilleure modularité. Imaginez que votre application soit faite pour être réutiliser en partie dans une autre, chaque composant doit être atomique.
-   Le niveau de couplage : Un trop haut niveau de couplage entre les composants d'une application empechera celle-ci d'evoluer facilement. Trouvez des solutions pour avoir un niveau de couplage faible. Penez à tout ce que vous offre la POO, les designs patterns, le langage (interfaces...) même si l'application est très simple pour le moment.
-   L'utilisation de Google Guice : Vos services doivent être correctement injectés et l'implementaiton d'un service dédié aux mocks serai un bon exemple.

Pour ce TP, la clarté du code sera également évalué. Indentation, nommage des variables, classes, méthodes... et commentaires sont vos amis.

## Annexe 1

Le score nutritionnel des aliments repose sur le calcul d’un score unique et global prenant en compte, pour chaque aliment :

-   une composante dite « négative » N,

-   une composante dite « positive » P

### Composante Negative N

La composante N du score prend en compte les éléments nutritionnels dont il est recommandé
de limiter la consommation : densité énergétique (apport calorique en kJ pour 100 g d’aliment),
teneurs en acides gras saturés (AGS), en sucres simples (en g pour 100g d’aliment) et en sel (en
mg pour 100g d’aliment). Sa valeur correspond à la somme des points attribués, de 1 à 10, en
fonction de la teneur de la composition nutritionnelle de l’aliment (cf. tableau 1). La note pour la
composante N peut aller de 0 à 40.

Tableau 1 : Points attribués à chacun des éléments de la composante dite « négative » N

| Points | Densité énergétique (kJ/100g) (energy_100g) | Graisses saturées (g/100g) (saturated-fat_100g) | Sucres simples (g/100g) (sugars_100g) | Sodium1 (mg/100g) (salt_100g) |
| ------ | ------------------------------------------- | ----------------------------------------------- | ------------------------------------- | ----------------------------- |
| 0      | <= 335                                      | <= 1                                            | <= 4,5                                | <= 90                         |
| 1      | > 335                                       | > 1                                             | > 4,5                                 | > 90                          |
| 2      | > 670                                       | > 2                                             | > 9                                   | > 180                         |
| 3      | > 1005                                      | > 3                                             | > 13,5                                | > 270                         |
| 4      | > 1340                                      | > 4                                             | > 18                                  | > 360                         |
| 5      | > 1675                                      | > 5                                             | > 22,5                                | > 450                         |
| 6      | > 2010                                      | > 6                                             | > 27                                  | > 540                         |
| 7      | > 2345                                      | > 7                                             | > 31                                  | > 630                         |
| 8      | > 2680                                      | > 8                                             | > 36                                  | > 720                         |
| 9      | > 3015                                      | > 9                                             | > 40                                  | > 810                         |
| 10     | > 3350                                      | > 10                                            | > 45                                  | > 900                         |

### Composante Positive P

La composante P est calculée, en fonction de la teneur de l’aliment en fibres et en protéines (exprimées en g pour 100 g d’aliment). Pour chacun de ces éléments, des points, allant de 1 à 5 sont attribués en fonction de leur teneur dans l’aliment (cf. tableau 2). La composante positive P du score nutritionnel est la note correspondant à la somme des points définis pour ces deux éléments : cette note est donc comprise entre 0 et 10.

Tableau 2 : Points attribués à chacun des nutriments de la composante dite « positive » P

| Points | Fibres (g/100g) (fiber_100g) | Protéines (g/100g) (proteins_100g) |
| ------ | ---------------------------- | ---------------------------------- |
| 0      | <= 0,9                       | <= 1,6                             |
| 1      | > 0,9                        | > 1,6                              |
| 2      | > 1,9                        | > 3,2                              |
| 3      | > 2,8                        | > 4,8                              |
| 4      | > 3,7                        | > 6,4                              |
| 5      | > 4,7                        | > 8,0                              |

Calcul du score nutritionnel  
Le calcul final du score nutritionnel se fait en soustrayant à la note de la composante négative N la note de la composante positive P avec quelques conditionnalités décrites ci après.

### Calcul du score nutritionnel

**Score nutritionnel = Total Points N – Total Points P**

La note finale du score nutritionnel attribuée à un aliment est donc susceptible d’être comprise entre une valeur théorique de - 10 (le plus favorable sur le plan nutritionnel) et une valeur théorique de + 40 (le plus défavorable sur le plan nutritionnel).

## Annexe 2

Classement de l’aliment dans l’échelle nutritionnelle à cinq niveaux sur la base du score calculé

| Classe    | Bornes du score | Couleur     |
| --------- | --------------- | ----------- |
| Trop Bon  | Min à -1        | green       |
| Bon       | 0 à 2           | light green |
| Mangeable | 3 à 10          | yellow      |
| Mouai     | 11 à 18         | orange      |
| Degueu    | 19 à Max        | red         |
