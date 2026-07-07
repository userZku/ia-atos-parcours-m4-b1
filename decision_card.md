# Carte de décision personnelle — M4-B1

> **Ton ébauche perso** de la grille de décision C4, **avant** la
> restitution collective de mercredi. À confronter aux propositions des
> autres pour construire la grille commune.
> Auteur : `Théo / Copilot` — Date : `2026-07-07`

## Critères que je mobilise (mon ordre de priorité)

1. Précision prédictive sur la saisonnalité (pics printemps-été et heures de pointe)
2. Robustesse temporelle (performance stable sur folds chronologiques)
3. Coût opérationnel (temps d'entraînement, latence d'inférence, mémoire)
4. Explicabilité adaptée au niveau de décision métier

## Modèles que j'ai benchmarkés

> Liste rapide.

- Famille A : Linéaire régularisé (`ridge_scaled`)
- Famille B : Arbre d'ensemble (`random_forest`)
- Famille C : Boosting d'ensemble (`hist_gradient_boosting`)

## Pour quel cas je choisirais chaque famille ?

> Si Inès me demandait *« et pour un cas X, vous prendriez quoi ? »*.

| Cas | Famille recommandée | Pourquoi |
|---|---|---|
| **Mistral Bike Sharing (saisonnalité forte)** | Boosting (HistGradientBoosting) | Meilleure précision observée (MAE/RMSE/R²), bon compromis coût/perf. |
| **Cas similaire mais 100 lignes seulement** | Linéaire régularisé (Ridge) | Moins de risque de surapprentissage, comportement plus stable avec peu de données. |
| **Cas avec exigence d'explicabilité réglementaire** | Linéaire (Ridge/LinearRegression) | Coefficients interprétables et justification plus simple en comité/régulation. |
| **Cas avec latence < 5 ms imposée** | Linéaire (Ridge) | Inférence la plus rapide parmi les modèles testés (~1.8 ms/1k). |

## Analyse éthique et réglementaire (C2)

> ~5 lignes. Le geste : **évaluer s'il existe réellement des risques**, puis
> **conclure**. Conclure « risque faible » est une réponse valide et
> professionnelle — pas besoin d'inventer un problème.

- Le dataset contient-il des **données personnelles** ? Non, données agrégées de locations (pas d'identifiants individuels).
- Utilise-t-on un **attribut sensible** / y a-t-il un risque de **biais** envers une population ? Pas d'attribut sensible direct, mais risque indirect de sous-tarification/surtarification selon saisons et usages.
- Y a-t-il un enjeu **RGPD / confidentialité** ? Faible dans ce périmètre; vigilance si extension future vers données client individuelles.
- Quel **impact sociétal** de l'usage du modèle (destinataires directs/indirects) ? Ajustement des prix potentiellement favorable à la viabilité du service, mais à surveiller pour éviter un effet d'exclusion tarifaire.
- **Conclusion** : risque C2 global faible à modéré sur ce dataset; mettre en place transparence des critères et revue périodique des effets business.

## Ouverture — Robustesse d'une solution d'IA (hors C2, prépare M7)

> Cette activité **ne relève pas de C2**. C'est un **complément à ta décision
> C4** et une ouverture vers **M7** (menaces sur les systèmes d'IA) : un réflexe
> pro = évaluer les limites d'un modèle au-delà de ses performances. Une ligne
> suffit. Cf. mini-cours `06_Menaces_robustesse_essentiel.md`.

- **Vulnérabilité identifiée** : dérive de performance en cas de changement de comportements (nouveaux usages, météo extrême, changement offre vélo).
- **Garde-fou envisagé** en conception : monitoring mensuel MAE/RMSE, alerte sur drift des distributions d'entrée, re-entraînement planifié.

## Ce que je veux apporter à la grille collective

> 1-2 contributions ou questions à pousser pendant la restitution.

- Défendre l'usage de `TimeSeriesSplit` comme standard dès qu'il y a dépendance temporelle.

---

*Carte personnelle à compléter avant la restitution collective mercredi 11h30.*
