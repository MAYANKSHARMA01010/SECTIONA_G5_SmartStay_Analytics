# Complete Tableau Guide: Dashboard 3 - Host Performance & Guest Experience

## 1. Overview & Business Problem
**Dashboard Name:** Host Performance & Guest Experience  
**Target Audience:** Active Hosts, Platform Quality Analysts, Guests  
**Core Question:** *"WHAT makes a 5-star host? Which levers actually drive guest satisfaction?"*

### The Business Value
71.3% of rated listings are already Excellent or Perfect — meaning the competitive battlefield is in tiny decimal differences (4.75 vs 4.90). This dashboard gives hosts a precise diagnostic: which sub-score is dragging their overall rating, whether their Superhost badge is backed by real data, and exactly where they sit on the Quality × Demand quadrant.

---

## 2. Data Source & Preparation
**Data Source to Use:** `tableau_ready.csv`

**Initial Data Checks:**
- Ensure `review_scores_rating` and all sub-scores (`accuracy`, `cleanliness`, `checkin`, `communication`, `location`, `value`) are Measures (Continuous).
- Ensure `host_is_superhost` and `instant_bookable` are Dimensions (Discrete, aliased to "Superhost/Regular" and "Yes/No").
- Ensure `rating_category` is a Dimension with a logical sort order: Poor → Good → Very Good → Excellent → Perfect.
- Ensure `reviews_per_month` and `host_acceptance_rate` are Measures (Continuous).

---

## 🎨 Airbnb Color Theme & Visual Standards

Apply these colors **consistently across every chart, KPI tile, and tooltip** on this dashboard.

### Core Palette
| Role | Name | Hex | Use |
|---|---|---|---|
| 🔴 Primary Accent | Airbnb Red | `#FF5A5F` | Primary bars, KPI tile borders, key highlights |
| 🩷 Secondary Pink | Rose Blush | `#FFB3B5` | Lighter fills, hover states, secondary accents |
| 🟠 Warning / Mid | Warm Orange | `#FC642D` | Mid-tier values, moderate performers |
| 🟢 Positive / High | Teal Green | `#00A699` | Superhosts, top ratings, high scores |
| ⚫ Neutral / Low | Soft Grey | `#767676` | Regular hosts, reference lines, labels |
| ⚪ Background | Off-White | `#F7F7F7` | Dashboard canvas, KPI tile backgrounds |
| 🖤 Body Text | Near-Black | `#484848` | All titles, labels, tooltips |
| 🔺 Negative | Deep Rose | `#E15759` | Underperformers, poor ratings |

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
| Sequential Rating | `#E15759` (Poor) → `#FC642D` → `#767676` → `#00A699` (Perfect) |
| Diverging (vs avg) | `#E15759` (below) · `#767676` (at avg) · `#00A699` (above) |

### Categorical: Rating Category Colors
| Category | Color | Hex |
|---|---|---|
| Poor (<4.0) | Deep Rose | `#E15759` |
| Good (4.0–4.5) | Warm Orange | `#FC642D` |
| Very Good (4.5–4.75) | Rose Blush | `#FFB3B5` |
| Excellent (4.75–4.9) | Airbnb Red | `#FF5A5F` |
| Perfect (4.9–5.0) | Teal Green | `#00A699` |

### Categorical: Superhost vs Regular
| Status | Color | Hex |
|---|---|---|
| Superhost | Teal Green | `#00A699` |
| Regular Host | Soft Grey | `#767676` |

### Categorical: Instant Book
| Status | Color | Hex |
|---|---|---|
| Yes (Instant Book) | Airbnb Red | `#FF5A5F` |
| No | Soft Grey | `#767676` |

### Sub-Score Bullet Chart Colors
| Direction | Color | Hex |
|---|---|---|
| Below market avg | Deep Rose | `#E15759` |
| At market avg | Soft Grey | `#767676` |
| Above market avg | Teal Green | `#00A699` |

---

## 3. Calculated Fields to Create in Tableau

Before building any visualizations, create these calculated fields in Tableau:

**1. Occupancy Rate %**
```tableau
[estimated_occupancy_l365d] / 365
```
*Format as: Percentage (0 decimal places)*

**2. Superhost Revenue Premium %**
```tableau
(AVG(IF [host_is_superhost]=1 THEN [estimated_revenue_l365d] END)
- AVG(IF [host_is_superhost]=0 THEN [estimated_revenue_l365d] END))
/ AVG(IF [host_is_superhost]=0 THEN [estimated_revenue_l365d] END)
```
*Format as: Percentage (1 decimal place) — expected result: +143.0%*

**3. Rating vs Market Avg**
```tableau
[review_scores_rating] - WINDOW_AVG(AVG([review_scores_rating]))
```
*Used in bullet charts to show how a listing compares to market avg (4.77)*

**4. % Superhost**
```tableau
SUM(IF [host_is_superhost] = 1 THEN 1 ELSE 0 END) / COUNT([listing_id])
```
*Format as: Percentage (1 decimal place)*

**5. % Instant Bookable**
```tableau
SUM(IF [instant_bookable] = '1' THEN 1 ELSE 0 END) / COUNT([listing_id])
```
*Format as: Percentage (1 decimal place)*

---

## 4. Step-by-Step: Building KPI Tiles

Create five separate worksheets for the top banner KPIs.

### KPI 1: Market Avg Rating
- **Drag** `review_scores_rating` to Text.
- **Change Measure to:** Average.
- **Format:** Number (2 decimal places, e.g., 4.77 ★).
- **Label:** "Market Avg Rating".

### KPI 2: % Superhosts
- **Drag** `% Superhost` (calculated field) to Text.
- **Format:** Percentage (e.g., 37.0%).
- **Label:** "Superhost Share".

### KPI 3: Avg Reviews Per Month
- **Drag** `reviews_per_month` to Text.
- **Change Measure to:** Average.
- **Format:** Number (2 decimal places, e.g., 1.39).
- **Label:** "Avg Reviews / Month".

### KPI 4: Instant Bookable %
- **Drag** `% Instant Bookable` (calculated field) to Text.
- **Format:** Percentage (e.g., 51.8%).
- **Label:** "Instant Bookable %".

### KPI 5: Avg Host Acceptance Rate
- **Drag** `host_acceptance_rate` to Text.
- **Change Measure to:** Average.
- **Format:** Percentage (e.g., 88.8%).
- **Label:** "Avg Acceptance Rate".

---

## 5. Step-by-Step: Building the Charts

### Chart 1: Rating Distribution (Bar / Histogram)
*Goal: Show where the market clusters — how compressed the top of the scale really is.*
- **Columns:** `rating_category` (sort manually: Poor → Good → Very Good → Excellent → Perfect)
- **Rows:** `listing_id` (Count)
- **Color:** `rating_category` — Poor `#E15759` · Good `#FC642D` · Very Good `#FFB3B5` · Excellent `#FF5A5F` · Perfect `#00A699`
- **Labels:** Add % of total labels on each bar.
- **Insight to show:** 71.3% of listings are Excellent or Perfect — the competitive margin is razor thin above 4.75.

### Chart 2: Superhost vs Non-Superhost Multi-KPI (Side-by-Side Grouped Bar)
*Goal: Quantify the real dollar value of the Superhost badge.*
- **Columns:** `host_is_superhost` (aliased: Superhost / Regular)
- **Rows:** Create separate sheets per metric, or use a parameter to toggle between:
  - `AVG(estimated_revenue_l365d)` → $58,810 vs $24,197
  - `AVG(review_scores_rating)` → rating comparison
  - `AVG(Occupancy Rate %)` → occupancy comparison
  - `AVG(reviews_per_month)` → demand comparison
- **Color:** Superhost = `#00A699` (Teal Green), Regular = `#767676` (Soft Grey)
- **Reference Line:** Add an average reference line across each metric.
- **Insight to show:** Superhosts earn 143% more revenue — not just an aesthetic label, it's a financial accelerator.

### Chart 3: Sub-Score Performance vs Market Avg (Bullet Charts — 6 Sub-Scores)
*Goal: Give hosts a precise diagnostic — which sub-score to fix first.*
- **Build 6 separate bar charts** (one per sub-score), arranged vertically:
  - `review_scores_value` → Avg **4.705** ← **lowest** (biggest opportunity)
  - `review_scores_location` → Avg **4.731**
  - `review_scores_cleanliness` → Avg **4.766**
  - `review_scores_accuracy` → Avg **4.805**
  - `review_scores_checkin` → Avg **4.855**
  - `review_scores_communication` → Avg **4.866** ← **highest**
- **For each:** Drag the sub-score to Columns (Average). Add a **Reference Line** at the market average for that sub-score.
- **Color:** `#E15759` (Deep Rose) if below market avg · `#00A699` (Teal Green) if above avg · `#767676` at avg.
- **Insight to show:** Value (4.705) and Location (4.731) are the two drag scores — improving perceived value-for-money is the #1 actionable lever.

### Chart 4: Quality × Demand Strategic Quadrant (Scatter Plot)
*Goal: Place every listing into one of 4 strategic archetypes.*
- **Columns:** `reviews_per_month` (Average — proxy for demand)
- **Rows:** `review_scores_rating` (Average — quality)
- **Detail:** `listing_id` (one dot per listing)
- **Color:** `rating_category` — Poor `#E15759` · Good `#FC642D` · Very Good `#FFB3B5` · Excellent `#FF5A5F` · Perfect `#00A699`
- **Size:** `estimated_revenue_l365d` (larger dot = higher earner)
- **Reference Lines:**
  - Vertical line at X = **1.39** (market avg reviews/month)
  - Horizontal line at Y = **4.77** (market avg rating)
- **Quadrant Labels (add as annotations):**
  - Top-Right: ⭐ Stars (High Quality + High Demand)
  - Top-Left: 💎 Hidden Gems (High Quality + Low Demand)
  - Bottom-Right: ⚡ Volume Players (Low Quality + High Demand)
  - Bottom-Left: ⚠️ Underperformers (Low Quality + Low Demand)
- **Insight to show:** Hidden Gems are underpriced opportunities — high rating, low visibility. Fixing SEO/photos moves them to Stars.

### Chart 5: Top 15 Neighbourhoods by Avg Rating (Horizontal Bar)
*Goal: Show whether quality clusters geographically.*
- **Rows:** `neighbourhood_cleansed` (Top 15 by AVG rating, sorted descending)
- **Columns:** `review_scores_rating` (Average)
- **Color:** `% Superhost` density per neighbourhood — sequential `#FFB3B5` (low SH%) → `#FF5A5F` → `#E15759` (high SH%)
- **Reference Line:** Market avg rating at **4.77** — dashed, color `#767676`
- **Insight to show:** Nieuw-Zuid (4.975), Hoboken-Zuidoost (4.935), Schoonbroek-Rozemaai (4.920) lead — high Superhost density correlates with top-rated neighbourhoods.

### Chart 6: Instant Book Impact on Revenue, Rating & Occupancy (Grouped Bar)
*Goal: Give a data-backed yes/no answer on enabling Instant Book.*
- **Columns:** `instant_bookable` (Yes / No)
- **Rows:** Build 3 separate grouped bars for:
  - `AVG(estimated_revenue_l365d)` → IB: $35,388 vs Non-IB: $38,768
  - `AVG(review_scores_rating)` → IB: 4.74 vs Non-IB: 4.81
  - `AVG(Occupancy Rate %)` → IB: 21.7% vs Non-IB: 18.7%
- **Color:** IB = `#FF5A5F` (Airbnb Red), Non-IB = `#767676` (Soft Grey)
- **Insight to show:** IB boosts occupancy by +3% but trades ~0.07 rating points. For revenue-maximising hosts it's worth it; for Superhost-status protection, use with caution.

---

## 6. Dashboard Assembly & Interactivity

### Recommended Layout Container Strategy
```text
+-----------------------------------------------------------------------+
|  [Filters: Room Type] [Neighbourhood] [Superhost] [Rating Category]   |
+-----------------------------------------------------------------------+
|  [ KPI 1 ]   [ KPI 2 ]   [ KPI 3 ]   [ KPI 4 ]   [ KPI 5 ]          |
+-----------------------------------------------------------------------+
|                                  |                                    |
|   Chart 1: Rating Distribution   |   Chart 2: Superhost vs Non        |
|   (Histogram by rating_category) |   Multi-KPI Grouped Bar            |
|                                  |                                    |
+-----------------------------------------------------------------------+
|                                  |                                    |
|   Chart 3: Sub-Score Bullets     |   Chart 4: Quality × Demand        |
|   (6 sub-scores vs market avg)   |   Strategic Quadrant Scatter       |
|                                  |                                    |
+-----------------------------------------------------------------------+
|                   |                                                   |
|   Chart 5:        |   Chart 6: Instant Book Impact                    |
|   Top 15 Hoods    |   Revenue / Rating / Occupancy Grouped Bar        |
|   by Avg Rating   |                                                   |
+-----------------------------------------------------------------------+
```

### Dashboard Actions (Crucial for Interactivity)
1. **Filter Action (Neighbourhood):**
   - Source: Chart 5 (Top Hoods bar)
   - Target: All other charts on the dashboard.
   - Run on: Select
   - Clearing the selection: Show all values.
2. **Filter Action (Rating Category):**
   - Source: Chart 1 (Rating Distribution)
   - Target: Chart 4 (Quadrant Scatter) and Chart 2 (Superhost comparison).
   - Run on: Select.
3. **Filter Action (Superhost):**
   - Source: Chart 2 (Superhost grouped bar)
   - Target: Chart 3 (Sub-scores), Chart 4 (Scatter), Chart 5 (Hoods by rating).
   - Run on: Select.

---

## 7. Key Business Insights (The "So What?")

When presenting this dashboard, these are the narrative talking points you deliver:

1. **The Rating Compression Problem — 71.3% are Excellent or Perfect:**
   *Insight:* The histogram shows the market has converged at the top — 33.8% Perfect (4.9–5.0) and 37.5% Excellent (4.75–4.9). Beating the competition is no longer about "being good" — it's about optimising sub-scores in the 4.70–4.95 range where tiny improvements change search ranking.

2. **The Superhost Badge = +143% Revenue (+$34,613/year):**
   *Insight:* The side-by-side chart proves Superhosts earn **$58,810/yr vs $24,197/yr** for regular hosts. The ROI on achieving Superhost status — investing in professional cleaning, faster communication, and a higher acceptance rate — pays back in the first month of improved bookings.

3. **Value (4.705) is the Weakest Sub-Score — Fix It First:**
   *Insight:* The bullet charts reveal that `review_scores_value` is the lowest of all 6 sub-scores at 4.705. Guests consistently feel pricing is slightly high for what they get. Hosts can address this by adding amenities (fast WiFi, quality toiletries, welcome basket) without dropping price.

4. **Hidden Gems Are an Untapped Opportunity:**
   *Insight:* The Quality × Demand quadrant reveals listings with ratings above 4.77 but review velocity below 1.39/month. These are excellent properties that are invisible. The fix is not product quality — it's marketing: better photos, SEO-optimised title, and enabling Instant Book.

5. **Instant Book: A Calculated Trade-Off:**
   *Insight:* The grouped bar shows IB = 21.7% occupancy vs 18.7% without it (+3%), but IB listings rate slightly lower (4.74 vs 4.81). For hosts whose priority is revenue and occupancy, enable it. For hosts protecting a 4.9+ rating to retain Superhost, consider the 0.07-point rating cost carefully.
