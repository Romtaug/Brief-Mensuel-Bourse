# Snapshots Directory

Ce dossier contient les **snapshots mensuels** du classement des actions.

Chaque mois, un fichier `ranking_YYYY-MM.json` y est ajouté automatiquement par le workflow GitHub Actions.

⚙️ **Rotation automatique** : les 5 snapshots les plus récents sont conservés. Les plus anciens sont supprimés à chaque run.

📂 Format des fichiers :
- `ranking_2026-05.json` — snapshot du 1er mai 2026 (run de juin)
- `ranking_2026-06.json` — snapshot du 1er juin 2026 (run de juillet)
- ...

Les snapshots permettent de calculer la **différence vs mois précédent** (entrées/sorties du Top 5).
