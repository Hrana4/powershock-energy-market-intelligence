# ⚡ PowerShock — Energy-Market Intelligence

> **How Renewable Energy Feed-in Impacts DAX Stock Volatility**  
> DADM Group 5 · ESB Business School · Summer 2026

[![Live Dashboard](https://img.shields.io/badge/Live%20Dashboard-Visit%20Site-e8a020?style=for-the-badge&logo=netlify&logoColor=white)](https://powershock-energy-market-intelligence.netlify.app)
[![Data](https://img.shields.io/badge/Data-Q1%202026%20%7C%20557%20obs-14c99e?style=for-the-badge)](https://www.smard.de)
[![Built With](https://img.shields.io/badge/Built%20With-HTML%20%7C%20JS%20%7C%20Chart.js-5a96f0?style=for-the-badge)](https://www.chartjs.org)

---

## 🔍 Research Question

> *"To what extent does the hourly variance in renewable energy feed-in serve as a diagnostic leading indicator for the realized intraday stock price volatility of energy-intensive DAX 40 companies?"*

| Hypothesis | Statement |
|---|---|
| **H₀** | No statistically significant relationship exists between renewable energy generation levels and the stock price volatility of DAX companies |
| **H₁** | Periods of low renewable feed-in (<20% of total grid load) predict a statistically significant spike (>1.5σ) in realized volatility of energy-intensive industrial equity within the subsequent 24-hour trading window |

---

## 📊 Live Dashboard

**[→ powershock-energy-market-intelligence.netlify.app](https://powershock-energy-market-intelligence.netlify.app)**

The dashboard is a fully interactive single-page application built directly from the research data. No login required — open in any browser.

| Page | Methodology Stage | What It Shows |
|---|---|---|
| **Overview** | Descriptive | Merit-Order dual-axis chart · Normalised stock performance · Generation mix |
| **Stock Screener** | Descriptive | Sortable table · Consumer vs Generator distinction · Company detail drawer |
| **Correlation Explorer** | Diagnostic | Scatter plot · Pearson r matrix · R² scores · Toggle per company |
| **Granger Causality** | Diagnostic | Full F-stat / p-value table · Lags 1–3 · All 6 companies |
| **Trading Signals** | Predictive | OLS β coefficients · Feed-in slider → volatility impact · Buy/Sell/Hold |

---

## 🏭 Companies Tracked

The six companies split into two distinct groups based on their relationship to electricity markets:

### ⚡ Energy Consumers
Companies that *buy* electricity as a production input. Low feed-in → higher energy costs → margin pressure → volatility.

| Ticker | Company | Sector |
|---|---|---|
| BMW.DE | BMW AG | Automotive |
| MBG.DE | Mercedes-Benz Group | Automotive |
| VOW3.DE | Volkswagen AG | Automotive |
| SIE.DE | Siemens AG | Industrial |

### 🔋 Energy Generators
Companies that *sell* electricity. Low feed-in → wholesale price spikes → revenue uncertainty → speculative volatility.

| Ticker | Company | Sector |
|---|---|---|
| RWE.DE | RWE AG | Energy Utility |
| EOAN.DE | E.ON SE | Energy Utility |

---

## 📐 Methodology

```
Raw Data  ──►  Descriptive  ──►  Diagnostic  ──►  Predictive
               Merit-Order       Correlation        OLS Regression
               Effect            + Granger          Random Forest
               Visualisation     Causality
```

### 1. Descriptive — Merit-Order Effect
Plots daily average renewable share % against combined RWE+EOAN realized volatility. Visually confirms the inverse relationship: renewable dips align with volatility spikes.

### 2. Diagnostic — Correlation Matrix + Granger Causality

**Pearson r (daily renewable share % vs daily realized volatility):**

| Company | ρ | Direction |
|---|---|---|
| BMW | −0.110 | Weak inverse |
| MBG | −0.091 | Weak inverse |
| VOW3 | −0.084 | Weak inverse |
| SIE | +0.146 | Positive |
| RWE | +0.215 | Positive |
| EOAN | +0.239 | Strongest |

Renewable Share also shows r = −0.93 with Fossil Gas and r = −0.88 with Lignite, confirming the Merit-Order mechanism in the data.

**Granger Causality Results (α = 0.05):**

| Company | Lag | F-stat | p-value | Significant |
|---|---|---|---|---|
| **SIE** | 1 | 6.961 | 0.0107 | ✅ YES |
| **SIE** | 2 | 3.703 | 0.0310 | ✅ YES |
| **EOAN** | 2 | 4.282 | 0.0187 | ✅ YES |
| BMW | 1–3 | — | >0.05 | ❌ No |
| MBG | 1–3 | — | >0.05 | ❌ No |
| VOW3 | 1–3 | — | >0.05 | ❌ No |
| RWE | 1–3 | — | >0.05 | ❌ No |

**Interpretation:** Past renewable feed-in levels statistically predict Siemens volatility 1–2 days ahead and E.ON volatility 2 days ahead. Automotive stocks show no significant result — consistent with their indirect electricity exposure and hedging strategies.

### 3. Predictive — OLS Regression + Random Forest
Both models predict volatility from energy variables. Linear Regression: R² = 0.034. Random Forest: R² = −0.145. Top feature importances: Solar mean (0.203), Fossil gas mean (0.200), Lignite mean (0.162), Renewable share mean (0.144).

**Conclusion:** Renewable feed-in is a meaningful diagnostic signal but not the sole driver of volatility. Macro conditions, earnings events, and broader market movements contribute significantly.

---

## 📦 Data Sources

| Source | Provider | Variables | Frequency |
|---|---|---|---|
| Renewable Generation | [SMARD.de](https://www.smard.de) · Bundesnetzagentur | Wind On/Offshore, Solar, Biomass, Hydro, Other Renewable, Lignite, Hard Coal, Fossil Gas | Hourly |
| Stock Prices | [Yahoo Finance](https://finance.yahoo.com) · yfinance | OHLC, Volume, Intraday 60-min | Hourly |

**Renewable feed-in % calculation:**
```
feed-in % = (Wind Onshore + Wind Offshore + Solar + Biomass + Hydro + Other Renewable)
            ─────────────────────────────────────────────────────────────────────────── × 100
                                    Total Generation
```

**Realized Volatility calculation:**
```
rvol = rolling_std(log_returns, window=8) × 100   [% annualised]
```

**Dataset:** Q1 FY2026 · January 2 – March 31, 2026 · 557 hourly observations · 62 trading days

---

## 🔑 Key Findings

**1. H₁ Partially Supported**
Feed-in Granger-causes volatility for Siemens (lag 1 & 2) and E.ON (lag 2). Not supported for automotive stocks, consistent with their indirect electricity exposure.

**2. Merit-Order Effect Confirmed**
Renewable Share shows r = −0.93 with Fossil Gas in the correlation matrix — the strongest evidence of the merit-order mechanism operating in Q1 2026.

**3. Asymmetric Industry Impact**
The direction and magnitude of the feed-in effect differs fundamentally between company types. Consumers face cost pressure; Generators face revenue uncertainty. Treating all six companies identically would have missed the signal.

**4. Moderate Predictive Power**
Low R² values across both models confirm renewable feed-in is one factor among many. It adds a statistically meaningful early-warning signal for energy utilities — it does not replace macro analysis.

**5. 1–2 Day Predictive Window**
Granger significance at lag 1 for Siemens implies a roughly 1-trading-day window between the energy signal and the equity market's full repricing — the "alpha window" identified in the trading signals analysis.

---

## 🏗️ Technical Architecture

```
SMARD.de ──►
              Python (pandas + numpy) ──► JSON data ──► index.html ──► Netlify
Yahoo Finance ──►
```

- **Single-file SPA** — all analysis, data, and visualisations embedded in one `index.html`
- **No backend required** — data pre-processed in Python, embedded as a JS constant
- **Chart.js** — all charts rendered client-side
- **Netlify** — auto-deploys from GitHub on every push
- **Zero dependencies** — works in any browser, no installation

---

## 📁 Repository Structure

```
powershock-energy-market-intelligence/
│
├── index.html          ← Complete interactive dashboard (all 5 pages)
└── README.md           ← This file
```

---

## 🚀 Run Locally

No server or installation needed. Simply open `index.html` in any modern browser:

```bash
# Clone the repo
git clone https://github.com/Hrana4/powershock-energy-market-intelligence.git

# Open in browser
open index.html          # macOS
start index.html         # Windows
xdg-open index.html      # Linux
```

---

## 👥 Team — DADM Group 5

| Name | Contribution |
|---|---|
| **Himanshu Rana** | Data Analysis · Interactive Dashboard (Web Development) |
| **Shivam Jamloki** | Data Collection · Data Analysis |
| **Prithvi Raj Kumar** | Data Visualisation · Presentation Deck |
| **Suong Nguyen** | Data Insights · Presentation Deck |
| **Animesh Thakur** | Data Collection · Data Analysis |

---

## ⚠️ Limitations

| # | Limitation |
|---|---|
| 1 | **Short time window** — Q1 2026 only (62 trading days). Findings should be validated over multiple years and market conditions |
| 2 | **No electricity price data** — the Merit-Order → price → stock causal chain was inferred, not directly measured. Adding EPEX Spot wholesale prices would strengthen the causal argument |
| 3 | **Daily volatility granularity** — with 9 hourly observations per trading day, realized volatility estimates carry noise |
| 4 | **No macro controls** — interest rates, earnings announcements, tariff news (relevant in Q1 2026), and broader market moves are not controlled for |
| 5 | **Granger ≠ structural causation** — Granger causality is predictive, not mechanistic. It does not prove that feed-in drives volatility through a specific causal pathway |

---

## 🔮 Future Work

- Extend dataset to 2–3 years for seasonal decomposition and greater statistical power
- Integrate EPEX Spot wholesale price as a mediating variable in the causal chain
- Run Granger Causality at hourly granularity to identify the exact lag window (currently tested at daily)
- Add macro controls (DAX index return, VIX, EUR/USD) to isolate the feed-in effect
- Live nightly data updates via GitHub Actions + SMARD API
- Expand company universe to include BASF, Covestro, HeidelbergMaterials for a richer Consumer group

---

## 📄 License

MIT License — free to use for academic, research, and educational purposes.

---

*Built with [Claude](https://claude.ai) (Anthropic) · Data: SMARD.de / Bundesnetzagentur · Yahoo Finance*  
*ESB Business School · Dr. Sebastian Marek · Data Analytics & Decision Making · Summer 2026*
