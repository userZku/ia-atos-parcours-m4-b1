# Verdict — Recommandation Mistral Assurances

> **5 lignes maximum.** Document remis à Inès Tabet.
> Auteur : `Théo / Copilot` — Date : `2026-07-07`

**Recommandation** : retenir `hist_gradient_boosting` pour la tarification Bike Sharing (version Mistral v2).

**Raison principale (chiffrée)** : meilleur score global observé en CV temporelle (`MAE 51.45`, `RMSE 75.58`, `R² 0.7996`), très au-dessus de la baseline (`R² 0.39`) et de `ridge_scaled` (`R² 0.2278`).

**Condition de changement d'avis** : si l'exigence prioritaire devient explicabilité réglementaire forte ou latence ultra-basse, je proposerais un modèle linéaire interprétable (avec perte de précision assumée) ou un modèle intermédiaire explicable.

---

*Verdict produit par Théo / Copilot, 2026-07-07, dans le cadre du brief M4-B1 ATOS.*
