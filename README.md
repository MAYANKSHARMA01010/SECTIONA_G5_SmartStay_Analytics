# SmartStay Analytics: Unlocking the Albany Airbnb Market

Hey there! Welcome to the **SmartStay Analytics** project. We are a team of students (Section A, Group 5) who spent the last few weeks diving deep into the Airbnb ecosystem of Albany, NY. 

Our goal was simple but ambitious: instead of just looking at numbers, we wanted to build a toolkit that actually helps people make decisions. Whether you're an investor looking to buy your next rental, a guest wondering what makes a "good" stay, or just curious about how Airbnb is growing in our city, we've got you covered.

### What are we trying to solve?
*   **For the Business-Minded:** We identify which neighborhoods are the "gold mines" and which property types actually bring in the most cash.
*   **For the Guests:** We look at what actually drives those 5-star ratings (is it just the price, or something else?).
*   **For the Platform:** We analyze growth trends and the real impact of being a "Superhost."

## 📂 Project Structure
- `data/`: Raw and processed datasets (CSV).
- `notebooks/`: Sequential analysis steps from extraction to final prep.
- `docs/`: Data dictionary and technical documentation.
- `scripts/`: Production-ready ETL pipeline.
- `tableau/`: Visualization workbooks and dashboards.
- `reports/`: Project outlines and final reports.

## 🚀 Analysis Workflow
1.  **Extraction:** Loading raw scrape data from Inside Airbnb.
2.  **Cleaning:** 3-pass cleaning process (dropping metadata, handling missing values, currency formatting).
3.  **EDA:** Multi-perspective visual exploration of pricing, demand, and quality.
4.  **Statistical Analysis:** Hypothesis testing (Superhost impact, Room type ANOVA) and Regression modeling.
5.  **Final Load & Prep:** Generating Tableau-optimized KPIs and summary aggregates.

## 🛠️ Installation & Setup
For detailed setup instructions, please refer to the following guides:

1.  **[How to Clone this Repository](docs/cloning_guide.md)**
2.  **[How to Setup Virtual Environment (.venv)](docs/setup_venv.md)**
3.  **[Business & Investment Insights (Market Analysis)](docs/BUSINESS_INSIGHTS.md)**

Quick Start:
```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
python3 scripts/etl_pipeline.py
```

## 📊 Business KPIs (Ready for Tableau)
We have prepared a `tableau_ready.csv` that includes advanced business metrics:
*   **RevPAR:** Revenue Per Available Room (Standard industry benchmark).
*   **Investment Potential:** A weighted score (0-100) to identify top property opportunities.
*   **Market Segmentation:** Categorized price, revenue, and quality tiers.

---
*Built with some late nights and a lot of coffee by Group 5 for our DVA Capstone 2 Project.*