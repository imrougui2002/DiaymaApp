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
| 1 | P2FixAnAppDotNetCode | Startup | constructeur | (arrêt initial) | Premier point d'arrêt au démarrage |
| 2 | P2FixAnAppDotNetCode | Startup | ConfigureServices | [mode] | [pourquoi] |
| 3 | P2FixAnAppDotNetCode.Models.Services | ProductService | constructeur | [mode] | Injection de dépendances |
| 4 | [namespace] | ProductController | constructeur | [mode] | [pourquoi] |
| 5 | P2FixAnAppDotNetCode.Models.Services | ProductService | GetAllProducts | [mode] | [pourquoi] |
| 6 | [namespace] | ProductRepository | GetAllProducts | [mode] | [pourquoi] |
| 7 | [namespace] | CartSummaryViewComponent | constructeur | [mode] | [pourquoi] |
| 8 | [namespace généré] | vue _Layout.cshtml | CartSummary / RenderBody | [mode] | [pourquoi] |

Rappel des modes : Step Over (principal), Step Into (détaillé), Step Out (sortant).

## Question 10 – Lien vers le zip
https://drive.google.com/file/d/1e7bY6dRhAAz9Ts9r1A66bRSOj8-SJbKy/view?usp=drive_link

