# 📖 SmartStay Analytics: The Rosetta Stone

Ever looked at a spreadsheet and felt like you were reading ancient hieroglyphs? This guide is here to help! We've translated raw Airbnb data into a lean, mean, business-fighting machine — spanning **Amsterdam, Antwerp, Athens, and Bangkok**.

---

## 📂 Where's the Data?
*   **The Raw Jungle:** `data/raw/airbnb_listing.csv` (The "Before" picture — 79 columns of chaos.)
*   **The Clean Machine:** `data/processed/tableau_ready.csv` (The "After" picture — 34 columns of pure business gold, **30,048 listings**.)

---

## 💎 The Business KPIs (Our Secret Sauce)
We didn't just clean the data; we added "Smart" metrics to help investors make better decisions.

| KPI | What is it? | Business Logic |
| :--- | :--- | :--- |
| **💰 RevPAR** | Revenue Per Available Room | The industry standard for hotel performance. `(Price × Occupancy) / 365`. Market avg: **$101.41** |
| **🚀 Investment Potential** | A score from 0–100 | A weighted score combining Quality (40%), Demand (40%), and Revenue (20%). Market avg: **57.56** |
| **📈 Revenue Tier** | No Revenue / Low / Mid / High / Top Earner | Segments listings by annual earnings. **27.1%** earn zero; only **13.5%** are Top Earners (>$50k). |
| **⭐ Rating Category** | Poor to Perfect | Human-readable version of numerical review scores. **71.3%** of rated listings are Excellent or Perfect. |
| **🏷️ Price Tier** | Budget / Mid / Mid-High / High / Luxury | Segments listings by nightly rate for pricing strategy analysis. |
| **📅 Availability Tier** | Very Low → Very High | Segments by days available per year — proxy for demand and host strategy. |

---

## 🔍 Core Columns (The Clean List)

### 📍 Location & Vibes
*   **`neighbourhood_cleansed`**: Standardized neighbourhood name across 169 unique areas in 4 cities.
*   **`latitude` / `longitude`**: Exact GPS coordinates for map/choropleth charts.
*   **`property_type`**: Is it a Condo, a Loft, a Houseboat, or a Hotel room?
*   **`room_type`**: Entire home/apt (82.3%) · Private room (16.2%) · Hotel room (1.0%) · Shared room (0.5%).

### 💸 Money Matters
*   **`price`**: The nightly rate (clean number, no `$` or `,`). Median: **$150** | Avg: $865.90 (skewed by luxury).
*   **`estimated_revenue_l365d`**: Total estimated earnings over the last 12 months. Avg: **$37,016** | Median: $4,410.
*   **`estimated_occupancy_l365d`**: Booked nights in the last 365 days. Avg: **73.9 nights (20.3% occupancy)**.
*   **`revpar`**: Revenue Per Available Room. Avg: **$101.41** | Median: $12.08.
*   **`price_tier`**: Budget (<$75) · Mid ($75–150) · Mid-High ($150–300) · High ($300–600) · Luxury (>$600).
*   **`revenue_tier`**: No Revenue (27.1%) · Low <$5k (24.8%) · Mid $5k–20k (24.1%) · High $20k–50k (10.4%) · Top Earner >$50k (13.5%).
*   **`investment_potential`**: Weighted composite score 0–100. Avg: 57.56. Top neighbourhood (Schoonbroek-Rozemaai): 81.85.

### 🏠 Property Specs
*   **`accommodates`**: How many guests can sleep there — key price driver.
*   **`bedrooms` / `beds` / `bathrooms`**: Physical specs that drive price regression.
*   **`minimum_nights` / `maximum_nights`**: Booking window constraints.
*   **`availability_365`**: Days per year the listing is open for booking.
*   **`availability_tier`**: Very Low (0–30d) · Low · Medium · High · Very High.

### ⭐ Review & Quality Scores
*   **`review_scores_rating`**: Overall satisfaction. Market avg: **4.77 / 5.0**.
*   **`review_scores_accuracy`**: Listing matched its description. Avg: **4.805** ← 2nd best.
*   **`review_scores_cleanliness`**: Was it clean? Avg: **4.766**.
*   **`review_scores_checkin`**: Was check-in smooth? Avg: **4.855** ← 2nd highest.
*   **`review_scores_communication`**: Did the host respond well? Avg: **4.866** ← highest sub-score.
*   **`review_scores_location`**: Was the location as described? Avg: **4.731** ← 2nd lowest.
*   **`review_scores_value`**: Was it worth the price? Avg: **4.705** ← lowest sub-score (biggest opportunity).
*   **`rating_category`**: Poor (<4.0) [3.6%] · Good (4.0–4.5) [7.8%] · Very Good (4.5–4.75) [17.4%] · Excellent (4.75–4.9) [37.5%] · Perfect (4.9–5.0) [33.8%].

### 👤 Host Behaviour
*   **`host_is_superhost`**: 1 = Superhost (37.0% of market), 0 = Regular host. Superhosts earn **+143% more revenue** ($58,810 vs $24,197).
*   **`host_acceptance_rate`**: % of booking requests accepted. Market avg: **88.8%**.
*   **`instant_bookable`**: 1 = Instant Book enabled (51.8% of market). IB listings: 21.7% occ vs 18.7% non-IB.

### 📊 Review Velocity
*   **`number_of_reviews`**: Lifetime total reviews — credibility signal.
*   **`number_of_reviews_ltm`**: Reviews in last 12 months — recency demand signal.
*   **`reviews_per_month`**: Booking frequency proxy. Market avg: **1.39/month**.

---

## 🗑️ The "Cutting Room Floor"
We trimmed down to 34 business-relevant columns. Removed:
1.  **Technical Junk:** `scrape_id`, `listing_url`, `last_scraped` — for computers, not business analysts.
2.  **Privacy:** Host names, bios, and exact addresses removed for professional focus.
3.  **Fluff:** Long-text columns (`description`, `neighborhood_overview`) — too messy for statistical analysis.
4.  **Redundant:** Columns with >60% null values or zero variance after cleaning.

---

## 📊 Market Snapshot (Multi-City: Amsterdam · Antwerp · Athens · Bangkok)

| Metric | Value |
| :--- | :--- |
| **Total Listings** | **30,048** across 169 neighbourhoods |
| **Cities Covered** | Amsterdam (NL) · Antwerp (BE) · Athens (GR) · Bangkok (TH) |
| **Median Nightly Price** | **$150/night** |
| **Avg Annual Revenue** | **$37,016/listing** |
| **Avg Occupancy Rate** | **20.3%** (73.9 nights/year) |
| **Avg RevPAR** | **$101.41** |
| **Avg Rating** | **4.77 / 5.0** |
| **Superhost Share** | **37.0%** of listings |
| **Instant Bookable** | **51.8%** of listings |
| **Top Earner Threshold (90th %ile)** | **$86,000/year** |
| **Highest Avg Revenue Neighbourhood** | **Parthum Wan (Bangkok): $255,176/yr** |
| **Highest Avg Investment Score** | **Schoonbroek-Rozemaai (Antwerp): 81.85/100** |
| **Superhost Revenue Premium** | **+143%** ($58,810 vs $24,197) |
| **Zero Revenue Listings** | **27.1%** (8,158 listings) |

*Happy Dashboarding! 📊*

---
*Last updated: April 2026 | SECTIONA_G5 | DVA Capstone 2*
