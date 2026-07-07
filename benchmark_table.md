# Tableau comparatif — Benchmark Mistral Assurances

> Document à remettre à **Inès Tabet** (responsable actuariat). Doit être
> lisible par un actuaire, pas par un data scientist.
> Auteur : `Théo / Copilot` — Date : `2026-07-07`

## Méthodologie

- **Split** : `TimeSeriesSplit(n_splits=5)`
- **Métriques** : MAE, RMSE, R² (moyennes sur N folds)
- **Mêmes folds, mêmes features** pour tous les modèles (règle d'or *comparabilité*)
- **Features** : `year, month, hour, is_holiday, weekday, is_working_day, weather, temperature_norm, temperature_feels_norm, humidity_norm, windspeed_norm`
- **Hyperparamètres** : défaut + variante raisonnable (Ridge + scaler, RandomForest 200 arbres, HistGradientBoosting 300 itérations)
- **Référence** : baseline `mistral-tarif-v1` (LinearRegression 2024)

## Tableau (à compléter)

| Modèle | MAE | RMSE | R² | Temps train (s) | Latence inférence (ms/1k) | Explicabilité (1-3) | Mémoire (Mo) |
|---|---|---|---|---|---|---|---|
| **mistral-tarif-v1** (baseline) | 105.0 | 139.4 | 0.39 | ~0.1 | ~1 | 3 (très explicable) | ~0.5 |
| **ridge_scaled** | 117.9415 | 156.8898 | 0.2278 | 0.0074 | 1.8036 | 3 (élevée) | <0.01 |
| **random_forest** | 52.9625 | 79.4872 | 0.7796 | 6.8424 | 41.1497 | 2 (moyenne) | 286.49 |
| **hist_gradient_boosting** | 51.4526 | 75.5833 | 0.7996 | 1.2818 | 17.2901 | 1 (faible à moyenne) | 0.95 |

## Interprétation pour Inès

> 1-2 paragraphes en langage actuaire (pas data scientist).

Les modèles d'ensemble améliorent fortement la précision par rapport à la baseline historique. HistGradientBoosting réduit l'erreur de manière significative (RMSE 75.58 vs 139.4 pour la baseline) tout en conservant un temps d'entraînement contenu (~1.28 s) et une taille mémoire faible (0.95 Mo). RandomForest reste performant mais coûteux en exploitation (286.49 Mo, inférence plus lente).

Ridge est très rapide et très explicable, mais sa performance est insuffisante sur cette série temporelle (R² 0.2278), ce qui confirme que les relations sont non linéaires (saisonnalité, effets d'heure et météo). Pour un usage tarification, HistGradientBoosting offre le meilleur compromis précision/coût opérationnel.

## Recommandation

Voir `verdict.md` (5 lignes max).

**Modèle recommandé au comité** : `hist_gradient_boosting`.

**Pourquoi** : meilleure précision globale (MAE/RMSE/R²), coût d'entraînement acceptable, latence inférence modérée, empreinte mémoire faible.

**Point de vigilance** : explicabilité plus faible que la régression linéaire; prévoir une couche d'explication locale (importance des variables / SHAP) pour les décisions sensibles.

---

*Document remis à Inès Tabet — `2026-07-07`.*
