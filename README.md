# Voltrix: E-Commerce Revenue, Growth & Loyalty Analytics

A Tableau analytics project analyzing sales performance, YoY/MoM revenue growth, loyalty program effectiveness, and refund trends for Voltrix, a global consumer electronics retailer.

🔗 [View the live dashboard on Tableau Public](https://public.tableau.com/app/profile/jonan.doan/viz/VoltrixsE-commerceTrendAnalysis/VoltrixsE-CommercePerformanceOverview)

---

## Voltrix Data

The underlying relational database consists of four tables — `orders`, `customers`, `order_status`, and `geo_lookup` — joined on customer and order IDs:

![ER diagram showing orders, customers, order_status, and geo_lookup tables](assets/erd.png)
<!-- TODO: add erd.png to an assets/ folder in the repo -->

For analysis, these were flattened into a single order-line-level table, `orders_data_cleaned`, with **108,127 rows** and **23 columns**, spanning **2019–2022**. Each row represents one product within an order, so a multi-item order spans multiple rows.

**Data note:** despite the "_cleaned" naming, the flattened extract still contained duplicate rows, corrupted identifiers, and missing region values that had to be identified and corrected for — see [Data Quality & Limitations](#data-quality--limitations).

## Tech Stack & Tools
* **Data Visualization:** Tableau Public (calculated fields, parameters, LOD expressions, dashboard actions)
* **Data Cleaning & Preprocessing:** Excel (initial cleaning pass before loading into Tableau)
* **Data Querying & Exploratory Analysis:** BigQuery (SQL)
* **Documentation:** GitHub / Markdown

## Overview

Founded in 2018, Voltrix is an e-commerce company that sells popular electronics products and has since expanded to a global customer base. Like most e-commerce companies, Voltrix sells products through its online site as well as through its mobile app, and uses a variety of marketing channels to reach customers — including email campaigns, SEO, and affiliate links. Over the last few years, its most popular products have come from Apple, Samsung, and ThinkPad.

This project is framed around a real-style stakeholder request. I'm playing the role of a data analyst partnering with Angie, Voltrix's Head of Operations, ahead of a company-wide town hall:

> **Subject:** Town hall next month - data requests
> **From:** Angie \<angie@voltrix.com\>
>
> Hi data team - the leadership team is preparing for the company-wide town hall next month and would like to present a walkthrough of our order trends from 2019-2022. Can you help us answer the following:
> - What were the overall trends in sales during this time?
> - What were our monthly and yearly growth rates?
> - How is the new loyalty program performing? Should we keep using it?
> - What were our refund rates and average order value?
>
> I'll set a meeting next week to review your findings - looking forward to discussing.
>
> Thank you,
> Angie
> *Head of Operations, Voltrix*

**Why I built this:** I picked this project to explore my own data analysis process — from data cleaning, to exploratory analysis, to a final deliverable — end to end. It's meant to be a portfolio piece that reflects how I actually work through a project, not just one isolated skill.

*(Note: this uses a synthetic practice dataset and a fictionalized company/stakeholder scenario.)*

## Key Business Objectives
* **Revenue Trends & Growth Rate:** Track Voltrix's sales trajectory, monthly and yearly growth rates, and average order value (AOV) from 2019–2022.
* **Loyalty Program Impact:** Evaluate whether Voltrix's loyalty program is performing well enough to justify continued investment.
* **Refund Rates & AOV:** Understand refund rate trends and how they relate to average order value.
* **Regional & Product Performance:** Identify which products and regions are driving sales (and refunds), as a starting point for deeper investigation by product and marketing teams.

## Executive Summary

Between 2019 and 2022, Voltrix generated **$24,045,399** in Sales across **92,930 orders**, for an overall AOV of **$258.75**.

* **Macro Trajectory:** Sharp, pandemic-era growth peaked in mid-2020 (+50.3% YoY in a single month), followed by a sustained decline through 2021 into 2022.
* **2022 Snapshot:** Sales fell to **$4.36M** (-44.1% YoY), Orders to **18,966** (-38.3% YoY), and AOV to **$229.80** (-9.4% YoY) — the steepest year of decline in the dataset.
* **Refund Tracking Gap:** Recorded refund rates fall to a flat 0% by late 2021 — a data-logging cutoff, not a real operational win. Within the reliable tracking window, the true refund rate is **7.59%**.
* **Loyalty Members Refund More:** Loyalty program members show a meaningfully higher refund rate than non-members (10.80% vs. 5.71%), a signal worth investigating alongside the program's strong adoption growth.
* **Data Quality:** ~56K orders (over half the dataset) were missing a Region value, inferred as North America based on currency (99.13% USD/CAD).

## Overall Sales Trends

Across the full 2019–2022 window, Voltrix generated **$24,045,399** in total Sales across **92,930 orders**, for an overall AOV of **$258.75**. The shape of that trend is a single, dramatic arc: monthly Sales peaked at **$883,406** during the 2020 pandemic-era surge, then declined steadily to a low of **$166,337** by the end of the dataset. Orders followed the same shape, peaking at **3,065** in a single month before falling to **719**.

That decline continued into 2022, Voltrix's weakest year on record: Sales fell to **$4.36M** for the year (-44.1% YoY), Orders to **18,966** (-38.3% YoY), and AOV to **$229.80** (-9.4% YoY). Because AOV held up far better than Sales or Orders, the 2022 decline was driven primarily by **fewer orders**, not smaller ones — see [Monthly & Yearly Growth Rates](#monthly--yearly-growth-rates) for the detailed breakdown.
<img width="1155" height="293" alt="image" src="https://github.com/user-attachments/assets/f04f2e0d-becd-4d60-9482-2affc41906a8" />

## Monthly & Yearly Growth Rates

Voltrix's growth followed a clear boom-bust pattern: explosive expansion in 2020, a cooling-off 2021, and a sharp pullback in 2022.

### Yearly Growth Rate (YoY)
* **2020 — Sales +164.6%, AOV +30.88%:** a pandemic-era demand surge, with order volume growing even faster than AOV, suggesting the growth was driven primarily by more people buying rather than bigger baskets.
* **2021 — Sales -9.6%, AOV -14.94%:** growth reversed off the prior year's high base, the first sign of the slowdown to come.
* **2022 — Sales -44.1%, AOV -9.44%:** the steepest decline in the dataset. Because AOV fell much less than Sales, the drop was driven mainly by **fewer orders**, not smaller ones.

### Monthly Volatility
The yearly figures smooth over real month-to-month swings that a stakeholder should know about:
* Sales' single best month was **+50.32%** YoY (mid-2020), and its worst was **-34.50%** YoY (late 2022) — even the "good" year of 2020 wasn't a smooth ride.
* AOV tells a different story: its sharpest jump (**+17.86%**) and its sharpest drop (**-16.45%**) both occur within a few months of each other in **late 2022** — not a steady trend in either direction, but a sign of real volatility in a shrinking order base during that stretch.

## Loyalty Program Performance & Recommendation

Because the loyalty program is genuinely new, the data shows an adoption story rather than a static comparison. Loyalty members start from almost nothing in 2019 (30 orders, $4,728 in sales) against an already-established Non-Loyalty base (peaking at 1,927 orders and $636,741 in monthly sales in 2020). By 2021, Loyalty program orders and sales overtake Non-Loyalty, and stay higher through 2022 as both segments decline together in the broader downturn.

On a per-order basis, Non-Loyalty customers have historically spent more — AOV peaked at $378.19 for Non-Loyalty vs. just $130.49 for Loyalty members early on, though that gap narrows considerably by 2022. Product mix explains part of this: Loyalty purchases are heavily concentrated in one product (58.17% Apple AirPods), while Non-Loyalty customers spread across both ends of the price spectrum — 31.32% on the low-cost Samsung Charging Cable Pack and a combined 8.29% on MacBook Air and ThinkPad laptops, versus just 3.56% for Loyalty. Non-Loyalty's higher AOV isn't simply "bigger spenders" — it's a basket mix that includes more high-ticket laptops.

One caution worth flagging: Loyalty program members show a meaningfully higher refund rate than non-members (10.80% vs. 5.71%, within the reliable refund-tracking window — see [Refund Rates & AOV](#refund-rates--aov)).

**Verdict: Keep investing in the loyalty program.** Its adoption curve is a real growth story — overtaking Non-Loyalty customers in both order volume and total sales within about two years of launching from a near-zero base. That said, the elevated refund rate among members is worth investigating before scaling further — worth checking whether it's concentrated in the AirPods purchases that dominate their basket, given how heavily loyalty members over-index on that one product.

## Refund Rates & AOV

While investigating refund rates, I found that the `REFUNDED` field goes flat to **0%** for every month from August 2021 onward — while order volume during that same window continues completely normally. That's inconsistent with a real business trend and points to a data-tracking cutoff, not an actual drop in refunds. To avoid reporting a misleadingly low number, every refund-rate figure below is calculated on the **clean window (January 2019 – July 2021)**, where tracking still appears complete. I confirmed this cutoff hits every segment equally — loyalty status, marketing channel, and region all drop to zero at the same point — which rules out it being isolated to one part of the business.

* **Overall refund rate:** **7.59%** within the clean window, vs. a diluted **5.06%** if the full (broken) dataset is used unadjusted — a difference large enough to materially mislead a stakeholder.
* **By product:** refund rate climbs with price point. ThinkPad Laptop (17.60%) and MacBook Air Laptop (17.42%) — Voltrix's two highest-AOV products — have by far the highest refund rates, compared to 2.15% for the low-cost Samsung Charging Cable Pack and 0% for Bose Soundsport Headphones. Higher AOV products carry meaningfully more refund risk, which matters financially: a refunded laptop is a much bigger revenue hit than a refunded accessory.
* **By loyalty program:** Loyalty members refund at **10.80%**, nearly double Non-Loyalty's **5.71%** (see [Loyalty Program Performance](#loyalty-program-performance--recommendation) for the likely product-mix explanation).
* **By marketing channel:** Unknown/untracked orders have the highest refund rate (13.21%), followed by Social Media (9.89%), Email (7.71%), Direct (7.57%), and Affiliate (6.35%). This ranks differently than a simple count of refunds over time would suggest — Direct generates the most refunds in raw volume simply because it's the largest channel, but it doesn't convert the largest *share* of its orders into refunds. "Unknown" also isn't a real channel — it's an attribution gap — so its high rate may reflect untracked traffic behaving differently, or could itself be an artifact of whatever caused that channel data to go missing.

## Region & Product Mix

### Product Mix

The 27in 4K Gaming Monitor and Apple AirPods Headphones are Voltrix's two largest products by Sales, together making up roughly 60-65% of total sales at any point in the dataset, with the Gaming Monitor typically the single largest revenue contributor.

By Order Count, the picture shifts: AirPods dominate even more heavily (~45-50% of orders), and the low-cost Samsung Charging Cable Pack — barely visible in the Sales breakdown — becomes a major share of order volume. In other words, Voltrix's order *volume* is driven largely by cheap, high-frequency accessories, while its *revenue* leans more heavily on the pricier Gaming Monitor.

Purchase platform makes this split even clearer: high-ticket items are bought almost exclusively on **desktop/website** (MacBook Air: 4.38% website vs. 0.04% mobile app; ThinkPad: 3.22% vs. 0%; Gaming Monitor: 25.91% vs. 0.70%), while low-cost accessories skew heavily toward the **mobile app** (Samsung Charging Cable Pack: 51.63% mobile vs. 13.96% website; Samsung Webcam: 37.31% vs. 0.18%). This is directly actionable: the desktop checkout experience matters most for big-ticket conversion, while the mobile app experience is what's actually driving accessory order volume.

Notably, MacBook Air and ThinkPad laptops — while a small share of total order volume — also carry Voltrix's highest refund rates (17.42% and 17.60% respectively; see [Refund Rates & AOV](#refund-rates--aov)), reinforcing that these low-volume, high-ticket items warrant closer quality or fulfillment attention relative to their revenue contribution.

### Region Performance

North America (NA) is Voltrix's largest region by both Sales (peaking at $484,459/month) and Orders (peaking at 1,588/month) — consistent with the earlier finding that most of the ~56K orders with a missing Region value were inferred to be North American based on currency (see [Data Quality & Limitations](#data-quality--limitations)). EMEA is a clear second, APAC third, and LATAM consistently the smallest region throughout.

AOV is far more uniform across regions than Sales or Orders — all four cluster in a broadly similar range — suggesting regional differences come mostly from order *volume*, not from customers in different regions spending fundamentally different amounts per order.

Refund rate shows comparatively little regional spread: NA 8.17%, APAC 7.36%, EMEA 6.91%, LATAM 6.55% — a range of only about 1.6 points, much tighter than the gaps seen by loyalty program (10.80% vs. 5.71%) or marketing channel (13.21% vs. 6.35%). NA does have the highest *absolute* refund volume, but — consistent with the same pattern noted earlier for Direct and Loyalty — that's substantially a reflection of NA simply being the largest region by order count, not a proportionally worse return rate.

## Data Quality & Limitations

This dataset required real cleanup, and I want to be transparent about what was found and how it was (or wasn't) resolved:

* **Refund-tracking cutoff:** `REFUNDED` data goes flat to 0% for every month from August 2021 onward, while order volume continues normally through the same period — a data-logging cutoff, not a real drop in refunds. All refund-rate figures in this project use the January 2019–July 2021 window instead of the full dataset. I confirmed this cutoff hits every segment I checked equally (loyalty status, marketing channel, and region all drop to zero at the same point), which rules out it being isolated to one part of the business — it's a systemic tracking issue, not a data quirk specific to one team or channel.
* **Region inference:** ~56K orders with a missing Region value were inferred to be North America based on currency (99.13% USD/CAD), not confirmed by an explicit country/region field. This is disclosed on the Region dashboard as an inference, not presented as fact.
* **Corrupted Order IDs:** 98 rows have malformed `ORDER_ID` values (extremely long digit strings), consistent with floating-point precision corruption at some point in the data's history. These rows were not excluded from totals but are flagged here for transparency.
* **Open question — duplicate row handling:** the dataset contains literal duplicate order-product rows (verified up to 3x duplication for the same pair), corrected for using a `Duplicate Count` calculation so Sales/Orders/AOV aren't overcounted. During QA, I found a small inconsistency between two derivations that should mathematically match, which I wasn't able to fully root-cause. It doesn't change the direction of any finding in this project, but it's an open item I'd want to resolve with more time.

I'd rather report these limitations plainly than hide a clean-looking number that isn't fully trustworthy.

## Strategic Recommendations

### 1. Investigate and Reverse the 2022 Decline
**The Insight:** Sales fell 44.1% YoY in 2022, driven primarily by fewer orders rather than a falling AOV — customers are still spending about the same per order, there are just fewer of them.

**Action:** Launch reactivation campaigns targeting customers acquired during the 2020 demand peak who haven't ordered recently, and investigate whether the falling order count reflects competitive pressure, reduced marketing spend, or broader market conditions.

### 2. Keep Scaling the Loyalty Program, but Audit Its Refund Behavior First
**The Insight:** Loyalty adoption grew from near-zero in 2019 to overtaking Non-Loyalty customers in both orders and sales by 2021. But Loyalty members refund at nearly double the rate of Non-Loyalty (10.80% vs. 5.71%), and their purchases are unusually concentrated in one product (58.17% Apple AirPods).

**Action:** Continue investing in loyalty acquisition, but audit refund reasons specifically for AirPods purchases before scaling further, since the elevated refund rate is closely tied to that single product concentration.

### 3. Prioritize Quality Control on High-Ticket Products
**The Insight:** MacBook Air and ThinkPad laptops carry Voltrix's highest refund rates (17.42% and 17.60%) despite being a small share of total order volume — each refund there represents a much larger revenue loss than an accessory refund.

**Action:** Prioritize return-reason tracking and fulfillment/quality review specifically for laptop SKUs, where the revenue impact per refund is highest.

### 4. Audit the Data Pipeline Behind Refund Tracking
**The Insight:** Refund tracking silently dropped to 0% after July 2021. Left uncorrected, this would have completely hidden a real refund problem — or made a bad quarter look artificially clean.

**Action:** Audit the ETL pipeline responsible for populating `REFUNDED`, and add automated data-quality monitoring (e.g., alert if any tracked metric goes flat to zero for multiple consecutive months) to catch this kind of silent failure sooner.

### 5. Match Channel Investment to Purchase Behavior
**The Insight:** High-ticket items (laptops, the gaming monitor) are purchased almost exclusively via desktop/website, while low-cost accessories (charging cables, webcams) drive mobile app order volume.

**Action:** Prioritize the desktop checkout experience for big-ticket conversion, while keeping the mobile app experience frictionless for the high-frequency accessory purchases it actually drives.

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
