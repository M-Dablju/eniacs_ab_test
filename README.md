# 🛒 Eniac A/B Test — Button Redesign Analysis

> A full end-to-end A/B testing case study for an e-commerce homepage CTA button, covering experimental design, statistical analysis, and a data-driven recommendation.

---

## 📌 Project Overview

**Eniac** is an online Apple products retailer. Their homepage features an iPhone 13 banner with a prominent "SHOP NOW" call-to-action button. Despite high web traffic (~7,000 daily visits), the button only achieved a **~2% click-through rate (CTR)** — the biggest drop-off point in the purchase funnel.

The goal of this project: **run a rigorous A/B test to determine whether any of the proposed button redesigns could meaningfully improve CTR — or whether the original should be kept.**

---

## 🎯 Business Context

The purchase funnel from homepage visit to confirmed iPhone sale looked like this:

| Step | Visitors | Conversion Rate |
|---|---|---|
| Website visits | 50,000 | — |
| Clicks on "SHOP NOW" | 1,000 | 2% ← bottleneck |
| Select iPhone model | 300 | 30% |
| Add to basket | 100 | 33% |
| Go to checkout | 75 | 75% |
| Delivery details | 60 | 80% |
| Payment details | 40 | 67% |
| Confirmed purchase | 30 | 75% |

The "SHOP NOW" button conversion is by far the weakest link, making it the highest-leverage target for optimization.

---

## 🔬 Experiment Design

### Variants Tested

| Version | Button Color | Button Text |
|---|---|---|
| **A** (control) | White | SHOP NOW |
| **B** | Red | SHOP NOW |
| **C** | White | SEE DEALS |
| **D** | Red | SEE DEALS |

### Hypothesis

- **Null Hypothesis (H₀):** All four versions have the same CTR; observed differences are due to chance.
- **Alternative Hypothesis (H₁):** At least one version has a statistically different CTR.

### Parameters

| Parameter | Value |
|---|---|
| Statistical significance | 95% (α = 0.05) |
| Statistical power | 80% |
| Minimum detectable effect (MDE) | 20% |
| Required visitors per variant | ~19,784 |
| Test duration | 14 days (2 full business cycles) |
| Test period | Nov 2 – Nov 16, 2021 |

The MDE was set deliberately large to keep the experiment duration practical for Eniac's traffic volume. A smaller MDE would have required months of data collection.

---

## 📊 Results

### Click-Through Rates

| Version | Clicks | Visits | CTR |
|---|---|---|---|
| **A** (White / SHOP NOW) | 512 | 25,326 | 2.02% |
| **B** (Red / SHOP NOW) | 281 | 24,747 | 1.14% |
| **C** (White / SEE DEALS) | 527 | 24,876 | **2.12%** ← highest |
| **D** (Red / SEE DEALS) | 193 | 25,233 | 0.76% ← lowest |

Key observation: **white buttons substantially outperform red buttons**. The color change appears to hurt performance, while the text change has a more nuanced effect.

---

## 🧪 Statistical Analysis

### Step 1 — Chi-Square Test (all four variants)

A chi-square test on the contingency table of clicks vs. no-clicks for all four variants produced a **p-value well below 0.05**, allowing us to reject the null hypothesis. The observed differences are not due to chance.

### Step 2 — Post-Hoc Pairwise Tests (Bonferroni Adjustment)

With 4 variants, there are **6 pairwise comparisons**. Using the Bonferroni correction:

```
adjusted α = 0.05 / 6 ≈ 0.0083
```

Results showed that:
- **A vs. B** → statistically significant (white >> red)
- **A vs. D** → statistically significant
- **C vs. B** → statistically significant (white >> red)
- **C vs. D** → statistically significant
- **A vs. C** → **not statistically significant** (too close to call on CTR alone)
- **B vs. D** → **not statistically significant**

The chi-square tests confirm that **color drives the significant difference**, but cannot separate A from C on CTR alone.

---

## 📈 Secondary Metrics

Since A and C were statistically indistinguishable by CTR, secondary metrics were used to break the tie. (Note: data collection error made Version B's secondary metrics unavailable.)

### Drop-Off Rate (lower = better)

| Version | Drop-Off Rate |
|---|---|
| **A** | ~62% |
| **C** | ~71% |
| **D** | ~70% |
| B | N/A |

Version A has the **lowest drop-off rate**, meaning users who clicked it were more likely to continue through the funnel.

### Homepage-Return Rate (lower = better)

| Version | Homepage-Return Rate |
|---|---|
| **D** | ~2.6% |
| **C** | ~4.7% |
| **A** | ~5.4% |
| B | N/A |

Version D has the lowest homepage-return rate, but it also has the worst CTR by far — not a useful trade-off.

---

## ✅ Recommendation

**Do not implement any of the proposed changes. Keep the original white "SHOP NOW" button (Version A).**

None of the three redesigns produced a statistically significant improvement in CTR:

1. **Red buttons (B, D) are significantly worse** — switching to red would actively hurt performance. This is a clear, actionable finding.
2. **"SEE DEALS" text (C) shows no meaningful gain** — Version C's CTR is marginally higher than A's (2.12% vs 2.02%), but the difference is not statistically significant. We cannot conclude it performs better.
3. **Post-click behaviour favours A** — Version A has the lowest drop-off rate (~62% vs ~71% for C), meaning the users it attracts are more genuinely interested and more likely to complete a purchase. This tips the tie-break firmly in A's favour.

### 🔭 Next Steps

A null result is still a useful result — it tells us that **minor cosmetic changes to this button are unlikely to move the needle**. The more productive directions to explore are:

- **Page-level redesign** — the banner surrounding the button has a ~3.5% CTR vs the button's ~2%, suggesting users are more drawn to the image/content than the CTA itself. A bolder or repositioned button might help more than colour or copy changes.
- **Funnel optimisation below the click** — once a user clicks "SHOP NOW", 62–71% drop off before completing a purchase. That's a larger absolute opportunity than squeezing more clicks out of the button.
- **User research** — survey responses in the design phase hinted that "SHOP NOW" feels too committal for some users. A deeper qualitative study could surface a more fundamentally different CTA concept worth testing.

---

## 🗂️ Repository Structure

```
├── data/
│   ├── eniac_a.csv        # Version A click data
│   ├── eniac_b.csv        # Version B click data
│   ├── eniac_c.csv        # Version C click data
│   └── eniac_d.csv        # Version D click data
├── notebooks/
│   ├── 01_explore_data.ipynb       # EDA and CTR calculation
│   └── 02_chi_square_test.ipynb    # Statistical tests & post-hoc analysis
├── images/
│   ├── ctr_comparison.png          # CTR and click counts by version
│   ├── pairwise_pvalues.png        # Bonferroni-adjusted pairwise p-values
│   └── secondary_metrics.png      # Drop-off and homepage-return rates
└── README.md
```

---

## 🛠️ Tech Stack

- **Python 3**
- **pandas** — data loading and manipulation
- **scipy** — chi-square test (`chi2_contingency`)
- **matplotlib** — visualizations

---

## 📚 Key Concepts Demonstrated

- Purchase funnel analysis and conversion rate metrics
- A/B test experimental design (MDE, statistical power, significance thresholds)
- Chi-square test for categorical data
- Bonferroni correction for multiple comparisons (post-hoc testing)
- Decision-making when primary metrics are inconclusive — using secondary metrics

---

*Case study based on Eniac's A/B test, November 2021.*
