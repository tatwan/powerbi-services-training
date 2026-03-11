# Day 2 Challenge — Customer Analytics Dashboard

## The Scenario

The Head of Sales was happy with the morning's dashboard — but now the Head of Customer Success has a request of her own.

She stops by your desk after lunch:

> *"The sales numbers are great, but I need to understand our customer base, not just our revenue. Who are our most valuable customers? Where in the world are they coming from — is it mostly the US, or are we genuinely global? And I want to make sure our three support reps are distributing the workload fairly and performing consistently. Can you build me a customer analytics view? I need it before end of day."*

This is a **different lens on the same data**: same Chinook database, same Snowflake connection — but now you're asking customer-level questions rather than revenue-level ones. You'll need to think about how customers relate to invoices, what "customer lifetime value" means in this context, and how to connect employees to the customers they support.

You're working independently this time. No step-by-step walkthrough. Use what you built this morning, the skills from Day 1, and the mockup as your target.

> **Open the mockup first:** [`mockup.html`](mockup.html) — study it before you start. Know what you're building before you open Power BI.

---

## What You Are Building

A single-page Power BI report with **9 visuals** that answer the question: *Who are our customers, where are they, and who is taking care of them?*

---

## Required Tables

Connect to Snowflake and load exactly these 4 tables from `CHINOOK_DB`:

| Table | Role |
|---|---|
| `CUSTOMER` | Dimension — customer profiles |
| `INVOICE` | Fact — purchase transactions |
| `INVOICE_LINE` | Fact — line items per invoice |
| `EMPLOYEE` | Dimension — support staff |

---

## Required Measures & Calculated Columns

You must create these yourself. The names and definitions below tell you what each one should do — you need to write the DAX.

### Calculated Columns (stored in the table, row-level)

| Name | Table | What it should compute |
|---|---|---|
| `Full Name` | `CUSTOMER` | Concatenate first name + space + last name |
| `Full Name` | `EMPLOYEE` | Same pattern for employees |
| `Revenue` | `INVOICE_LINE` | Unit price × quantity for each line item |

### Measures (dynamic aggregations)

| Measure Name | What it should compute |
|---|---|
| `Total Revenue` | Sum of all Revenue (from INVOICE_LINE) |
| `Total Customers` | Count of distinct customer IDs |
| `Customer Lifetime Value` | Total Revenue divided by Total Customers |
| `Countries Served` | Count of distinct countries in CUSTOMER |
| `Avg Orders per Customer` | Total invoice count divided by Total Customers |

---

## Required Visuals

Build all 9 visuals and lay them out to match the mockup:

| Visual | Type | Key fields |
|---|---|---|
| Total Customers | Card | Your measure |
| Avg Lifetime Value | Card | Your measure (format as currency) |
| Countries Served | Card | Your measure |
| Avg Orders / Customer | Card | Your measure (1 decimal) |
| Customer Distribution by Country | Filled Map or Map | Country → Location, Total Customers → Size/Color |
| Customer Segmentation by Value | Treemap | Country → Category, Total Revenue → Values |
| Top 10 Customers by Lifetime Value | Table | Full Name, Country, Total Revenue — filtered to Top 10 |
| Top Cities by Customer Count | Table | City, Total Customers, Total Revenue — filtered to Top 10 |
| Revenue by Support Rep | Clustered Bar Chart | Employee Full Name → Axis, Total Revenue → Values |
| Support Rep Performance | Table | Employee Full Name, Total Revenue |

> **Note on the Support Rep visuals:** The EMPLOYEE table includes more than just support reps. Filter both visuals so they only show employees with the job title **"Sales Support Agent"**.

---

## Success Criteria

Your finished dashboard should meet all of these:

- [ ] All 9 visuals are present and populated with real data
- [ ] The 4 KPI cards show the correct computed values
- [ ] The map displays customers across multiple countries (not just one)
- [ ] The Top 10 tables are filtered and sorted correctly
- [ ] The support rep visuals show only the 3 Sales Support Agents
- [ ] Clicking a country in the map cross-filters the other visuals
- [ ] The report has a title and a consistent color theme
- [ ] The file is saved as `Chinook_Customer_Dashboard.pbix`

---

## Getting Stuck?

Work through these in order before opening the hints file:

1. **Re-read your measures from Dashboard 1** — many of the patterns are identical
2. **Check the model view** — if a visual shows blank data, a relationship is probably missing or backwards
3. **Check the filter pane** — Top N filters need both a field and a "by value" measure to work
4. **Open `hints.md`** — step-by-step guidance is available if you need it

---

## Stretch Goals (if you finish early)

- Add a **date slicer** using `INVOICE[INVOICE_DATE]` so the dashboard is filterable by time period
- Add **conditional formatting** to the Top 10 Customers table — color the top 3 rows green
- Create a **"Revenue per Order"** measure: `Total Revenue ÷ COUNTROWS(INVOICE)`
- Add a **scatter chart**: X = Total Revenue per customer, Y = Total Orders per customer — one dot per customer
- Build a **drill-through page** — right-click any customer name → see their full invoice history
