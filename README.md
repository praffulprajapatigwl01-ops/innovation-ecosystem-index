# 🌍 Innovation Ecosystem Index: Where Should a Company Expand?

A data-driven country comparison that builds a weighted **Innovation Readiness Index** from live World Bank data, segments 189 economies into strategic quadrants, and turns the results into market-expansion recommendations.

**Skills demonstrated:** API data ingestion · data cleaning · feature engineering · index construction · regression modelling · data visualisation · business storytelling.

---

## 📌 Business Question

> *Which countries offer the strongest environment for innovation-led growth, and how should a firm prioritise markets for expansion or R&D investment?*

**Stakeholder:** a strategy / market-expansion team at a mid-size tech or consumer firm.
**Decision supported:** where to open a new office, build an R&D hub, or invest.

---

## 🗂️ Data

Live data pulled from the **[World Bank Open Data API](https://data.worldbank.org/)** (free, no authentication, credible source). Six indicators per country, most recent available year within 2015–2023:

| Indicator | World Bank code | Role |
|---|---|---|
| R&D expenditure (% GDP) | `GB.XPD.RSDV.GD.ZS` | Innovation input |
| GDP per capita (US$) | `NY.GDP.PCAP.CD` | Market wealth |
| High-tech exports (% manuf.) | `TX.VAL.TECH.MF.ZS` | Innovation output |
| Researchers per million | `SP.POP.SCIE.RD.P6` | Talent depth |
| Internet users (%) | `IT.NET.USER.ZS` | Digital readiness |
| GDP, total (US$) | `NY.GDP.MKTP.CD` | Market size |

**Final clean sample:** 189 countries (regional and income aggregates removed; countries with fewer than 4 of 6 indicators dropped; remaining gaps median-imputed).

---

## 🔬 Method

1. **Ingest** — pull each indicator for all countries over 2015–2023, keep the latest non-null value per country (`groupby().last()`).
2. **Clean** — strip World Bank aggregate codes (World, Euro area, income groups), enforce a data-completeness threshold, impute residual gaps with the median.
3. **Engineer** — min–max normalise all six indicators to 0–1 for comparability.
4. **Index** — combine into a weighted **Innovation Readiness Index (0–100)**:

   | Weight | Indicator |
   |---|---|
   | 0.25 | R&D expenditure |
   | 0.20 | Researchers per million |
   | 0.20 | High-tech exports |
   | 0.15 | Internet users |
   | 0.10 | GDP per capita |
   | 0.10 | GDP total |

   *Weights reflect a business view of what drives innovation and are stated explicitly as an assumption.*
5. **Segment** — split on median wealth and median innovation into four strategic quadrants:
   **Leaders · Risers · Undervalued · Laggards**.
6. **Model** — a linear regression of innovation factors on GDP per capita to test how much of national wealth the factors explain.

---

## 📊 Key Findings

- **Top of the index:** the Republic of Korea, United States, Israel, Singapore, Iceland, Switzerland, Hong Kong, Sweden, and Ireland — a cluster that matches established innovation leaders, validating the index.
- **Innovation explains ~52% of the variation in national wealth** (regression R² = 0.52) — a strong but honest relationship that leaves room for the many other drivers of prosperity.
- **Undervalued markets** — high innovation capacity relative to their wealth — include **Vietnam, the Philippines, and Thailand**, aligning with their real-world emergence as manufacturing and tech-services hubs. These are strong candidates for cost-efficient R&D or expansion.

---

## 💡 Recommendation

For a firm seeking innovation-led growth at lower cost, the **Undervalued** quadrant — led by Vietnam, the Philippines, and Thailand — offers innovation capacity without the wealth premium of the Leaders, making these markets strong candidates for R&D hubs or regional expansion. **Leaders** remain the choice where access to the deepest talent and ecosystems outweighs cost.

---

## ⚠️ Limitations

- The index weighting is a subjective business judgement; results shift if weights change (a sensitivity analysis is a natural extension).
- Median imputation fills missing values and may understate variation.
- The analysis is a single-year snapshot, not a trend.
- Very small economies (e.g. Liechtenstein) can score highly on per-capita and intensity ratios; a minimum-population filter is an optional refinement.
- Regression coefficients are affected by multicollinearity among the innovation indicators, so **correlation** — not individual coefficients — is used to read driver importance.

---

## 🛠️ Tech Stack

`Python` · `pandas` · `NumPy` · `requests` · `scikit-learn` · `Matplotlib` · `Seaborn` · `Plotly` · `Google Colab`

---

## 📁 Repository Contents

- `innovation_analysis.ipynb` — full analysis notebook
- `innovation_index_results.csv` — exported ranked results
- `innovation_dashboard.html` — interactive Plotly segmentation dashboard
- `README.md` — this file

---

## ▶️ Reproduce

Open the notebook in Google Colab and run all cells top to bottom. No API key required.

---

*Built as an independent portfolio project applying data analysis to an international-business and innovation question.*
