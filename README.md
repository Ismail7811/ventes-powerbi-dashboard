\# Dashboard Power BI : analyse des ventes (AdventureWorks)



Dashboard Power BI d'analyse des ventes d'une entreprise de vélos, d'accessoires et de vêtements : chiffre d'affaires, profit, commandes, retours, produits les plus vendus et répartition géographique.



> \*\*Contexte\*\* : projet d'apprentissage réalisé à partir d'un cours Power BI disponible sur YouTube. Données : jeu d'exemple AdventureWorks (Microsoft).



\## Aperçu du dashboard



!\[Dashboard](screenshots/dashboard.png)



Le rapport contient :

\- un filtre de dates (de janvier 2015 à juin 2017) et des filtres par pays ;

\- les KPI \*\*Total Revenue\*\*, \*\*Total Order\*\* et \*\*Total Profit\*\* ;

\- des indicateurs du mois précédent avec leur objectif (chiffre d'affaires, commandes, retours) ;

\- le meilleur produit par nombre de commandes et par chiffre d'affaires ;

\- une jauge de chiffre d'affaires par rapport à l'objectif ;

\- une carte géographique des ventes ;

\- un tableau des produits avec le nombre de commandes et le taux de retour ;

\- les commandes par catégorie et par sous-catégorie.



\## Technologies



\- \*\*Power BI Desktop\*\* : modélisation et visualisation

\- \*\*Power Query\*\* : import et préparation des données

\- \*\*DAX\*\* : mesures et table calculée



\## Données



| Fichier | Contenu |

|---|---|

| `Sales.csv` | Lignes de commandes (environ 56 000) : dates, produit, client, zone, quantité |

| `Customers.csv` | Clients (environ 18 000) |

| `Products.csv` | Produits, coût et prix |

| `Product\_Categories.csv` | Catégories de produits |

| `Product\_Subcategories.csv` | Sous-catégories de produits |

| `Returns.csv` | Retours de produits (environ 1 800) |

| `Locales.csv` | Zones de vente et pays |

| `Calendar.csv` | Table de dates (du 01/01/2015 au 30/06/2017) |



Les données sont des données d'exemple fictives.



\## Modèle de données



!\[Modèle de données](docs/Data\_Model.png)



Le modèle est un \*\*modèle en flocon\*\* (snowflake) :

\- \*\*Tables de faits\*\* : `Sales` et `Returns`

\- \*\*Dimensions\*\* : `Customers`, `Products`, `Locales`, `Calendar`

\- \*\*Hiérarchie produits\*\* : `Products` → `Product\_Subcategories` → `Product\_Categories`

\- Les relations sont de type un-à-plusieurs.



\## Mesures DAX



```dax

Total Order = DISTINCTCOUNT(Sales\[OrderNumber])



Total Revenue = SUMX(Sales, Sales\[OrderQuantity] \* RELATED(Products\[ProductPrice]))



Total Cost = SUMX(Sales, Sales\[OrderQuantity] \* RELATED(Products\[ProductCost]))



Total Profit = \[Total Revenue] - \[Total Cost]

```



\### Table calculée



```dax

VIP CUSTOMER = FILTER(Customers, AND(Customers\[AnnualIncome] > 130000, Customers\[HomeOwner] = "Y"))

```



Cette table isole les clients à haut revenu (plus de 130 000 $ par an) qui sont propriétaires de leur logement, pour analyser le segment premium. Elle n'est reliée à aucune autre table.



\## Structure du dépôt



```

ventes-powerbi-dashboard/

├── data/          # fichiers CSV

├── docs/          # schéma du modèle de données

├── powerbi/       # fichier Power BI (.pbix)

├── screenshots/   # capture du dashboard

└── README.md

```



\## Ouvrir le projet



1\. Installer \[Power BI Desktop](https://powerbi.microsoft.com/desktop/) (Windows).

2\. Télécharger le dépôt (\*\*Code\*\* > \*\*Download ZIP\*\*) et le décompresser.

3\. Ouvrir `powerbi/MyProject.pbix`.

4\. Pour actualiser les données : \*\*Fichier\*\* > \*\*Options et paramètres\*\* > \*\*Paramètres de la source de données\*\*, puis modifier le chemin de chaque source vers le dossier `data/`.



\## Pistes d'amélioration



\- Ajouter une page dédiée aux retours et une page dédiée aux clients (dont le segment VIP).

\- Ajouter des mesures : panier moyen, évolution annuelle (YoY).

\- Enregistrer le projet au format PBIP pour suivre les changements avec Git.



\## Auteur



RAGUIG Ismail : \[GitHub](https://github.com/Ismail7811)

