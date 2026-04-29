# 📖 Data Dictionary & Technical Specifications

This document provides a comprehensive breakdown of every field used in the SmartStay Analytics project, including our data cleaning logic and KPI formulas.

---

## 1. Core Listing Attributes
| Field Name | Type | Description | Cleaning Notes |
| :--- | :--- | :--- | :--- |
| `id` | Integer | Unique identifier for the listing. | Verified for uniqueness; no duplicates allowed. |
| `neighbourhood_cleansed` | String | The specific Albany Ward (e.g., 6th Ward). | Normalized to uppercase for consistency. |
| `property_type` | String | Specific type of home (e.g., Entire rental unit). | Standardized naming conventions. |
| `room_type` | String | Category (Entire home/apt, Private room). | Used for market segmentation. |
| `accommodates` | Integer | Max number of guests. | Imputed with median if missing. |
| `bedrooms` | Float | Number of bedrooms. | Imputed with median if missing. |
| `price` | Float | Nightly rate in USD. | Removed currency symbols; capped at $1,000. |

---

## 2. Calculated Metrics (KPIs)
These fields were generated during the ETL process to provide business intelligence.

### **Estimated Revenue (L365D)**
*   **Formula:** `price * (number_of_reviews / 0.5) * average_length_of_stay`
*   **Logic:** Uses the "San Francisco Model" assuming only 50% of guests leave reviews. 
*   **Use Case:** Estimating annual ROI for a property.

### **RevPAR (Revenue Per Available Room)**
*   **Formula:** `estimated_revenue_l365d / 365`
*   **Use Case:** The gold standard for hospitality performance comparison.

### **Occupancy Rate**
*   **Formula:** `(estimated_bookings * 3) / 365` 
*   *Note: Assumes an average stay of 3 nights per booking.*

---

## 3. Quality & Performance Metrics
| Field Name | Description |
| :--- | :--- |
| `host_is_superhost` | Binary indicator of elite host status. |
| `review_scores_rating` | Weighted average of guest satisfaction (0-5 stars). |
| `review_scores_cleanliness` | Specific score for property hygiene. |
| `host_response_time` | Categorical (within an hour, within a few hours, etc.). |

---

## 4. ETL & Cleaning Log
*   **Outliers:** Removed 124 listings with prices > $1,000 (potential fraud or error).
*   **Incompletes:** Dropped 45 listings with no `neighbourhood` or `latitude` data.
*   **Normalization:** All boolean values (`t`/`f`) converted to integer (1/0) for Tableau compatibility.

---
*Built for the DVA Capstone 2 Project — Group 5*
