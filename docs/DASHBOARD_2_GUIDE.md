# Complete Tableau Guide: Dashboard 2 - Revenue & Pricing Strategy

## 1. Overview & Business Problem
**Dashboard Name:** Revenue & Pricing Strategy  
**Target Audience:** Current Hosts, Prospective Hosts, Property Managers  
**Core Question:** *"HOW should I price my property? What actually drives revenue?"*

### The Business Value
Pricing mistakes are the #1 silent revenue killer for hosts. This dashboard replaces guesswork with data by revealing the "revenue sweet spot," quantifying the value of being a Superhost, and comparing pricing efficiency across different neighborhoods and property types.

---

## 2. Data Source & Preparation
**Data Source to Use:** `tableau_ready.csv`

**Initial Data Checks:**
- Ensure `listing_id` is a Dimension (String/Discrete).
- Ensure `price`, `estimated_revenue_l365d`, and `revpar` are Measures (Continuous).
- Ensure `host_is_superhost` and `instant_bookable` are Dimensions (Discrete, ideally aliased to "Yes/No" or "Superhost/Regular").
- Ensure `latitude` and `longitude` have the correct geographic roles assigned.

---

## 🎨 Airbnb Color Theme & Visual Standards

Apply these colors **consistently across every chart, KPI tile, and tooltip** on this dashboard.

### Core Palette
| Role | Name | Hex | Use |
|---|---|---|---|
| 🔴 Primary Accent | Airbnb Red | `#FF5A5F` | Primary bars, KPI tile borders, key highlights |
| 🩷 Secondary Pink | Rose Blush | `#FFB3B5` | Lighter fills, hover states, secondary accents |
| 🟠 Warning / Mid | Warm Orange | `#FC642D` | Mid-tier values, moderate performers |
| 🟢 Positive / High | Teal Green | `#00A699` | Top earners, high scores, positive deltas |
| ⚫ Neutral / Low | Soft Grey | `#767676` | Reference lines, non-primary bars, labels |
| ⚪ Background | Off-White | `#F7F7F7` | Dashboard canvas, KPI tile backgrounds |
| 🖤 Body Text | Near-Black | `#484848` | All titles, labels, tooltips |
| 🔺 Negative | Deep Rose | `#E15759` | Underperformers, zero-revenue zones |

### How to Apply in Tableau
1. **Dashboard Canvas Background:** `#F7F7F7` → Format → Dashboard → Background Color.
2. **KPI Tile Styling:** White background with a `#FF5A5F` top-border accent (use a colored rectangle shape above each number tile).
3. **Chart Titles:** Font `#484848`, Bold, 13pt. Sheet titles: uppercase.
4. **Reference Lines:** Color `#767676`, dashed style, label font `#484848`.
5. **Tooltips:** Dark background `#484848`, white text `#F7F7F7`, emoji icons for each field.
6. **Navigation Bar:** Background `#FF5A5F`, button text white `#FFFFFF`.

### Colour Scales
| Scale Type | Low → High |
|---|---|
| Sequential Revenue | `#FFB3B5` → `#FF5A5F` → `#E15759` |
| Sequential Score | `#F7F7F7` → `#767676` → `#484848` |
| Diverging (vs avg) | `#E15759` (below) · `#767676` (at avg) · `#00A699` (above) |

### Categorical: Room Type Colors
| Room Type | Color | Hex |
|---|---|---|
| Entire home/apt | Airbnb Red | `#FF5A5F` |
| Private room | Warm Orange | `#FC642D` |
| Hotel room | Teal Green | `#00A699` |
| Shared room | Soft Grey | `#767676` |

### Categorical: Price Tier Colors
| Tier | Color | Hex |
|---|---|---|
| Budget (<$75) | Soft Grey | `#767676` |
| Mid ($75–150) | Rose Blush | `#FFB3B5` |
| Mid-High ($150–300) | Warm Orange | `#FC642D` |
| High ($300–600) | Airbnb Red | `#FF5A5F` |
| Luxury (>$600) | Deep Rose | `#E15759` |

### Categorical: Revenue Tier Colors
| Tier | Color | Hex |
|---|---|---|
| No Revenue | Deep Rose | `#E15759` |
| Low (<$5k) | Soft Grey | `#767676` |
| Mid ($5k–20k) | Rose Blush | `#FFB3B5` |
| High ($20k–50k) | Warm Orange | `#FC642D` |
| Top Earner (>$50k) | Teal Green | `#00A699` |

---

## 3. Calculated Fields to Create in Tableau

Before building any visualizations, create these calculated fields in Tableau:

**1. Occupancy Rate %**
```tableau
[estimated_occupancy_l365d] / 365
```
*Format as: Percentage (0 decimal places)*

**2. RevPAR (Revenue Per Available Room)**
*(Note: Use this if `revpar` is not already in your dataset, or to ensure accuracy)*
```tableau
([price] * [Occupancy Rate %])
```
*Format as: Currency (Standard)*

**3. Superhost Revenue Premium %**
```tableau
(AVG(IF [host_is_superhost]=1 THEN [estimated_revenue_l365d] END)
- AVG(IF [host_is_superhost]=0 THEN [estimated_revenue_l365d] END))
/ AVG(IF [host_is_superhost]=0 THEN [estimated_revenue_l365d] END)
```
*Format as: Percentage (1 decimal place)*

**4. Top Earner Threshold (90th Percentile)**
```tableau
PERCENTILE([estimated_revenue_l365d], 0.90)
```
*Format as: Currency (Standard)*

---

## 4. Step-by-Step: Building KPI Tiles

Create five separate worksheets for the top banner KPIs.

### KPI 1: Avg Estimated Revenue
- **Drag** `estimated_revenue_l365d` to Text.
- **Change Measure to:** Average.
- **Format:** Currency (e.g., $37,016).
- **Label:** "Avg Annual Revenue".

### KPI 2: Median Occupancy Rate
- **Drag** `Occupancy Rate %` to Text.
- **Change Measure to:** Median.
- **Format:** Percentage (e.g., 8.2%).
- **Label:** "Median Occupancy".

### KPI 3: Avg RevPAR
- **Drag** `revpar` to Text.
- **Change Measure to:** Average.
- **Format:** Currency (e.g., $101.41).
- **Label:** "Avg RevPAR".

### KPI 4: Top 10% Revenue Threshold
- **Drag** `Top Earner Threshold` (or `estimated_revenue_l365d` set to Percentile 90) to Text.
- **Format:** Currency (e.g., $86,000).
- **Label:** "Top 10% Revenue Threshold".

### KPI 5: Superhost Revenue Premium
- **Drag** `Superhost Revenue Premium %` to Text.
- **Format:** Percentage (+143.0%).
- **Coloring (Optional):** Put the calculation on the Color mark (Green for positive).
- **Label:** "Superhost Revenue Premium".

---

## 5. Step-by-Step: Building the Charts

### Chart 1: Price vs Revenue Sweet Spot (Scatter Plot)
*Goal: Find the optimal price range where revenue peaks.*
- **Columns:** `price` (Continuous Dimension, or Average Measure)
- **Rows:** `estimated_revenue_l365d` (Average)
- **Detail:** `listing_id` (so every dot is a property)
- **Color:** `room_type` — Entire home `#FF5A5F` · Private room `#FC642D` · Hotel `#00A699` · Shared `#767676`
- **Size:** `estimated_occupancy_l365d` (Demand)
- **Analytics Pane:** Drag a **Trend Line** to the view.
- **Insight to show:** The median price is $150; avg is $865.90 (skewed by luxury outliers). Revenue peaks in the Mid–Mid-High tier. Beyond $600/night, occupancy drops sharply (avg 20.3% overall).

### Chart 2: Revenue Distribution by Price Tier (Box & Whisker Plot)
*Goal: Show the reality of earnings within different price categories.*
- **Columns:** `price_tier`
- **Rows:** `estimated_revenue_l365d` (Continuous)
- **Detail:** `listing_id` (to generate the distribution dots)
- **Analytics Pane:** Drag a **Box Plot** to the view.
- **Color:** `price_tier` — Budget `#767676` · Mid `#FFB3B5` · Mid-High `#FC642D` · High `#FF5A5F` · Luxury `#E15759`
- **Insight to show:** High price tiers have massive variance; budget tiers are highly concentrated.

### Chart 3: RevPAR by Neighbourhood (Horizontal Bar + Reference Line)
*Goal: Rank neighbourhoods by pricing efficiency.*
- **Rows:** `neighbourhood_cleansed` (Sort Descending by RevPAR)
- **Columns:** `revpar` (Average)
- **Color:** `revpar` — sequential scale `#FFB3B5` (low) → `#FF5A5F` → `#E15759` (top RevPAR neighbourhoods)
- **Reference Line:** Market median RevPAR = `$12.08` — dashed, color `#767676`
- **Filter:** Keep Top 15 or Top 20 Neighbourhoods to avoid clutter.

### Chart 4: Revenue + Occupancy by Room Type (Dual-Axis Combo)
*Goal: Identify which property types are mispriced (high occupancy/low revenue, etc.).*
- **Columns:** `room_type`
- **Rows:** `estimated_revenue_l365d` (Average) AND `Occupancy Rate %` (Average)
- **Setup Dual Axis:** Right-click the second measure on the Rows shelf -> Dual Axis.
- **Synchronize Axes:** No (because one is $, one is %).
- **Marks Card:** Set Revenue to **Bar** (`#FF5A5F`), set Occupancy to **Line** with circles (`#00A699`).

### Chart 5: Revenue Concentration Heatmap (Highlight Table)
*Goal: Show where the top-earning properties are clustered geographically.*
- **Columns:** `revenue_tier` (Low, Mid, High, Top Earner)
- **Rows:** `neighbourhood_cleansed` (Top 20)
- **Text:** `listing_id` (Count)
- **Color:** `listing_id` (Count) — sequential `#FFB3B5` (few) → `#FF5A5F` → `#E15759` (many/concentrated).

---

## 6. Dashboard Assembly & Interactivity

### Recommended Layout Container Strategy
```text
+-----------------------------------------------------------------------+
|  [Filters: Room Type] [Neighbourhood] [Superhost] [Price Range]       |
+-----------------------------------------------------------------------+
|  [ KPI 1 ]   [ KPI 2 ]   [ KPI 3 ]   [ KPI 4 ]   [ KPI 5 ]            |
+-----------------------------------------------------------------------+
|                                  |                                    |
|   Chart 1: Price vs Revenue      |   Chart 2: Box Plot                |
|   Scatter Plot (Sweet Spot)      |   Revenue by Price Tier            |
|                                  |                                    |
+-----------------------------------------------------------------------+
|                                  |                                    |
|   Chart 3: RevPAR by             |   Chart 4: Dual-Axis               |
|   Neighbourhood (Bar)            |   Rev & Occupancy by Room Type     |
|                                  |                                    |
+-----------------------------------------------------------------------+
|                                                                       |
|   Chart 5: Heatmap Table (Revenue Tier x Neighbourhood)               |
|                                                                       |
+-----------------------------------------------------------------------+
```

### Dashboard Actions (Crucial for Interactivity)
1. **Filter Action (Neighbourhood):** 
   - Source: Chart 3 (RevPAR Bar) and Chart 5 (Heatmap)
   - Target: All other charts on the dashboard.
   - Run on: Select
   - Clearing the selection: Show all values.
2. **Filter Action (Room Type):**
   - Source: Chart 4 (Dual-Axis)
   - Target: All other charts.
   - Run on: Select.

---

## 7. Key Business Insights (The "So What?")

When presenting this dashboard, these are the narrative talking points you deliver:

1. **The Revenue Sweet Spot — Mid to Mid-High Tier:**
   *Insight:* The scatter plot reveals that median price is $150/night, but the avg is $865.90 — massively skewed by luxury outliers. The Mid ($75–150) and Mid-High ($150–300) tiers show the most consistent revenue. Beyond the Luxury tier (>$600), occupancy collapses below the market avg of 20.3%, erasing the price premium.
2. **The Superhost Premium is Enormous — +143%:**
   *Insight:* Becoming a Superhost isn't just an ego boost — Superhosts earn **$58,810/yr vs $24,197/yr for regular hosts, a +143% revenue premium**. This is the single highest-ROI action any host can take. Invest in cleanliness, communication, and acceptance rate.
3. **27.1% of Listings Earn Zero Revenue:**
   *Insight:* More than 1 in 4 listings generates absolutely no revenue. The heatmap reveals these are concentrated in specific neighbourhoods and price tiers — a critical signal for investors evaluating market entry risk.
4. **RevPAR Gap: Parthum Wan ($699) vs Median ($12.08) — 57× Difference:**
   *Insight:* The RevPAR bar chart exposes a massive location premium. Bangkok neighbourhoods (Parthum Wan, Bang Rak, Vadhana) dominate the top with RevPAR 10–57× above the market median. Location efficiency matters far more than nightly price.
5. **Instant Book: More Bookings, Slightly Lower Rating:**
   *Insight:* Instant Book listings show 21.7% occupancy vs 18.7% for non-IB — a meaningful 3% boost in demand. However, IB listings average a 4.74 rating vs 4.81 for non-IB. For revenue-maximising hosts, IB is worth enabling; for rating-focused Superhosts, the trade-off warrants caution.
