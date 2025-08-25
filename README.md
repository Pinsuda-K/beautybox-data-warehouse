#### beautybox-data-warehouse
# BeautyBox Data Warehouse – v1 Status
![Status](https://img.shields.io/badge/status-in%20progress-yellow)
![Version](https://img.shields.io/badge/version-v1.0-blue)
![Database](https://img.shields.io/badge/database-PostgreSQL-336791?logo=postgresql)
![Queries](https://img.shields.io/badge/analysis-7%20SQL%20queries-brightgreen)
![Language](https://img.shields.io/badge/language-SQL-lightgrey)
> BeautyBox Data Warehouse is a portfolio project simulating a retail analytics pipeline for a fictional cosmetics brand. Using PostgreSQL, synthetic sales and customer data are generated and transformed into actionable insights with SQL queries. The project demonstrates data modeling, ETL-like seeding, and business intelligence reporting.

### Project Overview
This project simulates a retail analytics pipeline for a fictional cosmetics retailer. It uses PostgreSQL as the data warehouse engine, with three main components:
- **Schema setup** – tables for customers, branches, orders, order_items, products, and categories.
- **Seed data generation** – synthetic but realistic transactions from Jan–Jul 2025 (≈5,000 orders evenly across 4 branches).
- **Analysis queries** – a set of SQL scripts answering key business questions.
---
### Key features:
- PostgreSQL schema for retail transactions.
- Synthetic dataset (5,000 orders across 4 branches, members & general customers).
- Core analytics queries: revenue growth, AOV, product performance, profit margins, customer segments, and campaign evaluation.
- Designed for extension into dashboards and advanced analytics in later versions.
- โครงสร้างฐานข้อมูล (Schema) สำหรับธุรกิจค้าปลีก
- ชุดข้อมูลจำลอง (5,000 ออเดอร์จาก 4 สาขา ลูกค้าสมาชิกและทั่วไป)
- คิวรีวิเคราะห์หลัก: การเติบโตของรายได้, ค่าเฉลี่ยการสั่งซื้อ, สินค้าทำกำไรสูงสุด, มาร์จินตามหมวดสินค้า, การเปรียบเทียบสมาชิก vs ลูกค้าทั่วไป และการประเมินผลแคมเปญโปรโมชั่น
- ออกแบบเพื่อสร้าง Dashboard และการวิเคราะห์ข้อมูลในขั้นต่อไป
---
### Current Milestone (v1)
As of this version:
- Database initialized with sample data.
- Seed data validated (distribution of orders, members vs general, branch balance).
- 7 core business metrics implemented in **analysis.sql.**
---
### Key Metrics:
1. Monthly Revenue + MoM Growth (Paid Orders Only)
   - Tracks revenue trends with specific date range, from Jan–Jul 2025.
     - <details> Result: Revenue fluctuates between ~930K–1.07M, with natural ups/downs. </details>
   - Useful for seasonality & growth analysis.
2. Average Order Value (AOV)
   - Formula: Revenue ÷ Orders.
     - <details> Result: ~1,487 THB/order across all segments. </details>
   - Benchmark for customer spending power.
3. Top 10 Products by Profit
   - Rank-ordered products based on profit contribution.
     - <details> Result: Fragrances dominate the top, followed by skincare. </details>
   - Helps in product strategy & promotion targeting.
4. Member vs General Customer Performance
   - Compares loyalty members vs general shoppers.
     - <details> Result: Members = 34% of orders, slightly higher AOV (1491 vs 1486). </details>
   - Validates loyalty program impact.
5. Profit Margin % by Category
   - Profit ÷ Sales by category.
     - <details> Result: Fragrance ~46% (highest), Hair ~33% (lowest). </details>
   - Guides pricing & category focus.
6. Branch Leaderboard
   - Revenue, profit, and margin by branch.
     - <details> Result: 4 branches perform almost equally due to even seeding. </details>
   - Infrastructure check — will show variation with real data.
7. Promo Effect (April vs March & May)
   - Compare promo month (April) with surrounding months.
     - <details> Result: April dips to 993K, while Mar/May stay ~1.1M. </details>
   - Useful for campaign evaluation.
---
#### Sample Query Snippets:
<details> <summary>Click to view sample SQL query: Monthly Revenue + MoM Growth (Paid Orders Only)</summary><pre> 
WITH m AS (
  SELECT to_char(order_date, 'YYYY-MM') AS ym,
         SUM(pay.amount) AS revenue
  FROM orders o
  JOIN payments pay USING(order_id)
  WHERE pay.payment_status='PAID'
  GROUP BY ym
)
SELECT ym,
       revenue,
       ROUND(
         (revenue - LAG(revenue) OVER (ORDER BY ym))
         * 100.0 / NULLIF(LAG(revenue) OVER (ORDER BY ym),0), 2
       ) AS mom_growth_pct
FROM m
ORDER BY ym; 
</pre></details>
---

### Next Steps (v2 Roadmap):
- Add more realistic variability in branches (so some outperform).
- Enhance promo seeding (discounted products, campaign tags).
- Add customer lifetime value (CLV) + retention analysis queries.
- Results to dashboards (e.g., Metabase, Power BI, or Superset).
---
### 💡Note for Reviewers:
This v1 milestone shows a working data pipeline from schema → synthetic data → SQL analysis.
It demonstrates familiarity with PostgreSQL, data modeling, and business analytics queries.
