# SmartStay Analytics — Naming & Typography Standards
### Section A | Group 5 | Applied to All 3 Dashboards

---

## 🔤 Typography System

Airbnb uses a proprietary font called **Cereal** — unavailable in Tableau. Use these substitutes that match the same clean, modern, humanist feel.

### Primary Font: **Tableau Book / Tableau Light**
> Tableau Desktop ships with its own "Tableau" font family — use it as your first choice since it renders perfectly in all Tableau views.

| Element | Font | Style | Size | Color |
|---|---|---|---|---|
| Dashboard Title | `Tableau Bold` | Bold, ALL CAPS | 20pt | `#484848` |
| Section / Sheet Title | `Tableau Medium` | Bold | 14pt | `#484848` |
| KPI Number (big value) | `Tableau Bold` | Bold | 28–36pt | `#FF5A5F` |
| KPI Label (below number) | `Tableau Book` | Regular, ALL CAPS | 10pt | `#767676` |
| Chart Axis Labels | `Tableau Book` | Regular | 10pt | `#767676` |
| Chart Axis Titles | `Tableau Medium` | Bold | 11pt | `#484848` |
| Data Labels on bars | `Tableau Book` | Regular | 9–10pt | `#484848` |
| Tooltip Text | `Tableau Book` | Regular | 10pt | `#F7F7F7` on dark bg |
| Reference Line Label | `Tableau Book` | Italic | 9pt | `#767676` |
| Filter Labels | `Tableau Book` | Regular | 10pt | `#484848` |
| Navigation Button Text | `Tableau Bold` | Bold | 11pt | `#FFFFFF` |

### Fallback Font (if Tableau font unavailable): **Trebuchet MS**
> Pre-installed on all systems, humanist sans-serif, closest free match to Cereal/Tableau.

---

## 📐 Naming Conventions

### Rule: Every title follows this format
```
[Action Verb / Question] — [Metric] by [Dimension]
```
Examples: *"Average Revenue by Neighbourhood"* · *"Listing Count by Price Tier"*

- **KPI Tile labels:** SHORT (2–4 words), ALL CAPS, no units in the label (put units in the number format)
- **Chart titles:** Sentence case, starts with the insight, not the chart type
- **Sheet names in Tableau:** Use the exact names below — they appear in the tab bar and tooltips

---

---

## 📊 Dashboard 1 — Market Overview & Investment Intelligence

### Dashboard Title (top of canvas)
```
SMARTSTAY ANALYTICS  ·  MARKET OVERVIEW & INVESTMENT INTELLIGENCE
```

### KPI Tile Names

| Tile # | Big Number Value | Label (shown below number) | Tooltip Subtext |
|---|---|---|---|
| KPI 1 | `30,048` | `ACTIVE LISTINGS` | Total listings in dataset across all cities |
| KPI 2 | `$150` | `MEDIAN NIGHTLY PRICE` | Middle price point across all listings |
| KPI 3 | `20.3%` | `AVG OCCUPANCY RATE` | Average nights booked ÷ 365 |
| KPI 4 | `$101.41` | `AVG REVPAR` | Revenue Per Available Room — industry efficiency metric |

### Chart Titles (Sheet Names in Tableau)

| Chart # | Sheet Name (Tab) | Chart Display Title | Subtitle / Caption |
|---|---|---|---|
| Chart 1 | `D1_Map_Revenue` | **Revenue by Neighbourhood** | Avg estimated annual revenue per listing |
| Chart 2 | `D1_Treemap_RoomType` | **Market Composition by Room Type** | Size = listing count · Colour = avg revenue |
| Chart 3 | `D1_Bar_InvestmentScore` | **Top 15 Neighbourhoods — Investment Score** | Weighted score: Quality 40% · Demand 40% · Revenue 20% |
| Chart 4 | `D1_StackedBar_PriceTier` | **Listing Distribution by Price Tier & Room Type** | Where are the market gaps? |

### Filter Labels (shown in the filter card header)
| Filter | Label to Display |
|---|---|
| `room_type` | `PROPERTY TYPE` |
| `neighbourhood_cleansed` | `NEIGHBOURHOOD` |
| `price_tier` | `PRICE TIER` |
| `host_is_superhost` | `SUPERHOST ONLY` |

---

---

## 📊 Dashboard 2 — Revenue & Pricing Strategy

### Dashboard Title (top of canvas)
```
SMARTSTAY ANALYTICS  ·  REVENUE & PRICING STRATEGY
```

### KPI Tile Names

| Tile # | Big Number Value | Label (shown below number) | Tooltip Subtext |
|---|---|---|---|
| KPI 1 | `$37,016` | `AVG ANNUAL REVENUE` | Average estimated revenue per listing over last 12 months |
| KPI 2 | `8.2%` | `MEDIAN OCCUPANCY RATE` | Median booked nights ÷ 365 (true market demand, not skewed by outliers) |
| KPI 3 | `$101.41` | `AVG REVPAR` | Revenue Per Available Room — pricing efficiency benchmark |
| KPI 4 | `$86,000` | `TOP 10% REVENUE` | 90th percentile threshold — what it takes to be a top earner |
| KPI 5 | `+143%` | `SUPERHOST PREMIUM` | Superhosts earn $58,810 vs $24,197 for regular hosts |

### Chart Titles (Sheet Names in Tableau)

| Chart # | Sheet Name (Tab) | Chart Display Title | Subtitle / Caption |
|---|---|---|---|
| Chart 1 | `D2_Scatter_PriceRevenue` | **Price vs Revenue — Finding the Sweet Spot** | Each dot = 1 listing · Size = occupancy · Colour = room type |
| Chart 2 | `D2_BoxPlot_RevenueTier` | **Revenue Distribution by Price Tier** | Does charging Luxury = earning Luxury? |
| Chart 3 | `D2_Bar_RevPAR` | **Top 20 Neighbourhoods by RevPAR** | Revenue efficiency — not just highest price, highest yield |
| Chart 4 | `D2_DualAxis_RoomType` | **Revenue & Occupancy by Room Type** | Bar = avg revenue (left axis) · Line = avg occupancy % (right axis) |
| Chart 5 | `D2_Heatmap_RevenueTier` | **Revenue Concentration by Neighbourhood** | Where are the Top Earners clustered? |

### Filter Labels
| Filter | Label to Display |
|---|---|
| `room_type` | `PROPERTY TYPE` |
| `neighbourhood_cleansed` | `NEIGHBOURHOOD` |
| `price` (slider) | `NIGHTLY PRICE RANGE` |
| `host_is_superhost` | `SUPERHOST ONLY` |
| `availability_tier` | `AVAILABILITY TIER` |

---

---

## 📊 Dashboard 3 — Host Performance & Guest Experience

### Dashboard Title (top of canvas)
```
SMARTSTAY ANALYTICS  ·  HOST PERFORMANCE & GUEST EXPERIENCE
```

### KPI Tile Names

| Tile # | Big Number Value | Label (shown below number) | Tooltip Subtext |
|---|---|---|---|
| KPI 1 | `4.77 ★` | `MARKET AVG RATING` | Average overall guest satisfaction score across all rated listings |
| KPI 2 | `37.0%` | `SUPERHOST SHARE` | Share of listings where host holds Superhost status |
| KPI 3 | `1.39` | `AVG REVIEWS / MONTH` | Booking velocity proxy — higher = more in-demand listing |
| KPI 4 | `51.8%` | `INSTANT BOOKABLE` | Share of listings with Instant Book enabled |
| KPI 5 | `88.8%` | `AVG ACCEPTANCE RATE` | Host request acceptance — lower = more friction for guests |

### Chart Titles (Sheet Names in Tableau)

| Chart # | Sheet Name (Tab) | Chart Display Title | Subtitle / Caption |
|---|---|---|---|
| Chart 1 | `D3_Bar_RatingDist` | **Rating Distribution Across the Market** | 71.3% of listings rated Excellent or Perfect — the bar is high |
| Chart 2 | `D3_GroupedBar_Superhost` | **Superhost vs Regular Host — Head to Head** | Revenue · Rating · Occupancy · Reviews per Month |
| Chart 3 | `D3_Bullet_SubScores` | **Sub-Score Diagnostic — Where Are You Losing Stars?** | Each bar vs market average · Value (4.705) is the weakest link |
| Chart 4 | `D3_Scatter_QualityDemand` | **Quality × Demand Strategic Quadrant** | Stars · Hidden Gems · Volume Players · Underperformers |
| Chart 5 | `D3_Bar_HoodRating` | **Top 15 Neighbourhoods by Average Rating** | Colour intensity = Superhost density in that area |
| Chart 6 | `D3_GroupedBar_InstantBook` | **Instant Book Impact — Revenue, Rating & Occupancy** | Is the occupancy gain worth the rating trade-off? |

### Filter Labels
| Filter | Label to Display |
|---|---|
| `room_type` | `PROPERTY TYPE` |
| `neighbourhood_cleansed` | `NEIGHBOURHOOD` |
| `host_is_superhost` | `SUPERHOST STATUS` |
| `rating_category` | `RATING CATEGORY` |
| `instant_bookable` | `INSTANT BOOK` |

---

---

## 🧭 Navigation Bar

Place a horizontal container at the very top of every dashboard with 3 buttons:

| Button Label | Links To | Active State |
|---|---|---|
| `MARKET OVERVIEW` | Dashboard 1 | Background `#E15759`, text white |
| `REVENUE & PRICING` | Dashboard 2 | Background `#E15759`, text white |
| `HOST PERFORMANCE` | Dashboard 3 | Background `#E15759`, text white |

**Inactive state:** Background `#FF5A5F`, text `#FFFFFF`  
**Active/selected state:** Background `#E15759` (darker red), text `#FFFFFF`  
**Font:** `Tableau Bold`, 11pt, ALL CAPS, letter spacing +1

---

## 📝 Tooltip Template (All Dashboards)

Use this exact format for every chart's tooltip. Edit via Marks Card → Tooltip:

```
📍 <Neighbourhood / Room Type / Category>
💰 Avg Revenue:  $<estimated_revenue_l365d (avg, currency)>
⭐ Avg Rating:   <review_scores_rating (avg, 2dp)> ★
🏠 Listings:     <listing_id (count, comma)>
📅 Avg Occupancy: <Occupancy Rate % (avg, %)>
🔢 RevPAR:       $<revpar (avg, 2dp)>
```

**Tooltip Formatting:**
- Background: `#484848` (Near-Black)
- Text: `#F7F7F7` (Off-White)
- Font: `Tableau Book`, 10pt
- Add a title line in `Tableau Bold` using the dimension name

---

## ✅ Quick Reference Cheat Sheet

### Fonts
| Where | Font | Size |
|---|---|---|
| Dashboard Title | Tableau Bold | 20pt |
| Chart Title | Tableau Medium | 14pt |
| KPI Number | Tableau Bold | 28–36pt |
| KPI Label | Tableau Book | 10pt (ALL CAPS) |
| Axis / Labels | Tableau Book | 9–10pt |
| Fallback (all) | Trebuchet MS | same sizes |

### Colors
| Purpose | Hex |
|---|---|
| Primary (bars, KPI numbers) | `#FF5A5F` |
| Positive / Superhost / Top | `#00A699` |
| Mid / Warning | `#FC642D` |
| Negative / Zero Revenue | `#E15759` |
| Light fill / Secondary | `#FFB3B5` |
| Reference lines / Non-primary | `#767676` |
| All text / Labels | `#484848` |
| Canvas background | `#F7F7F7` |
