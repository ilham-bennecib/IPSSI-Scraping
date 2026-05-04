# TP Solo - Web Scraping Finance & Analyse CAC40

## Problématique

**Le cours du CAC40 varie-t-il davantage le lendemain d'une forte actualité négative ?**

---

## Structure du projet

```
TP SOLO/
├── Finance_bours_scraping.ipynb   # Scraping des cotations CAC40 (Boursorama)
├── Finance_echo_scraping.ipynb    # Scraping des articles (Les Echos)
├── Clean_data.ipynb               # Nettoyage, analyse et visualisation
├── quotation.csv                  # Cotations CAC40 sur 3 mois
├── articles.csv                   # Articles scrapés (Les Echos - Monde)
├── articles_with_trend.csv        # Articles avec classification (positif/négatif/neutre)
├── CAC40_*.txt                    # Fichiers bruts téléchargés depuis Boursorama
└── PowerBi_TP_SOLO.pbix           # Dashboard Power BI
```

---

## Etapes du projet

### 1. Scraping des cotations CAC40 — `Finance_bours_scraping.ipynb`

- Source : Boursorama (page CAC40)
- Outil : **Selenium** + ChromeDriver
- Sélection de la période **3 mois** via le bouton "3M"
- Téléchargement automatique du fichier `.txt` sans popup
- Export final dans `quotation.csv`

Données collectées : date, ouverture, haut, bas, clôture, volume

### 2. Scraping des articles — `Finance_echo_scraping.ipynb`

- Source : Les Echos - section Monde
- Outils : **Selenium** + **BeautifulSoup**
- Parcours de 25 pages maximum pour couvrir 3 mois d'articles
- Parsing des dates en français (ex: "Publié le 3 févr. à 12:08")
- Déduplication des titres
- Export dans `articles.csv`

Résultat : **877 articles** collectés entre février et mai 2026

### 3. Nettoyage et analyse — `Clean_data.ipynb`

**Nettoyage :**
- Suppression des colonnes inutiles
- Conversion et harmonisation des dates
- Filtrage sur les 90 derniers jours

**Classification des articles :**
- Chaque titre est classé en `positive`, `negative` ou `neutral` par mots-clés
- Exemples de mots négatifs : guerre, crise, chute, droits de douane, tensions...
- Exemples de mots positifs : accord, croissance, hausse, optimisme...

**Analyse J-1 :**
- Les articles du jour J sont décalés d'un jour pour mesurer leur impact sur les cours du jour J+1
- Calcul de la variation journalière (`pct_change`) sur le cours de clôture

**Résultats :**

| Contexte | Amplitude moyenne | Direction moyenne |
|---|---|---|
| Forte actualité négative | **0.97%** | -0.27% |
| Actualité calme | **0.70%** | +0.21% |

Le CAC40 varie **0.27% de plus** en valeur absolue après une forte actualité négative.
La direction tend également à être baissière après une actualité négative (-0.27% vs +0.21%).

> **Note statistique :** p-value = 0.20 — la différence n'est pas statistiquement significative, les résultats sont à interpréter avec prudence.

---

## Technologies utilisées

| Outil | Usage |
|---|---|
| Python 3.10 | Langage principal |
| Selenium | Scraping dynamique (JS) |
| BeautifulSoup | Parsing HTML |
| Pandas | Manipulation des données |
| Matplotlib | Visualisation |
| SciPy | Test statistique (t-test de Welch) |
| Power BI | Dashboard interactif |

---

## Installation

```bash
pip install selenium webdriver-manager beautifulsoup4 requests pandas matplotlib scipy
```

ChromeDriver est géré automatiquement par `webdriver-manager`.

---

## Lancement

Exécuter les notebooks dans cet ordre :

1. `Finance_bours_scraping.ipynb`
2. `Finance_echo_scraping.ipynb`
3. `Clean_data.ipynb`
