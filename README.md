# Boutique Diayma

# Boutique Diayma – TD .NET 03

## Environnement
- macOS 11.7 (Intel), VS Code, SDK .NET 6.0.136
- Le projet cible `netcoreapp2.0` à l'origine ; migré vers `net6.0` sur la branche `mac-net6`
  pour pouvoir l'exécuter sur Mac.

## Question 2 – Projets de la solution
Un seul projet : `Diayma` (`P2FixAnAppDotNetCode/Diayma.csproj`).

## Question 3 – Version du SDK
`netcoreapp2.0` (.NET Core 2.0, ASP.NET Core 2.0).

## Question 6 – Bugs trouvés
1. **Total et moyenne du panier faux** (`Cart.GetTotalValue` et `GetAverageValue`) :
   ils utilisent `Product.Price` sans multiplier par `Quantity`.
   Exemple : 3 clés USB (1 000 €) + 12 Galaxy A25 (130 000 €) → Total 131 000 € au lieu de 1 563 000 €.
2. **Pas de contrôle du stock** : on peut ajouter 12 Galaxy A25 alors que le stock est de 10.

## Question 8 – Parcours de débogage
| Ordre | Namespace | Classe | Méthode | Mode | Justification |
|---|---|---|---|---|---|
| 1 | P2FixAnAppDotNetCode | Startup | constructeur (ligne 20) | (arrêt initial) | Premier point d'arrêt atteint au démarrage de l'application |
| 2 | P2FixAnAppDotNetCode | Startup | ConfigureServices (ligne 32) | Step Over | Chaque ligne enregistre un service : il n'y a rien à explorer à l'intérieur |
| 3 | P2FixAnAppDotNetCode.Models.Services | ProductService | constructeur (ligne 15) | Continue | Construit par l'injection de dépendances avant le contrôleur ; on saute jusqu'au prochain point d'arrêt |
| 4 | P2FixAnAppDotNetCode.Controllers | ProductController | constructeur (lignes 15 à 17) | Step Over | Simples affectations de champs, sans appel à explorer |
| 5 | P2FixAnAppDotNetCode.Controllers | ProductController | Index (ligne 20) | Continue | Point d'arrêt sur l'action qui traite la requête de la page d'accueil |
| 6 | P2FixAnAppDotNetCode.Models.Services | ProductService | GetAllProducts (ligne 25) | Step Into | On entre dans le code de la boutique appelé par Index |
| 7 | [P2FixAnAppDotNetCode.Models.Repositories] | ProductRepository | GetAllProducts (lignes 42 à 44) | Step Into, puis Step Over | Step Into pour entrer dans le dépôt, Step Over pour passer la ligne LINQ (Where, OrderBy) sans entrer dans le framework |
| 8 | [P2FixAnAppDotNetCode.Components] | CartSummaryViewComponent | constructeur (ligne 12) | Continue | Point d'arrêt atteint pendant le rendu de la vue |
| 9 | [namespace généré par ASP.NET] | vue _Layout.cshtml | CartSummary (ligne 22), RenderBody (ligne 27) | Step Over | On passe l'appel du composant de vue, déjà vu, pour arriver au rendu du corps de la page |

Rappel des modes : Step Over (principal), Step Into (détaillé), Step Out (sortant).
Remarque : le débogueur indique « Stepping over non-user code ... LanguageService..ctor » : ce constructeur n'est pas écrit dans le projet, donc il est ignoré par « Just My Code ».

## Question 10 – Lien vers le zip
https://drive.google.com/file/d/1e7bY6dRhAAz9Ts9r1A66bRSOj8-SJbKy/view?usp=drive_link

