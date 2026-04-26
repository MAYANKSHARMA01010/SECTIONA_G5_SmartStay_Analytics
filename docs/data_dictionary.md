# 📖 SmartStay Analytics: The Rosetta Stone

Ever looked at a spreadsheet and felt like you were reading ancient hieroglyphs? This guide is here to help! We've translated 79 messy columns from Inside Airbnb into a lean, mean, business-fighting machine.

---

## 📂 Where’s the Data?
*   **The Raw Jungle:** `data/raw/airbnb_listing.csv` (The "Before" picture. 79 columns of chaos.)
*   **The Clean Machine:** `data/processed/tableau_ready.csv` (The "After" picture. 40 columns of pure business gold.)

---

## 💎 The Business KPIs (Our Secret Sauce)
We didn't just clean the data; we added "Smart" metrics to help investors make better decisions.

| KPI | What is it? | Business Logic |
| :--- | :--- | :--- |
| **💰 RevPAR** | Revenue Per Available Room | The industry standard for hotel performance. `(Price * Occupancy) / 365`. |
| **🚀 Investment Potential** | A score from 0-100 | A weighted score combining Quality (40%), Demand (40%), and Revenue (20%). |
| **📈 Revenue Tier** | Low, Mid, High, Top Earner | Segments listings based on annual earnings to find the "Sweet Spot." |
| **⭐ Rating Category** | Poor to Perfect | A human-readable version of the numerical review scores. |

---

## 🔍 Core Columns (The Clean List)

### 📍 Location & Vibes
*   **`neighbourhood_cleansed`**: The standardized neighborhood name (Crucial for Map charts!).
*   **`latitude` / `longitude`**: Exact GPS coordinates for detailed plotting.
*   **`property_type`**: Is it a Condo, a Loft, or a Castle?

### 💸 Money Matters
*   **`price`**: The nightly rate (Now a clean number, no more `$` or `,`).
*   **`estimated_revenue_l365d`**: Total estimated earnings over the last 12 months.
*   **`price_tier`**: Categorizes listings (Budget, Mid, Luxury) for segment analysis.

### 🏠 Property Specs
*   **`accommodates`**: How many people can sleep there.
*   **`bedrooms` / `beds` / `bathrooms`**: The physical specs that drive the price.
*   **`room_type`**: Entire home vs. Private room.

### 📅 Demand & Quality
*   **`estimated_occupancy_l365d`**: How many nights the place was likely booked.
*   **`reviews_per_month`**: Our proxy for booking frequency.
*   **`review_scores_rating`**: The overall customer satisfaction score.
*   **`host_is_superhost`**: 1 = High quality service, 0 = Regular host.

---

## 🗑️ The "Cutting Room Floor"
We threw away over 40 columns. Here’s why:
1.  **Technical Junk:** Columns like `scrape_id` and `listing_url` are for computers, not business analysts.
2.  **Privacy:** We removed host names and exact bios to keep things professional and focused on the real estate, not the person.
3.  **Fluff:** Long-winded descriptions (`neighborhood_overview`) were too messy for statistical analysis.

---

## 📊 Market Snapshot (Albany, NY)
*   **Current Inventory:** 32,585 listings.
*   **Market Price:** Median is **$147/night**.
*   **Top Earners:** The top 5% of hosts are pulling in over **$50,000/year**.
*   **Quality Standard:** The average host rating is a stellar **4.78/5.0**.

*Happy Dashboarding! 📊*
