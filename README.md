# 📊 Brief Mensuel Bourse

Pipeline automatisé Python qui génère chaque mois un **brief boursier LinkedIn** : analyse de ~1075 actions (S&P 500 + CAC 40 + DAX + MDAX + SBF 120 + FTSE 100 + STOXX 600), classement par performance et potentiel, vidéo MP4 portrait 1080×1350 et post LinkedIn ~4000 chars.

Le tout publié automatiquement le **premier jour ouvré de chaque mois** via GitHub Actions + Make.com + LinkedIn.

---

## 🎯 Ce que ça fait

Chaque premier jour ouvré du mois à ~8h Paris :

1. ✅ Fetch yfinance (~1075 actions, ~5-10 min)
2. ✅ Fetch benchmarks indices (S&P 500, CAC 40, STOXX 600)
3. ✅ Calcule Top 5 perf / Top 5 potentiel / Best par secteur GICS
4. ✅ Génère :
   - 📊 Excel 6 onglets (xlsx)
   - 📝 Post LinkedIn ~4000 chars
   - 🎬 Vidéo MP4 portrait 1080×1350 avec musique
5. ✅ Upload vidéo sur litterbox.catbox.moe (URL publique 24h)
6. ✅ Envoie webhook Make.com → publie automatiquement sur LinkedIn
7. ✅ Commit snapshot du mois dans `snapshots/` (5 derniers max)

---

## 🚀 Setup initial (une seule fois)

### 1. Cloner ce repo sur GitHub

```bash
# Renomme le repo en BriefMensuelBourse (ou autre nom de ton choix)
```

### 2. Configurer les secrets GitHub Actions

Va sur ton repo → **Settings → Secrets and variables → Actions → New repository secret** :

| Secret | Description | Exemple |
|---|---|---|
| `WEBHOOK_URL` | URL du webhook Make.com | `https://hook.eu2.make.com/xxxxx` |
| `CODE_PARRAINAGE` (optionnel) | Ton code parrain Boursorama | `ROTA0058` |
| `PARRAINAGE_URL` (optionnel) | URL de parrainage | `https://bour.so/p/GB93ZfQVNVr` |

### 3. (Optionnel) Ajouter ta musique perso

Le script télécharge automatiquement une musique royalty-free (Internet Archive · CC BY-SA 4.0) à chaque run si `assets/music.mp3` n'existe pas.

Si tu veux **ta propre musique** :
1. Trouve un MP3 royalty-free sur https://pixabay.com/music/ (filtre "Corporate" ou "Tech")
2. Renomme-le en `music.mp3`
3. Place-le dans `assets/music.mp3`
4. Commit + push

Le script utilisera automatiquement ton fichier au lieu d'auto-download.

---

## 🧪 Tester sans publier sur LinkedIn

Va dans **Actions → Brief Mensuel Bourse → Run workflow** :

| Champ | Valeur recommandée |
|---|---|
| TEST_MODE | `true` (~28 actions, 1 min) |
| SEND_TO_WEBHOOK | `false` (pas d'envoi LinkedIn) |
| FORCE_RUN | `true` (bypass check premier jour ouvré) |

→ Tu obtiens la vidéo + post **en artifact téléchargeable** sans publier nulle part.

---

## 🚀 Premier run en prod (= sur LinkedIn)

| Champ | Valeur |
|---|---|
| TEST_MODE | `false` (~1075 actions, 6-10 min) |
| SEND_TO_WEBHOOK | `true` |
| FORCE_RUN | `true` (si tu veux tester aujourd'hui, sinon attends le 1er ouvré) |

⚠️ Vérifie que ton scénario Make.com est bien actif avant.

---

## 📅 Planification automatique

Le workflow `.github/workflows/brief.yml` se déclenche **automatiquement tous les jours du 1er au 4 du mois à 7h UTC** (= 8h-9h Paris selon saison).

Le script Python vérifie en interne :
- Si aujourd'hui est le **premier jour ouvré du mois** → RUN ✅
- Sinon → `sys.exit(0)` propre, GitHub Actions marque le run comme success

**Exemples 2026** :

| Mois | 1er du mois | Jour publication |
|---|---|---|
| Juin | Lundi 1er | 1er juin (lundi) |
| Juillet | Mercredi 1er | 1er juillet |
| Août | Samedi 1er | **Lundi 3 août** ⬅ reporté |
| Septembre | Mardi 1er | 1er septembre |
| Octobre | Jeudi 1er | 1er octobre |
| Novembre | Dimanche 1er | **Lundi 2 novembre** ⬅ reporté |
| Décembre | Mardi 1er | 1er décembre |

---

## 🗂️ Structure du repo

```
BriefMensuelBourse/
├── brief_mensuel.py          # Pipeline complet
├── requirements.txt          # Dépendances Python
├── .github/
│   └── workflows/
│       └── brief.yml         # GitHub Actions workflow
├── assets/
│   └── music.mp3             # (optionnel) ta musique perso
├── snapshots/                # Historique mensuel (5 derniers max, auto-rotation)
│   ├── ranking_2026-05.json
│   ├── ranking_2026-06.json
│   └── ...
├── out/                      # Outputs locaux (gitignored)
└── .gitignore
```

---

## 🐛 Debug

Logs disponibles dans **Actions → Run → brief job → Run Brief Mensuel** step.

Outputs téléchargeables comme **artifacts** (30 jours de rétention).

Si le run skip (pas le premier jour ouvré), tu verras :
```
⏸  Pas le premier jour ouvré du mois.
   Aujourd'hui : 2026-05-15 (Friday)
   Attendu     : 2026-05-01 (Friday)
   → Skip propre, exit 0
```

---

## 🔧 Variables d'environnement

| Variable | Défaut | Description |
|---|---|---|
| `TEST_MODE` | `true` | `true` = 28 actions (test), `false` = ~1075 (prod) |
| `SEND_TO_WEBHOOK` | `true` | Envoyer au webhook Make.com (= publier sur LinkedIn) |
| `WEBHOOK_URL` | (vide) | URL Make.com — **obligatoire si SEND_TO_WEBHOOK=true** |
| `CODE_PARRAINAGE` | `ROTA0058` | Code parrain Boursorama affiché dans le post |
| `PARRAINAGE_URL` | `https://bour.so/p/GB93ZfQVNVr` | URL de parrainage |
| `FORCE_RUN` | `false` | Si `true`, bypass le check premier jour ouvré |

---

## 📜 Licence

Code privé · auteur **Romain Taugourdeau**.

Données : Yahoo Finance (yfinance).
Musique : Internet Archive · Adrian Diaz · CC BY-SA 4.0.
