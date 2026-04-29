# Complete Tableau Guide: Dashboard 1 - Market Overview & Investment Intelligence

## 1. Overview & Business Problem
**Dashboard Name:** Market Overview & Investment Intelligence  
**Target Audience:** Real Estate Investors, Platform Managers, Executive Stakeholders  
**Core Question:** *"WHERE should I invest? Which neighbourhoods and property types actually win?"*

### The Business Value
Choosing the wrong neighbourhood or property type is a permanent, expensive mistake. This dashboard replaces gut-feel with evidence — ranking every neighbourhood by revenue, RevPAR, and our composite Investment Potential Score, and revealing exactly where market supply and actual money are misaligned.

---

## 2. Data Source & Preparation
**Data Source to Use:** `tableau_ready.csv`

**Initial Data Checks:**
- Ensure `neighbourhood_cleansed` is a Dimension (String/Discrete).
- Ensure `latitude` and `longitude` have the correct **geographic roles** assigned (Latitude / Longitude).
- Ensure `estimated_revenue_l365d`, `revpar`, and `investment_potential` are Measures (Continuous).
- Ensure `room_type` and `price_tier` are Dimensions (Discrete).
- Ensure `host_is_superhost` is a Dimension aliased to "Superhost / Regular".

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
3. **Chart Titles:** Font `#484848`, Bold, 13pt. Sheet titles: uppercase, letter-spacing.
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

### Categorical: Investment Score Band Colors
| Band | Color | Hex |
|---|---|---|
| High Potential (≥70) | Teal Green | `#00A699` |
| Moderate (50–69) | Warm Orange | `#FC642D` |
| Low Potential (<50) | Soft Grey | `#767676` |

### Categorical: Price Tier Colors
| Tier | Color | Hex |
|---|---|---|
| Budget (<$75) | Soft Grey | `#767676` |
| Mid ($75–150) | Rose Blush | `#FFB3B5` |
| Mid-High ($150–300) | Warm Orange | `#FC642D` |
| High ($300–600) | Airbnb Red | `#FF5A5F` |
| Luxury (>$600) | Deep Rose | `#E15759` |

---

## 3. Calculated Fields to Create in Tableau

Before building any visualizations, create these calculated fields in Tableau:

**1. Occupancy Rate %**
```tableau
[estimated_occupancy_l365d] / 365
```
*Format as: Percentage (0 decimal places)*

**2. Investment Score Band**
```tableau
IF [investment_potential] >= 70 THEN "High Potential"
ELSEIF [investment_potential] >= 50 THEN "Moderate"
ELSE "Low Potential" END
```
*Use as a Color dimension on the Investment Score bar chart*

**3. % of Market (Room Type)**
```tableau
COUNT([listing_id]) / TOTAL(COUNT([listing_id]))
```
*Format as: Percentage (1 decimal place) — use as label on the Treemap*

**4. Revenue per Listing (Neighbourhood Avg)**
```tableau
AVG([estimated_revenue_l365d])
```
*Used as the colour/size measure on the choropleth map*

---

## 4. Step-by-Step: Building KPI Tiles

Create four separate worksheets for the top banner KPIs.

### KPI 1: Total Active Listings
- **Drag** `listing_id` to Text.
- **Change Measure to:** Count.
- **Format:** Number (whole number, e.g., 30,048).
- **Label:** "Total Active Listings".

### KPI 2: Median Nightly Price
- **Drag** `price` to Text.
- **Change Measure to:** Median.
- **Format:** Currency (e.g., $150).
- **Label:** "Median Nightly Price".

### KPI 3: Market Avg Occupancy
- **Drag** `Occupancy Rate %` (calculated field) to Text.
- **Change Measure to:** Average.
- **Format:** Percentage (e.g., 20.3%).
- **Label:** "Market Avg Occupancy".

### KPI 4: Market Avg RevPAR
- **Drag** `revpar` to Text.
- **Change Measure to:** Average.
- **Format:** Currency (e.g., $101.41).
- **Label:** "Market Avg RevPAR".

---

## 5. Step-by-Step: Building the Charts

### Chart 1: Neighbourhood Revenue Heatmap (Filled Map / Choropleth)
*Goal: Show at a glance which areas of each city earn the most.*
- **Marks Card:** Change mark type to **Map**.
- **Detail:** `neighbourhood_cleansed`
- **Latitude / Longitude:** Drag to the Rows/Columns shelves (or let Tableau auto-detect).
- **Color:** `AVG(estimated_revenue_l365d)` — sequential palette from `#FFB3B5` (low) → `#FF5A5F` → `#E15759` (highest earners).
- **Tooltip:** Add `neighbourhood_cleansed`, `AVG(estimated_revenue_l365d)`, `AVG(revpar)`, `COUNT(listing_id)`.
- **Filter:** Add a City/Region quick filter if you want to isolate Amsterdam, Antwerp, Athens, or Bangkok.
- **Insight to show:** Bangkok dominates — Parthum Wan ($255,176 avg), Bang Rak ($198,377), Vadhana ($166,544) are the top 3 globally.

### Chart 2: Market Composition by Room Type (Treemap)
*Goal: Show where supply lives vs where the money is — are they the same?*
- **Marks Card:** Change mark type to **Square** (Treemap).
- **Size:** `COUNT(listing_id)` — bigger square = more listings.
- **Color:** `AVG(estimated_revenue_l365d)` — diverging: `#767676` (low/Shared) → `#FC642D` (mid) → `#FF5A5F` (high/Entire home).
- **Label:** `room_type` + `% of Market` + `AVG(estimated_revenue_l365d)`.
- **Insight to show:** Entire home/apt = 82.3% of supply and $38,907 avg revenue vs Private room = 16.2% at $28,541. Hotel room = 1.0% at $33,931. Shared room = 0.5% at only $6,521 — near zero market value.

### Chart 3: Investment Potential Score — Top 15 Neighbourhoods (Horizontal Bar)
*Goal: Give investors a rank-ordered shortlist of where to buy.*
- **Rows:** `neighbourhood_cleansed` — filter to **Top 15 by AVG(investment_potential)**, sorted descending.
- **Columns:** `investment_potential` (Average).
- **Color:** `Investment Score Band` — `#00A699` (High ≥70) · `#FC642D` (Moderate 50–69) · `#767676` (Low <50).
- **Reference Line:** Market avg at **57.56** — dashed, color `#767676`.
- **Labels:** Show the AVG score value on each bar.
- **Insight to show:** Schoonbroek-Rozemaai (81.85), Bijlmer-Centrum (69.28), Nieuwdreef (67.78), Koornbloem (66.84), Centrum-West (65.40) lead — a mix of Antwerp and Amsterdam neighbourhoods.

### Chart 4: Listing Distribution by Price Tier × Room Type (Stacked Bar)
*Goal: Reveal market gaps — underserved price + property type combinations.*
- **Columns:** `price_tier` (sort logically: Budget → Mid → Mid-High → High → Luxury).
- **Rows:** `listing_id` (Count).
- **Color:** `room_type`.
- **Labels:** Add count labels on each stack segment.
- **Insight to show:** Budget (<$75): 7,781 listings. Mid ($75–150): 7,294. Luxury (>$600): 7,372 — surprisingly large, skewed by Bangkok's high prices. High ($300–600): only 2,513 — a potential gap in that band.

---

## 6. Dashboard Assembly & Interactivity

### Recommended Layout Container Strategy
```text
+-----------------------------------------------------------------------+
|  [Filters: Room Type] [Neighbourhood] [Price Tier] [Superhost Toggle] |
+-----------------------------------------------------------------------+
|  [ KPI 1: 30,048 Listings ]  [ KPI 2: $150 Median Price ]            |
|  [ KPI 3: 20.3% Avg Occ  ]  [ KPI 4: $101.41 Avg RevPAR ]           |
+-----------------------------------------------------------------------+
|                                  |                                    |
|   Chart 1: Choropleth Map        |   Chart 2: Treemap                 |
|   (Neighbourhood Revenue)        |   (Room Type Composition)          |
|                                  |                                    |
+-----------------------------------------------------------------------+
|                                  |                                    |
|   Chart 3: Investment Score      |   Chart 4: Stacked Bar             |
|   Top 15 Neighbourhoods (Bar)    |   Price Tier × Room Type           |
|                                  |                                    |
+-----------------------------------------------------------------------+
```

### Dashboard Actions (Crucial for Interactivity)
1. **Filter Action (Neighbourhood — from Map):**
   - Source: Chart 1 (Choropleth Map)
   - Target: Chart 3 (Investment Score bar) and Chart 4 (Stacked bar).
   - Run on: Select
   - Clearing the selection: Show all values.
2. **Filter Action (Room Type — from Treemap):**
   - Source: Chart 2 (Treemap)
   - Target: All other charts on the dashboard.
   - Run on: Select.
3. **Filter Action (Price Tier — from Stacked Bar):**
   - Source: Chart 4 (Stacked Bar)
   - Target: Chart 1 (Map) and Chart 3 (Investment Score).
   - Run on: Select.

---

## 7. Key Business Insights (The "So What?")

When presenting this dashboard, these are the narrative talking points you deliver:

1. **Bangkok Neighbourhoods Dominate Revenue — By a Massive Margin:**
   *Insight:* The choropleth map immediately reveals geographic concentration. Parthum Wan ($255,176 avg revenue, $699 RevPAR) outperforms the entire Amsterdam and Antwerp markets on a per-listing basis. For investors purely optimising for revenue, Bangkok is the clear signal — though market entry costs and local regulations matter too.

2. **Entire Homes Are 82.3% of Supply AND Earn 36% More Than Private Rooms:**
   *Insight:* The Treemap proves this isn't a close contest. Entire home listings generate $38,907 avg vs $28,541 for private rooms. Shared rooms ($6,521) are essentially not a viable investment vehicle. Converting a private room property to an entire-home listing where possible is the highest-ROI structural change.

3. **27.1% of Listings Earn Zero Revenue — Market Entry Risk is Real:**
   *Insight:* The revenue tier breakdown shows 8,158 listings generating no revenue at all. Before investing, the heatmap and investment score chart reveal which neighbourhoods have the lowest proportion of zero-earners — the score already bakes this in via the Demand (40%) weighting.

4. **Investment Score Reveals Hidden Gems Beyond the Obvious Hotspots:**
   *Insight:* The top-15 investment score bar is surprising — Schoonbroek-Rozemaai (Antwerp, 81.85) and Bijlmer-Centrum (Amsterdam, 69.28) rank above many Bangkok neighbourhoods because they combine strong quality ratings with consistent demand and lower competition. High RevPAR ≠ high investment score if risk is high.

5. **The Luxury Tier (>$600) Has 7,372 Listings — But Most Are in Bangkok:**
   *Insight:* The stacked bar reveals the Luxury tier is the 2nd largest by listing count (7,372), but this is driven entirely by Bangkok's higher absolute price levels. Entering the Luxury tier in Amsterdam or Antwerp is a very different proposition — far lower supply, more discerning guests, and a much higher floor for property quality.
