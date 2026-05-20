# Solar Market Opportunity Index: Where to Sell Next

> A data-driven decision framework for solar installers to identify the highest-potential, lowest-saturation US markets — built on Google's Project Sunroof dataset.

**Author:** Thanushree Keshava Murthy
**Course:** Visual Analytics, Purdue University

---

## The Question

**Where should a solar installer focus next?**

US solar adoption is uneven. Some regions have abundant sunlight, high-quality roofs, and few installations — clear opportunity. Others are saturated with competition or have poor solar economics. This project quantifies that landscape across **9,552 US regions** to recommend specific markets.

---

## The Answer (TL;DR)

A **2×2 strategic quadrant** that any sales team can act on:

| | Low Saturation | High Saturation |
|---|---|---|
| **High Opportunity** | 🟢 **GO NOW** — sell aggressively | 🟠 **COMPETE** — only with differentiation |
| **Low Opportunity** | 🔵 **NURTURE** — long-term plays | ⚪ **CROWDED** — avoid |

**Top-10 GO NOW states** (highest average Opportunity Index, lowest existing saturation):
Arizona · Nevada · Bayamón (PR) · New Mexico · Texas · Florida · Oklahoma · Carolina · Idaho · Utah

**Projected impact** (at 200 installs across the top-10 GO NOW regions):
- ~25.7 MW installed capacity
- ~4,046,375 metric tons CO₂ offset

---

## The Methodology

### Opportunity Index

A composite score combining solar viability and market openness:

```
Opportunity Index = 0.4 × normalized(Yearly Sunlight kWh)
                  + 0.4 × normalized(% Qualified Roofs)
                  + 0.2 × (1 - normalized(Existing Installs))
```

**Rationale for weights:**
- Sunlight and roof quality are equally weighted (40% each) as they're the *physical* drivers of solar economics — high sunlight on a roof that can't host panels is wasted potential
- Saturation gets less weight (20%) because some saturated markets still have headroom

All inputs are min-max normalized to [0, 1] before combining.

### Saturation Index

```
Saturation = normalized(Existing Installs Count)
```

Pure measure of how penetrated the market already is.

### Quadrant Thresholds

- **Opportunity threshold** = 75th percentile across all regions (top quartile = "high opportunity")
- **Saturation threshold** = median across all regions

This gives a defensible split that doesn't depend on arbitrary cutoffs.

### Visual Encoding

The flagship bubble plot encodes four dimensions:
- **X-axis:** Market saturation (existing installs, normalized)
- **Y-axis:** Opportunity index
- **Bubble size:** Annual CO₂ offset potential (metric tons)
- **Bubble color:** Flat-roof share (commercial / industrial flag — higher = more accessible for large installations)

---

## The Data

**Source:** [Google Project Sunroof](https://sunroof.withgoogle.com/data-explorer) — public dataset aggregating solar viability by US census tract / zip code

**Sample:** 9,552 regions across all 50 states + Puerto Rico

**Key variables used:**
| Variable | Description |
|---|---|
| `yearly_sunlight_kwh_total` | Total annual solar energy hitting qualified roofs (kWh) |
| `percent_qualified` | % of buildings with roofs suitable for solar |
| `existing_installs_count` | Current number of solar installations in the region |
| `carbon_offset_metric_tons` | Estimated CO₂ offset if all qualified roofs adopted solar |
| `number_of_panels_{f,n,s,e,w}` | Panel orientation breakdown (flat/N/S/E/W) |
| `kw_median` | Median system size in kW |

**Summary statistics:**
- Yearly sunlight: median ~98M kWh, max ~1.19B kWh
- Percent qualified: median 83%, mean 81%
- Existing installs: median 7, max 1,954 (highly right-skewed)
- CO₂ offset: median ~51K tons, max ~802K tons

---

## Key Findings

### 1. Solar opportunity is geographically concentrated

The Southwest dominates the GO NOW quadrant — Arizona, Nevada, and New Mexico lead on the Opportunity Index, driven by both abundant sunlight AND high qualified-roof rates.

### 2. Texas and Florida are leading second-tier markets

Both states have large addressable populations with high opportunity scores but variable saturation by region — meaning **sub-state targeting matters**.

### 3. The Northeast is underperformed by aggregate metrics

Despite high installation counts in some states (suggesting "saturation"), New England's lower sunlight scores keep its overall Opportunity Index lower than the Southwest, even in low-saturation regions.

### 4. Flat roofs cluster in commercial corridors

The `flat_share` variable (bubble color in the killer chart) shows that high-flat-roof regions concentrate around urban commercial centers — these are higher-margin install opportunities and warrant different sales approaches than residential markets.

### 5. The top-10 GO NOW regions could deliver ~4M tons of CO₂ offset

A modest scenario (200 installs per region × 10 regions × average median system size) yields ~25.7 MW of capacity and 4M+ tons of CO₂ offset. Useful for ESG-tied marketing or grant applications.

---

## Limitations

Including this because honest analysts disclose what they don't know.

1. **Project Sunroof is not exhaustive.** Google's coverage skews toward populated areas. Rural and Alaskan markets are under-represented in the source data.

2. **No demand-side data.** The Opportunity Index captures *physical* potential (sunlight + roof quality + market openness) but not **economic** demand (household income, electricity prices, local incentive programs). A region with high opportunity but $0.08/kWh electricity may have lower actual demand than the index suggests.

3. **No competitor-level granularity.** Saturation is measured by total installs, not by installer brand. A region "dominated" by one large installer may be more or less competitive than the metric implies.

4. **Static snapshot.** This is a one-time analysis. Solar adoption rates change quickly, especially with policy changes (e.g., post-IRA tax credit dynamics).

5. **Equal-weight scoring is a choice.** The 40/40/20 weighting on the Opportunity Index is defensible but not unique. A sensitivity analysis varying weights would strengthen confidence in the rankings — not implemented here.

6. **Bayamón as a "state."** The dataset includes Puerto Rico's largest municipality as a state-level entry due to its size. This isn't an error but readers may be surprised to see it in a US state ranking.

---

## How to Run

### Prerequisites

- Python 3.9+
- Project Sunroof data (`Solar_data.xlsx`) — download from [Google Project Sunroof Data Explorer](https://sunroof.withgoogle.com/data-explorer)

### Setup

```bash
git clone https://github.com/thanushreekmurthy1999-hub/solar-market-opportunity-index.git
cd solar-market-opportunity-index
pip install -r requirements.txt
```

### Run

1. Place `Solar_data.xlsx` in the project root
2. Open `solar_market_analysis.ipynb` in Jupyter or Colab
3. Run all cells — the killer bubble plot saves to `killer_where_to_sell_next.png`

---

## Repository Structure

```
solar-market-opportunity-index/
├── solar_market_analysis.ipynb   # Full pipeline: load → compute indices → visualize
├── requirements.txt              # pandas, numpy, matplotlib, openpyxl
├── README.md                     # This file
├── .gitignore                    # Excludes data, generated PNGs
└── outputs/                      # Generated charts (not committed)
```

---

## Tech Stack

- **Python** — pandas, numpy
- **Matplotlib** — custom quadrant visualizations
- **Project Sunroof API / Data Explorer** — data source
- **Excel/openpyxl** — input format handling

---

## Acknowledgments

Built as part of the Visual Analytics course at Purdue University, MS Business Analytics & Information Management program.

---

*Data source: [Google Project Sunroof](https://sunroof.withgoogle.com/data-explorer) — public dataset under Google's terms of service. Analysis and Opportunity Index methodology are original work.*
