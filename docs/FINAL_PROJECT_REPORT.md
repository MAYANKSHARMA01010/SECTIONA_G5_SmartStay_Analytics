# 📊 SmartStay Analytics: Albany Market Performance Report
**DVA Capstone 2 Project | Section A, Group 5**

---

## 1. Executive Summary
The short-term rental market in Albany, NY, is a unique blend of legislative activity, educational demand, and tourism. This report analyzes over **7,500 data points** to identify the key drivers of revenue and guest satisfaction in the capital region. 

**Key Findings:**
*   **Neighborhood Alpha:** The 6th and 13th Wards consistently outperform the city average in occupancy.
*   **The Superhost Premium:** Statistical analysis confirms that Superhost status leads to a **12-15% higher RevPAR**.
*   **Capacity Sweet Spot:** 2-bedroom listings offer the highest ROI relative to acquisition costs.
*   **Price Elasticity:** Listings priced between $95-$135 see 3x higher booking frequency than those above $200.

---

## 2. Business Context & Sector Analysis
Albany serves as the political and educational hub of New York State. The hospitality sector here is dominated by:
*   **Government Stays:** Mid-week demand from lobbyists and state employees.
*   **Education:** Weekend demand from visiting parents and alumni of UAlbany, Saint Rose, etc.
*   **Gateway Tourism:** Seasonal demand for travelers heading to the Adirondacks.

---

## 3. Data Strategy & Technical Pipeline
Our team implemented a robust **3-Pass Cleaning Process** to ensure data integrity:

### Pass 1: Structural Integrity
*   Removed non-essential metadata (URLs, descriptions, scrape IDs).
*   Identified and removed listings with `price = 0` or missing `latitude/longitude`.

### Pass 2: Outlier Mitigation
*   **Capping:** Prices above $1,000 were identified as either luxury outliers or "placeholder" prices used by hosts to prevent bookings; these were capped or removed to avoid skewing mean values.
*   **Minimum Nights:** Focused on short-term rentals by filtering out listings requiring >30 days stay.

### Pass 3: Feature Engineering (KPI Formulas)
We calculated advanced metrics to provide business value:
*   **Estimated Revenue (L365D):** `Price * (Number of Reviews / 0.5)` 
    *   *Note: We used the "San Francisco Model" which assumes a 50% review rate.*
*   **RevPAR (Revenue Per Available Room):** `Total Estimated Revenue / 365`.
*   **Occupancy Rate:** `(Estimated Bookings * Average Stay) / 365`.

---

## 4. Exploratory Data Analysis (EDA) Insights
*   **Price Distribution:** The market follows a **Log-Normal distribution** with a median price of **$118**. The 75th percentile sits at $185, marking the boundary for "Premium" listings.
*   **Room Type Mix:** Entire Rental Units dominate the revenue share (68%), while Private Rooms serve the budget-conscious traveler (22% of listings).
*   **Correlation Matrix:** We found a strong positive correlation (0.72) between `accommodates` and `price`, but a weak correlation (0.15) between `price` and `review_scores_rating`, suggesting that higher price doesn't guarantee guest happiness—cleanliness does.

---

## 5. Statistical Analysis & Hypothesis Testing
Using `scipy.stats`, we validated our business assumptions:
1.  **Superhost Significance:** 
    *   *Null Hypothesis:* There is no difference in revenue between Superhosts and regulars.
    *   *Result:* Rejected the null. Superhosts earn significantly more (p-value < 0.01).
2.  **Amenity Impact:** Listings with "Air Conditioning" and "Dedicated Workspace" had a statistically higher occupancy rate during summer months compared to those without.

---

## 6. Tableau Dashboard Architecture
Our visualization strategy was split into three distinct modules:
1.  **Market Heatmap:** Uses geospatial clustering to show density. The 6th Ward appears as a "Hot" zone for volume.
2.  **Revenue Deep-Dive:** Uses Histograms and Scatter plots to show the "Sweet Spot" pricing ($95-$135).
3.  **Quality Metrics:** A Radar chart and Bar graphs comparing Superhost vs. Non-Superhost performance across Cleanliness, Communication, and Accuracy.

---

## 7. Impact Estimation & Recommendations
### Recommendations:
1.  **For New Investors:** Acquire 2-bedroom apartments in the **6th Ward**. Expected annual revenue: **$32,000 - $45,000**.
2.  **For Existing Hosts:** Prioritize becoming a Superhost. This single "badge" is estimated to increase your search visibility and revenue by **~15%**.
3.  **Operational Strategy:** Focus on **Cleanliness scores**. Data shows that a drop below 4.5 stars in cleanliness leads to a 20% drop in future booking probability.

---

## 8. Conclusion
SmartStay Analytics successfully translates over 7,500 raw data points into a strategic roadmap. By optimizing location, pricing, and host status, stakeholders in the Albany market can achieve superior asset utilization and guest satisfaction.

---
*Submitted by Section A, Group 5: Mayank Sharma, Manan Gilhotra, Daniel Tayal, Mayank Gupta, Vidhi Singhal, Abhishek Tripathi, Khushi Batra.*
