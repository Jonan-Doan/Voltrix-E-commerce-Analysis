# Voltrix Ecommerce Performance Report
</div>

## Client Background

  
Founded in 2018, **Voltrix** is an e-commerce company that sells popular consumer electronics and has since expanded to a global customer base. The company sells products through its website and mobile app, leveraging a variety of marketing channels to reach customers, including email campaigns, SEO, and affiliate links. Over the last few years, its most popular product lines have come from Apple, Samsung, and ThinkPad.

Voltrix serves **88,000 customers** across North America, EMEA, APAC, and LATAM, recording **over 108,000 transactions** and generating over **$24 million** in total sales revenue.

In partnership with Angie, Voltrix’s Head of Operations, this in-depth analysis evaluates performance from 2019 to 2022. This comprehensive review highlights critical pain points across sales trends, product performance, regional demand, and loyalty program engagement, offering stakeholders actionable insights to optimize market positioning and drive commercial growth. The resulting insights and strategic recommendations target four core pillars:


* **Revenue Trends & Growth Rate:** Track Voltrix's sales trajectory, monthly and yearly growth rates, and average order value (AOV) from 2019–2022.
* **Loyalty Program Impact:** Evaluate whether Voltrix's loyalty program is performing well enough to justify continued investment.
* **Refund Rates & AOV:** Understand refund rate trends and how they relate to average order value.
* **Regional & Product Performance:** Identify which products and regions are driving sales (and refunds), as a starting point for deeper analysis.


---
  
## Dataset Structure and ERD (Entity relationship diagram)

The underlying relational database consists of four tables (`orders`, `customers`, `order_status`, and `geo_lookup`) joined on customer and order IDs:

<div align="center">
<img width="685" height="402" alt="image" src="https://github.com/user-attachments/assets/11adcf7b-0059-4b46-aaa3-458fe4e52cce" />
</div>

For analysis, these were flattened into a single order-line-level table, `orders_data_cleaned`:

<div align="center">
  
| | |
|---|---|
| Rows | 108,127 |
| Columns | 23 |
| Date Range | 2019 – 2022 |
| Grain | One row per product within an order |
</div>

**Data note:** Despite the "_cleaned" naming, the flattened extract still contained duplicate rows, corrupted identifiers, and missing region values that had to be identified and corrected for — see [Data Quality & Limitations](#data-quality--limitations).

## Tech Stack & Tools
* **Data Visualization:** Tableau Public (calculated fields, parameters, LOD expressions, dashboard actions)
* **Data Cleaning & Preprocessing:** Excel (initial cleaning pass before loading into Tableau)
* **Data Querying & Exploratory Analysis:** BigQuery (SQL)
* **Documentation:** GitHub / Markdown
  
## Executive Summary

Between 2019 and 2022, Voltrix generated **$24,045,399** in Sales across **92,930 orders**, for an overall AOV of **$258.75**.

* **Macro Trajectory:** Sharp, pandemic-era growth peaked in mid-2020 (+50.3% YoY in a single month), followed by a sustained decline through 2021 into 2022.
* **2022 Snapshot:** Sales fell to **$4.36M** (-44.1% YoY), Orders to **18,966** (-38.3% YoY), and AOV to **$229.80** (-9.4% YoY) — the steepest year of decline in the dataset.
* **Refund Tracking Gap:** Recorded refund rates fall to a flat 0% by late 2021 — a data-logging cutoff, not a real operational win. Within the reliable tracking window, the true refund rate is **7.59%**.
* **Loyalty Members Refund More:** Loyalty program members show a meaningfully higher refund rate than non-members (10.80% vs. 5.71%), a signal worth investigating alongside the program's strong adoption growth.
* **Data Quality:** ~56K orders (over half the dataset) were missing a Region value, inferred as North America based on currency (99.13% USD/CAD).


## Overall Sales Trends

Voltrix's Sales, Orders, and AOV all peaked in 2020 **(pandemic-era surge)** and have declined every year since, showing a dramatic arc rather than steady growth.
* Monthly Sales peaked at **$883,406**, then declined to **$166,337** by the close of the period
* Monthly Orders peaked at **3,065** , then declined to **719** by the close of the period.
* Monthly AOV peaked at **$320.05**, then declined to **$216.99** by the close of the period

<div align = "center">

<img width="1147" height="402" alt="image" src="https://github.com/user-attachments/assets/52935810-c287-4373-9d2e-05e2e5f7b2f3" />

</div>

That decline carried into 2022, Voltrix's weakest year on record.

* Sales fell to **$4.36M (-44.1% YoY)**
* AOV slipped only modestly to **$229.80 (-9.4% YoY)**
* Orders dropped to **18,966 (-38.3% YoY)**

<div align = "center">
  
<img width="1152" height="93" alt="image" src="https://github.com/user-attachments/assets/7341f288-6d19-47f6-b945-c03f8979c12e" />

</div>

Because AOV held up far better than Sales or Orders, the 2022 decline was driven primarily by **fewer orders** rather than smaller ones—a distinction worth noting, since it points to a demand or retention problem rather than a pricing one. See [Monthly & Yearly Growth Rates](#monthly--yearly-growth-rates) for the detailed year-by-year and month-by-month breakdown.


## Monthly & Yearly Growth Rates

**TL;DR:** 2020 was explosive growth, 2021 cooled off, and 2022 fell sharply — and the monthly view reveals real volatility that the yearly numbers smooth over.

### Yearly Growth Rate (YoY)

| Year | Sales YoY | AOV YoY | What's Happening |
|---|---|---|---|
| 2020 | +164.6% | +30.88% | Pandemic-era demand surge — growth driven by order volume, not bigger baskets |
| 2021 | -9.6% | -14.94% | Growth reverses off 2020's high base |
| 2022 | -44.1% | -9.44% | Steepest decline — driven by fewer orders, not a falling AOV |

<div align = "center">
  
<img width="800" height="390" alt="image" src="https://github.com/user-attachments/assets/57514196-3de0-4e33-9e3d-d067c289a667" />


</div>

### Monthly Volatility

| Metric | Best Month (YoY) | Worst Month (YoY) |
|---|---|---|
| Sales | +50.32% (mid-2020) | -34.50% (late 2022) |
| AOV | +17.86% (late 2022) | -16.45% (late 2022) |

AOV's best *and* worst months both land in late 2022, within a few months of each other, a sign of real volatility in a shrinking order base, not a steady trend in either direction.


## Loyalty Program Performance & Recommendation

**TL;DR:** Loyalty adoption grew from near-zero to overtaking Non-Loyalty customers by 2021 (a real growth story), but members refund at nearly double the rate, concentrated in one product.

Loyalty started from almost nothing in 2019 (30 orders and $4,728 in Sales) and grew steadily enough to **overtake Non-Loyalty customers** in both order volume and total Sales by 2021, a strong trajectory for a program that young.

<div align = 'center'>
  
<img width="1146" height="228" alt="image" src="https://github.com/user-attachments/assets/fcd91419-ff61-4573-9dc7-13ea678690ac" />
</div>


That growth comes with a trade-off in basket composition. Loyalty's purchases are unusually concentrated in one product **(Apple AirPods)**, which make up 58.17% of all Loyalty orders, while Non-Loyalty's basket is split more evenly across three: AirPods (34.98%), Samsung Charging Cable Pack (31.32%), and a 27in 4K gaming monitor (22.58%).

<div align = 'center'>
<img width="578" height="195" alt="image" src="https://github.com/user-attachments/assets/81aa112d-abc5-4eb6-b80f-49d5f3be7f13" />

<img width="1155" height="200" alt="image" src="https://github.com/user-attachments/assets/23325438-3bfc-4285-9c52-2c894ded12dc" />

</div>

The refund-count trend tells the same story the rate does, with one caveat: Loyalty refunds climbed sharply through 2021 (peaking at 254 in a single month) before falling back to near-zero by the end of the dataset. That drop **isn't a real recovery**. It lines up with the same refund-tracking cutoff noted in [Data Quality & Limitations](#data-quality--limitations), so the reliable comparison window for this metric is 2019–2021, not the full period.

**Verdict: Keep investing in the loyalty program.** Its adoption curve is a real growth story, overtaking Non-Loyalty customers in both order volume and total sales within about two years of launching from a near-zero base. That said, the elevated refund rate among members is worth investigating before scaling further, especially given how concentrated their basket is in a single product (AirPods).

## Refund Rates & AOV

## Region & Product Mix

### Product Mix

### Region Performance

## Data Quality & Limitations

## Strategic Recommendations

## Dashboard Preview

🔗 [View the full interactive dashboard on Tableau Public](https://public.tableau.com/app/profile/jonan.doan/viz/VoltrixsE-commerceTrendAnalysis/VoltrixsE-CommercePerformanceOverview)

The workbook is organized into four dashboards, connected by a persistent navigation bar:

1. **E-Commerce Performance Overview** — the landing page. Headline KPIs (2022 Sales, AOV, Orders with YoY change) with trend lines underneath, a Loyalty Program / Purchase Platform / Marketing Channel / Region breakdown selector, and click-through access to a Growth Rate view for each KPI.
2. **Deep Dive: Product Mix** — a 100%-stacked view of Sales/Orders share by product, plus purchase-platform and loyalty breakdowns.
3. **Deep Dive: Refund Rate** — refund rate overall and broken down by loyalty status, purchase platform, marketing channel, and product, with a visible callout for the refund-tracking data gap.
4. **Deep Dive: Region** — Sales/AOV/Orders and refund rate broken down by geographic region, including a transparent note on how orders with missing region data were handled.

## Repo Contents

| File | Description |
|---|---|
| `README.md` | This file |
| `Elist_Tableau_Data_Sourced.xlsx` | Raw source data |
| SQL script | *Coming soon* — BigQuery queries used for exploratory analysis |
| ER diagram | *Coming soon* |
| Data cleaning file (Excel) | *Coming soon* |

---

*This project uses a practice dataset and a fictionalized company/stakeholder scenario for portfolio purposes.*
