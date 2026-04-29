# SmartStay Analytics — Complete Business Intelligence Report
### Section A | Group 5 | DVA Capstone 2 Project
**Dataset:** 30,048 cleaned Airbnb listings | Multi-City: Amsterdam · Antwerp · Athens · Bangkok

---

## 🧠 The Core Problem We Are Solving

### Root Cause: **Information Asymmetry in the Short-Term Rental Market**

> The Airbnb platform sits on top of all market data. Investors, hosts, and guests are all making expensive decisions with almost zero data — they guess. SmartStay Analytics removes that gap.

**Who suffers from this asymmetry?**

| Stakeholder | Their Expensive Decision | What They Used to Do |
|---|---|---|
| Real Estate Investor | "Which neighbourhood should I buy a property in?" | Gut feeling + property portals |
| Active Host | "What should I charge per night?" | Look at 2–3 nearby listings |
| Platform / Guest | "What actually makes a stay 5-star?" | Read subjective reviews |

**What we built:** A data pipeline + 3 Tableau dashboards that replace guessing with evidence.

---

---

# 📌 BUSINESS PROBLEM 1
## "WHERE Should I Invest? Which Neighbourhoods & Property Types Win?"

### Problem Name
**Investment Location & Property Type Optimization**

### Who Has This Problem
Real estate investors, property buyers, Airbnb expansion teams — anyone allocating capital into short-term rental properties across Amsterdam, Antwerp, Athens, and Bangkok.

### What They Are Trying to Decide
- Which specific neighbourhood gives the highest annual revenue?
- Is the market too saturated in popular areas to be profitable?
- Should I buy an entire home or convert a spare room?
- What is a realistic annual income from an Airbnb investment here?

### Why This Matters (Business Impact)
Choosing the wrong neighbourhood or property type directly costs money:
- Top Bangkok neighbourhoods (Parthum Wan avg $255,176/yr) vs bottom-tier markets = **$200,000+/year difference on the same investment**
- Buying the wrong property type in a market that rewards Entire Homes = permanently underperforming asset
- Entering a saturated market = low occupancy (27.1% of listings generate zero revenue), price wars, slow ROI

### What Our Analysis Proves
- **82.3% of the market** is Entire Homes/Apartments — they dominate both supply AND revenue ($38,907 avg vs $28,541 for Private rooms)
- **Top-earning neighbourhoods** are concentrated in Bangkok (Parthum Wan, Bang Rak, Vadhana) with RevPAR up to $699/night
- **Investment Potential Score (0–100)** combines Quality (40%) + Demand (40%) + Revenue (20%) — top score: 81.85 (Schoonbroek-Rozemaai)
- **37.1% Superhost penetration** signals a competitive but quality-driven market

### Columns Used From Data
| Column | Role |
|---|---|
| `neighbourhood_cleansed` | Geographic grouping for market comparison |
| `latitude` / `longitude` | Choropleth map plotting |
| `room_type` | Entire Home vs Private vs Hotel vs Shared |
| `property_type` | Condo, Apartment, House, Loft, etc. |
| `accommodates` | Capacity — drives price premium |
| `bedrooms` / `beds` / `bathrooms` | Physical specs — regression price drivers |
| `estimated_revenue_l365d` | Annual estimated earnings per listing |
| `estimated_occupancy_l365d` | Booked nights — demand signal |
| `price` | Nightly rate |
| `investment_potential` | Weighted composite score (0–100) |
| `revpar` | Revenue Per Available Room |

### Segment Columns Used for Filtering
| Column | Values |
|---|---|
| `price_tier` | Budget (<$75) / Mid ($75–150) / Mid-High ($150–300) / High ($300–600) / Luxury (>$600) |
| `revenue_tier` | No Revenue / Low (<$5k) / Mid ($5k–20k) / High ($20k–50k) / Top Earner (>$50k) |
| `availability_tier` | Very Low (0–30d) / Low / Medium / High / Very High |

---

### 📊 Dashboard 1: Market Overview & Investment Intelligence

**Audience:** Investors, Platform Managers, Executive Stakeholders

#### KPI Tiles (Top Banner — 4 Big Number Tiles)

| # | KPI Name | Column Used | Calculation | Format | Business Purpose |
|---|---|---|---|---|---|
| 1 | Total Active Listings | `listing_id` | `COUNT([listing_id])` | `30,048` | Market size — how competitive is it? |
| 2 | Median Nightly Price | `price` | `MEDIAN([price])` | `$150` | Pricing benchmark for new entrants |
| 3 | Market Avg Occupancy | `estimated_occupancy_l365d` | `AVG([estimated_occupancy_l365d])/365` | `20.3%` | Demand health of the overall market |
| 4 | Market Avg RevPAR | `revpar` | `AVG([revpar])` | `$101.41` | Combined pricing + demand efficiency (hotel industry standard) |

#### Charts

| # | Chart Name | Chart Type | X-axis / Dimension | Y-axis / Measure | Color | Business Question Answered |
|---|---|---|---|---|---|---|
| 1 | Neighbourhood Revenue Heatmap | **Filled Map (Choropleth)** | `latitude`, `longitude` | `AVG(estimated_revenue_l365d)` | Sequential green (light=low, dark=high) | "Which areas earn the most?" |
| 2 | Market Composition by Room Type | **Treemap** | `room_type` | `COUNT(listing_id)` (size) + `AVG(estimated_revenue_l365d)` (color) | Diverging by revenue | "Where is supply vs where is the money?" |
| 3 | Investment Potential Score — Top 15 Neighbourhoods | **Horizontal Bar Chart (sorted)** | `neighbourhood_cleansed` (top 15) | `AVG(investment_potential)` | `revenue_tier` segment | "Rank-ordered: where should I buy?" |
| 4 | Listing Distribution by Price Tier × Room Type | **Stacked Bar Chart** | `price_tier` | `COUNT(listing_id)` | `room_type` | "Where are the market gaps?" |

#### Filters Applied
- Room Type (multi-select)
- Neighbourhood (searchable dropdown)
- Price Tier (segment)
- Superhost Only toggle (`host_is_superhost`)

#### Layout
```
[ KPI: 30,048 Listings ] [ KPI: $150 Med Price ] [ KPI: 20.3% Avg Occ ] [ KPI: $101.41 RevPAR ]
[       Choropleth Map (neighbourhood revenue)       ] [ Treemap (room type) ]
[ Bar: Investment Score Top 15 ] [ Stacked Bar: Price Tier × Room Type ]
```

---

---

# 📌 BUSINESS PROBLEM 2
## "HOW Should I Price? What Actually Drives Revenue?"

### Problem Name
**Revenue Optimization & Pricing Strategy**

### Who Has This Problem
Active Airbnb hosts, prospective hosts about to list a property, property managers running multiple listings.

### What They Are Trying to Decide
- What is the optimal nightly price for my specific property and neighbourhood?
- Is charging more always better, or are there diminishing returns?
- Does enabling Instant Book genuinely increase my revenue?
- Is the effort of becoming a Superhost worth it in actual dollars?
- How does my revenue efficiency compare to competitors in my area?

### Why This Matters (Business Impact)
Pricing mistakes are the #1 silent revenue killer for hosts:
- **Under-pricing:** Charging $100/night when the market supports $150 = $5,000 lost per year on just 100 bookings
- **Over-pricing:** Setting $600/night in a $150 median market = low occupancy, near-zero revenue
- **Not knowing RevPAR:** Our dataset shows avg RevPAR of $101.41 but median of just $12.08 — the massive gap exposes how many hosts are dramatically underperforming
- **Ignoring Superhost:** Our data proves Superhosts earn **$58,810 avg vs $24,197 for non-Superhosts — a +143% revenue premium**

### What Our Analysis Proves
- **Median nightly price is $150** — but avg price is $865.90 driven by luxury outliers; most revenue sweet spot is Mid–Mid-High tiers
- **Only 13.5% of listings are Top Earners (>$50k/yr)** — the 90th percentile threshold is $86,000/year
- **Superhost status generates a +143% revenue premium** — the single most impactful host action
- **Instant Book listings** show higher occupancy (21.7% vs 18.7%) though non-IB listings have slightly higher avg revenue ($38,768 vs $35,388)
- **RevPAR varies dramatically by neighbourhood** — Parthum Wan ($699) vs market median ($12.08) = 57× difference proving location efficiency is paramount
- **27.1% of listings generate zero revenue** — critical insight for investors evaluating market risk

### Columns Used From Data
| Column | Role |
|---|---|
| `price` | Nightly rate — primary pricing KPI |
| `estimated_revenue_l365d` | Annual revenue outcome |
| `estimated_occupancy_l365d` | Demand/booking rate |
| `revpar` | Revenue Per Available Room = `(price × occupancy)/365` |
| `host_is_superhost` | Superhost flag (1=yes, 0=no) |
| `instant_bookable` | Instant Book flag (1=yes, 0=no) |
| `neighbourhood_cleansed` | Location benchmark grouping |
| `room_type` | Property category for comparisons |
| `accommodates` | Capacity — price regression driver |
| `bedrooms` | Bedroom count — regression driver |

### Segment Columns Used for Filtering
| Column | Values |
|---|---|
| `price_tier` | Budget (<$75) / Mid ($75–150) / Mid-High ($150–300) / High ($300–600) / Luxury (>$600) |
| `revenue_tier` | No Revenue / Low (<$5k) / Mid ($5k–20k) / High ($20k–50k) / Top Earner (>$50k) |
| `availability_tier` | Very Low → Very High |

---

### 📊 Dashboard 2: Revenue & Pricing Strategy

**Audience:** Current Hosts, Prospective Hosts, Property Managers

#### KPI Tiles (Top Banner — 5 Big Number Tiles)

| # | KPI Name | Column Used | Calculation | Format | Business Purpose |
|---|---|---|---|---|---|
| 1 | Avg Estimated Revenue | `estimated_revenue_l365d` | `AVG([estimated_revenue_l365d])` | `$37,016` | Realistic annual earnings benchmark |
| 2 | Median Occupancy Rate | `estimated_occupancy_l365d` | `MEDIAN([estimated_occupancy_l365d])/365` | `8.2%` | True market demand level (median, not skewed avg) |
| 3 | Avg RevPAR | `revpar` | `AVG([revpar])` | `$101.41` | Pricing efficiency — the professional metric |
| 4 | Top 10% Revenue Threshold | `estimated_revenue_l365d` | `PERCENTILE(0.90)` | `$86,000` | "What does it take to be a top earner?" |
| 5 | Superhost Revenue Premium | `estimated_revenue_l365d` + `host_is_superhost` | `(AVG(Superhost) - AVG(Non-Superhost)) / AVG(Non-Superhost)` | `+143%` | Quantified financial value of the Superhost badge |

#### Charts

| # | Chart Name | Chart Type | X-axis / Dimension | Y-axis / Measure | Extra | Business Question Answered |
|---|---|---|---|---|---|---|
| 1 | Price vs Revenue Sweet Spot | **Scatter Plot** | `price` | `estimated_revenue_l365d` | Color=`room_type`, Size=`estimated_occupancy_l365d`, Trend line | "What is the optimal price range — where does charging more stop helping?" |
| 2 | Revenue Distribution by Price Tier | **Box & Whisker Plot** | `price_tier` | `estimated_revenue_l365d` | Color=`price_tier` | "Does pricing Luxury = earning Luxury? (Exposes outliers and reality)" |
| 3 | RevPAR by Neighbourhood (Top 20) | **Horizontal Bar + Reference Line** | `neighbourhood_cleansed` | `AVG(revpar)` | Dotted line at `MEDIAN(revpar)` = $12.08 | "Which neighbourhoods have the most efficient pricing?" |
| 4 | Revenue + Occupancy by Room Type | **Dual-Axis Combo (Bar + Line)** | `room_type` | Bar=`AVG(estimated_revenue_l365d)`, Line=`AVG(occupancy%)` | Dual Y-axes | "Entire Home earns $38,907 at 20.6% occ vs Private room $28,541 at 19.5%" |
| 5 | Revenue Concentration Heatmap | **Highlight Table (Cross-tab)** | Columns=`revenue_tier` | Rows=Top 20 neighbourhoods | Color=COUNT of listings | "Which neighbourhoods have the most Top Earners concentrated?" |

#### Filters Applied
- Room Type (multi-select)
- Neighbourhood (dropdown)
- Price Range slider (min/max on `price`)
- Superhost (toggle)
- Availability Tier (segment filter)

#### Layout
```
[ KPI: $37,016 Avg Rev ] [ KPI: 8.2% Med Occ ] [ KPI: $101.41 RevPAR ] [ KPI: $86k 90th %ile ] [ KPI: +143% SH Premium ]
[ Scatter: Price vs Revenue (large, left) ] [ Box Plot: Revenue by Price Tier (right) ]
[ Bar: RevPAR by Neighbourhood ]           [ Dual-Axis: Revenue + Occupancy by Room Type ]
[ Heatmap Table: Revenue Tier × Neighbourhood (full width) ]
```

---

---

# 📌 BUSINESS PROBLEM 3
## "WHAT Makes a 5-Star Host? What Drives Guest Satisfaction?"

### Problem Name
**Host Performance Analysis & Guest Experience Optimization**

### Who Has This Problem
- Active hosts who receive 4-star reviews and don't know why they aren't getting 5-star
- Platform quality managers who need to identify underperforming hosts at scale
- Guests trying to identify genuinely high-quality listings before booking

### What They Are Trying to Decide
- Which specific sub-score (cleanliness, accuracy, communication, location, value, check-in) is pulling my overall rating down?
- Is the Superhost badge actually earned through better performance, or just gaming the system?
- Does enabling Instant Book increase satisfaction or reduce it?
- What is the relationship between review frequency (demand) and rating quality?
- Which hosts/listings are "Hidden Gems" — high quality but low visibility?

### Why This Matters (Business Impact)
Rating quality has a direct compounding financial effect:
- A listing going from 4.5 → 4.9 stars gets **better search placement** on Airbnb → more bookings → more revenue
- A host dropping below 4.8 can **lose Superhost status** → measurable revenue drop (−143% per our analysis)
- A platform with declining average ratings loses **guest trust** → bookings shift to competitors
- **71.3% of listings are Excellent or Perfect** — so sub-score differences in the 4.7–5.0 range have outsized competitive impact

### What Our Analysis Proves
- The market clusters heavily in **4.75–5.0 rating range** (71.3% of rated listings) — small sub-score differences determine ranking
- **Value (4.705) and Location (4.731)** are the lowest-scoring sub-categories — biggest improvement opportunity
- **Communication (4.866) and Check-in (4.855)** score highest — these are already market strengths
- **Superhost hosts are statistically better performers** with +143% revenue premium validated
- **Instant Book shows 21.7% vs 18.7% occupancy** — higher demand without rating degradation (IB rating: 4.74 vs Non-IB: 4.81)
- The **Quality × Demand quadrant** (scatter of rating vs reviews/month) reveals 4 strategic host archetypes: Stars, Hidden Gems, Volume Players, and Underperformers

### Columns Used From Data
| Column | Role |
|---|---|
| `review_scores_rating` | Overall guest satisfaction (primary quality KPI) |
| `review_scores_accuracy` | Sub-score: Did the listing match its description? (avg 4.805) |
| `review_scores_cleanliness` | Sub-score: Was it clean? (avg 4.766) |
| `review_scores_checkin` | Sub-score: Was the check-in smooth? (avg 4.855) |
| `review_scores_communication` | Sub-score: Did the host respond well? (avg 4.866 — highest) |
| `review_scores_location` | Sub-score: Was the location as described? (avg 4.731) |
| `review_scores_value` | Sub-score: Was it worth the price? (avg 4.705 — lowest) |
| `number_of_reviews` | Total historical reviews — credibility signal |
| `number_of_reviews_ltm` | Reviews in last 12 months — recency/demand signal |
| `reviews_per_month` | Booking frequency proxy — demand velocity (avg 1.39/mo) |
| `host_is_superhost` | Superhost status (1=yes, 0=no) — 37.0% of market |
| `instant_bookable` | Instant Book enabled flag — 51.8% of market |
| `host_acceptance_rate` | % of booking requests accepted — avg 88.8% |
| `estimated_revenue_l365d` | Revenue outcome linked to quality |
| `estimated_occupancy_l365d` | Demand outcome linked to quality |

### Segment Columns Used for Filtering
| Column | Values |
|---|---|
| `rating_category` | Poor (<4.0) [3.6%] / Good (4.0–4.5) [7.8%] / Very Good (4.5–4.75) [17.4%] / Excellent (4.75–4.9) [37.5%] / Perfect (4.9–5.0) [33.8%] |
| `price_tier` | Budget → Luxury (to compare quality by price segment) |

---

### 📊 Dashboard 3: Host Performance & Guest Experience

**Audience:** Active Hosts, Platform Quality Analysts, Guests

#### KPI Tiles (Top Banner — 5 Big Number Tiles)

| # | KPI Name | Column Used | Calculation | Format | Business Purpose |
|---|---|---|---|---|---|
| 1 | Market Avg Rating | `review_scores_rating` | `AVG([review_scores_rating])` | `4.77 ★` | Overall market quality baseline |
| 2 | % Superhosts | `host_is_superhost` | `SUM([host_is_superhost])/COUNT([listing_id])` | `37.0%` | Quality tier penetration in market |
| 3 | Avg Reviews Per Month | `reviews_per_month` | `AVG([reviews_per_month])` | `1.39` | Booking velocity / demand indicator |
| 4 | Instant Bookable % | `instant_bookable` | `SUM([instant_bookable])/COUNT([listing_id])` | `51.8%` | Market convenience standard |
| 5 | Avg Host Acceptance Rate | `host_acceptance_rate` | `AVG([host_acceptance_rate])` | `88.8%` | Host responsiveness / friction measure |

#### Charts

| # | Chart Name | Chart Type | X-axis / Dimension | Y-axis / Measure | Extra | Business Question Answered |
|---|---|---|---|---|---|---|
| 1 | Rating Distribution | **Histogram / Bar** | `rating_category` | `COUNT(listing_id)` | Color=`rating_category` (red→green), % labels | "71.3% are Excellent/Perfect — how do I differentiate in this compressed range?" |
| 2 | Superhost vs Non-Superhost Multi-KPI | **Side-by-Side Grouped Bar** | KPI type (Revenue, Rating, Occupancy, Reviews/Month) | Normalized values | Color: Superhost=teal, Non=grey | "$58,810 vs $24,197 avg revenue — what the Superhost badge delivers in real dollars" |
| 3 | Sub-Score Performance vs Market Avg | **Bullet Charts (6 sub-scores)** | Sub-score dimension | Score value | Target line = `AVG` per sub-score | "Value (4.705) and Location (4.731) are the drag — which to fix first?" |
| 4 | Quality × Demand Strategic Quadrant | **Scatter Plot with 4 Quadrants** | `reviews_per_month` (X = demand) | `review_scores_rating` (Y = quality) | Color=`rating_category`, Size=`estimated_revenue_l365d`, Reference lines at X=1.39 and Y=4.77 | "Am I a Star, Hidden Gem, Volume Player, or Underperformer?" |
| 5 | Top 15 Neighbourhoods by Avg Rating | **Horizontal Bar** | `neighbourhood_cleansed` | `AVG(review_scores_rating)` | Color=`pct_superhost` density | "Nieuw-Zuid (4.975), Hoboken-Zuidoost (4.935) — do quality clusters map to investment opportunities?" |
| 6 | Instant Book Impact on Revenue, Rating & Occupancy | **Grouped Bar** | Instant Book = Yes / No | Avg Revenue + Avg Rating + Avg Occupancy | Grouped by metric | "IB = +3% occupancy, slightly lower rating (4.74 vs 4.81) — worth the trade-off?" |

#### Strategic Quadrant Explanation (Chart 4)
| Quadrant | Meaning | Action |
|---|---|---|
| 🟢 Top-Right (High Quality + High Demand) | **Stars** — Best performing hosts | Maintain, scale up |
| 🔵 Top-Left (High Quality + Low Demand) | **Hidden Gems** — Great host, low visibility | Optimize title, photos, SEO |
| 🟡 Bottom-Right (Low Quality + High Demand) | **Volume Players** — Popular but risks complaints | Improve cleanliness, value urgently |
| 🔴 Bottom-Left (Low Quality + Low Demand) | **Underperformers** — Losing on all fronts | Full listing audit required |

#### Filters Applied
- Room Type (multi-select)
- Neighbourhood (dropdown)
- Superhost Status (Yes / No / All)
- Rating Category (segment filter)
- Instant Bookable (toggle)

#### Layout
```
[ KPI: 4.77★ Rating ] [ KPI: 37.0% Superhost ] [ KPI: 1.39 Rev/Mo ] [ KPI: 51.8% IB ] [ KPI: 88.8% Accept ]
[ Histogram: Rating Distribution ]   [ Side-by-Side: Superhost vs Non ]
[ Bullet Charts: 6 Sub-Scores vs Market Avg ]   [ Scatter: Quality × Demand Quadrant ]
[ Bar: Top Hoods by Rating ]  [ Grouped Bar: Instant Book Impact ]
```

---

---

# 🎨 Cross-Dashboard Design Standards

### Consistent Color Palette
| Use | Color | Hex |
|---|---|---|
| Primary Accent | Airbnb Red | `#FF5A5F` |
| Positive / High | Teal Green | `#00A699` |
| Warning / Mid | Warm Orange | `#FC642D` |
| Neutral / Low | Soft Grey | `#767676` |
| Background | Off-White | `#F7F7F7` |
| Text | Near-Black | `#484848` |

### Standard Tooltip Template (All Charts)
```
📍 [Neighbourhood / Room Type / Category]
💰 Avg Revenue: $X,XXX
⭐ Avg Rating: X.XX ★
🏠 Listings: X,XXX
📅 Avg Occupancy: XX%
🔢 RevPAR: $XX.XX
```

### Navigation
- Top nav bar with 3 buttons: `[Market Overview]` → `[Revenue & Pricing]` → `[Host Performance]`
- Filter actions: clicking on a chart element filters all other charts on the same dashboard

### Data Sources in Tableau
- **Primary:** `tableau_ready.csv` (30,048 listing-level rows — Amsterdam, Antwerp, Athens, Bangkok)
- **Secondary (joined):** `neighbourhood_summary.csv` joined on `neighbourhood_cleansed`
- **Reference:** `room_type_summary.csv` for room-type aggregated charts

### Calculated Fields to Build in Tableau
```
Occupancy Rate % = [estimated_occupancy_l365d] / 365

Superhost Revenue Premium % =
  (AVG(IF [host_is_superhost]=1 THEN [estimated_revenue_l365d] END)
   - AVG(IF [host_is_superhost]=0 THEN [estimated_revenue_l365d] END))
  / AVG(IF [host_is_superhost]=0 THEN [estimated_revenue_l365d] END)

Rating vs Market Avg =
  [review_scores_rating] - WINDOW_AVG(AVG([review_scores_rating]))

Investment Score Band =
  IF [investment_potential] >= 70 THEN "High Potential"
  ELSEIF [investment_potential] >= 50 THEN "Moderate"
  ELSE "Low Potential" END
```

---

---

# 📐 Complete Summary Table

| | Dashboard 1 | Dashboard 2 | Dashboard 3 |
|---|---|---|---|
| **Name** | Market Overview & Investment Intelligence | Revenue & Pricing Strategy | Host Performance & Guest Experience |
| **Primary User** | Investors, Executives | Hosts, Property Managers | Hosts, Platform QA, Guests |
| **Business Problem** | Information asymmetry in location & property selection | Pricing decisions made without market data | Quality gaps with no root-cause visibility |
| **Core Question** | Where should I invest? | How should I price? | What makes me 5-star? |
| **# of KPI Tiles** | 4 | 5 | 5 |
| **# of Charts** | 4 | 5 | 6 |
| **Key KPIs** | 30,048 Listings, $150 Median Price, 20.3% Avg Occ, $101.41 RevPAR | $37,016 Avg Rev, 8.2% Median Occ, $101.41 RevPAR, $86k 90th %ile, +143% SH Premium | 4.77★ Avg Rating, 37.0% Superhost, 1.39 Rev/Mo, 51.8% IB%, 88.8% Accept Rate |
| **Signature Chart** | Choropleth Revenue Map | Price vs Revenue Scatter (Sweet Spot) | Quality × Demand Quadrant Scatter |
| **Segment Filters** | price_tier, revenue_tier, availability_tier | price_tier, revenue_tier, availability_tier | rating_category, price_tier |
| **Data Source** | tableau_ready.csv | tableau_ready.csv | tableau_ready.csv |

---

# 🔑 The One Equation That Ties Everything Together

> **Revenue = f ( Location × Property Type × Host Quality )**
>
> - **Location** → Fixed after purchase → Dashboard 1 helps you pick right BEFORE buying (Parthum Wan earns $255k/yr vs bottom markets)
> - **Property Type** → Fixed after purchase → Dashboard 1 shows Entire Homes earn 36% more than Private rooms
> - **Host Quality** → 100% in your control → Dashboard 3 shows Superhosts earn 143% more, Value sub-score is the easiest win
> - **Pricing** → Adjustable anytime → Dashboard 2 shows median $150/night but 27.1% of hosts earn zero revenue by mispricing

---

# 🎤 One-Line Project Pitch

> *"SmartStay Analytics turns 30,048 Airbnb listings across Amsterdam, Antwerp, Athens, and Bangkok into a three-lens decision engine — helping investors find the right neighbourhood (Parthum Wan at $255k avg revenue), hosts find the right price ($101.41 avg RevPAR), and platforms identify what separates a 5-star experience from a 3-star complaint (Value at 4.705 is the weakest link)."*

---

*Built with late nights and data by SECTIONA_G5 | DVA Capstone 2*
*Notebooks: [EDA](../notebooks/03_eda.ipynb) · [Statistical Analysis](../notebooks/04_statistical_analysis.ipynb) · [Final Prep](../notebooks/05_final_load_prep.ipynb)*
