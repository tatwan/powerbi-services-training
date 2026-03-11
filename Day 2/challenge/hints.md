# Hints — Customer Analytics Dashboard

> **Use this file only after you've attempted each step on your own.**
> Work through the hints one section at a time — don't read ahead.
> Cross-reference the rendered mockup (`mockup.html`) at each stage to check your layout.

---

## Step 1: Connect to Snowflake and Load Data

1. Open Power BI Desktop → new blank file
2. **Home** → **Get Data** → **More** → search for **Snowflake** → **Connect**
3. Enter connection details:
   - **Server**: `[your-account].snowflakecomputing.com`
   - **Warehouse**: `[your-warehouse]`
   - **Database**: `CHINOOK_DB`
4. Authenticate with your Snowflake username and password
5. In the Navigator, check these 4 tables and click **Load**:
   - ✅ `CUSTOMER`
   - ✅ `INVOICE`
   - ✅ `INVOICE_LINE`
   - ✅ `EMPLOYEE`

> **📋 Checkpoint — Data pane (right side)**
> You should see 4 tables listed. Expand each one to confirm the columns below exist before continuing:
>
> | Table | Key columns to confirm |
> |---|---|
> | CUSTOMER | CUSTOMER_ID, FIRST_NAME, LAST_NAME, CITY, COUNTRY, SUPPORT_REP_ID |
> | INVOICE | INVOICE_ID, CUSTOMER_ID, INVOICE_DATE, TOTAL |
> | INVOICE_LINE | INVOICE_LINE_ID, INVOICE_ID, UNIT_PRICE, QUANTITY |
> | EMPLOYEE | EMPLOYEE_ID, FIRST_NAME, LAST_NAME, TITLE |
>
> **If a table is missing:** go back to Home → Transform Data → check the query list on the left for any errors marked with a ⚠️ yellow triangle.

---

## Step 2: Create Relationships in Model View

1. Click the **Model View** icon (third icon in the left sidebar — looks like three connected boxes)
2. Drag the tables to arrange them in a star pattern — `INVOICE` in the center
3. Create relationships by dragging from one column to another:
   - `CUSTOMER[CUSTOMER_ID]` → `INVOICE[CUSTOMER_ID]`
   - `INVOICE[INVOICE_ID]` → `INVOICE_LINE[INVOICE_ID]`
   - `CUSTOMER[SUPPORT_REP_ID]` → `EMPLOYEE[EMPLOYEE_ID]`

> **📋 Checkpoint — Model View canvas**
> You should see 3 relationship lines connecting the 4 tables:
>
> ```
> EMPLOYEE ──── CUSTOMER ──── INVOICE ──── INVOICE_LINE
> ```
>
> Each line should show **1** on one end and **\*** (many) on the other.
> - CUSTOMER → INVOICE: 1 (customer) to many (invoices) ✓
> - INVOICE → INVOICE_LINE: 1 (invoice) to many (line items) ✓
> - EMPLOYEE → CUSTOMER: 1 (employee) to many (customers) ✓
>
> **If a relationship shows a warning:** double-click the line to edit it. The cardinality should be "Many to one (*:1)" and Cross filter direction should be "Single".

---

## Step 3: Create Calculated Columns

Switch to **Report View** first, then use the **Modeling** tab → **New Column** for each of these.

### CUSTOMER table — Full Name
Click the `CUSTOMER` table in the Data pane, then:
```dax
Full Name = CUSTOMER[FIRST_NAME] & " " & CUSTOMER[LAST_NAME]
```

### EMPLOYEE table — Full Name
Click the `EMPLOYEE` table, then:
```dax
Full Name = EMPLOYEE[FIRST_NAME] & " " & EMPLOYEE[LAST_NAME]
```

### INVOICE_LINE table — Revenue
Click `INVOICE_LINE`, then:
```dax
Revenue = INVOICE_LINE[UNIT_PRICE] * INVOICE_LINE[QUANTITY]
```

> **📋 Checkpoint — Data pane**
> After creating all three columns, expand each table in the Data pane. You should see:
> - `CUSTOMER` → a new `Full Name` column (with a calculator icon **fx**)
> - `EMPLOYEE` → a new `Full Name` column
> - `INVOICE_LINE` → a new `Revenue` column
>
> **Quick sanity check:** Click the `INVOICE_LINE` table → switch to **Table View** (second icon in the left sidebar). Sort the `Revenue` column descending. The highest values should be around $25–$99. If you see very large numbers like $10,000+, check that UNIT_PRICE and QUANTITY are both numeric columns (not text).

---

## Step 4: Create Measures

Right-click the table name in the Data pane → **New Measure** for each. Place measures in the table that makes the most logical sense (shown below).

### In the CUSTOMER table

```dax
Total Customers = DISTINCTCOUNT(CUSTOMER[CUSTOMER_ID])
```

```dax
Countries Served = DISTINCTCOUNT(CUSTOMER[COUNTRY])
```

```dax
Customer Lifetime Value = DIVIDE(
    SUM(INVOICE_LINE[Revenue]),
    [Total Customers]
)
```

### In the INVOICE table

```dax
Avg Orders per Customer = DIVIDE(
    COUNTROWS(INVOICE),
    [Total Customers]
)
```

### In the INVOICE_LINE table

```dax
Total Revenue = SUM(INVOICE_LINE[Revenue])
```

> **📋 Checkpoint — expected measure values**
> Click each measure in the Data pane. The value shown in the bottom status bar (or a blank card visual) should be approximately:
>
> | Measure | Expected value |
> |---|---|
> | Total Customers | **59** |
> | Countries Served | **24** |
> | Customer Lifetime Value | **$39.47** |
> | Avg Orders per Customer | **~7.0** |
> | Total Revenue | **~$2,328** |
>
> **If Total Revenue shows $0 or blank:** check that the `Revenue` calculated column exists in INVOICE_LINE and that the measure references `INVOICE_LINE[Revenue]` (not `INVOICE_LINE[UNIT_PRICE]`).
>
> **If Customer Lifetime Value shows a very large number** (e.g., $39,000+): the formula is likely summing revenue *per customer row* instead of dividing. Make sure you're using `DIVIDE([Total Revenue], [Total Customers])` — both referencing the *measures*, not the raw columns.

---

## Step 5: Build the 4 KPI Cards

Add a **Card** visual for each measure. Drag each card to the top of the canvas side-by-side.

| Card # | Measure | Label to set | Format |
|---|---|---|---|
| 1 | `Total Customers` | "Total Customers" | Whole number |
| 2 | `Customer Lifetime Value` | "Avg Lifetime Value" | Currency, 2 decimals |
| 3 | `Countries Served` | "Countries Served" | Whole number |
| 4 | `Avg Orders per Customer` | "Avg Orders / Customer" | Decimal, 1 place |

**To align all 4 cards:** select all four (Ctrl+click each) → **Format** ribbon → **Align** → **Align Top**, then **Distribute Horizontally**.

> **📋 Checkpoint — canvas top row**
> The four cards should read (left to right): **59 · $39.47 · 24 · 7.0**
>
> The cards should be the same height and evenly spaced. Compare against the top row of `mockup.html`.
>
> **If the label doesn't update:** click the card → Format pane → **General** → **Title** → type the label text there, or use **Category label** → type in the Custom label field.

---

## Step 6: Customer Distribution Map

1. Click a blank area of the canvas (below the KPI cards)
2. Select **Filled Map** from the Visualizations pane (globe icon with filled regions)
3. Drag fields:
   - `CUSTOMER[COUNTRY]` → **Location**
   - `Total Customers` measure → **Color saturation**
   - `Total Revenue` measure → **Tooltips**
4. Resize to take up about 55–60% of the canvas width

**Format settings to apply:**
- Title: `Customer Distribution by Country`
- Data colors: choose a gradient (recommend light-to-dark teal/green)
- Map style: **Road** or **Grayscale** (avoids busy backgrounds)
- Turn on **Zoom buttons**

> **📋 Checkpoint — map visual**
> The map should show colored regions across multiple continents. The **darkest** shading should appear over:
> - **USA** (13 customers — highest)
> - **Canada** (8 customers)
> - **Brazil** and **France** (5 customers each)
>
> Europe should have several moderately shaded countries. South America, India, and Australia should show lighter shading.
>
> **If the map shows a warning "Geocoding may not be accurate":** this is normal — click **OK** to proceed. Power BI geocodes country names automatically.
>
> **If all countries show the same shade:** make sure you dragged the `Total Customers` *measure* (not the `CUSTOMER_ID` column) to Color saturation.

---

## Step 7: Customer Segmentation Treemap

1. Click a blank area to the right of the map
2. Select **Treemap** from Visualizations
3. Drag fields:
   - `CUSTOMER[COUNTRY]` → **Category**
   - `Total Revenue` measure → **Values**
4. Resize to fill the remaining canvas width in that row

**Format settings:**
- Title: `Customer Segmentation by Value`
- Data labels: **On**
- Category labels: **On**

> **📋 Checkpoint — treemap visual**
> The largest rectangle should be labeled **USA** and represent roughly 22% of the total area.
> The second largest should be **Canada**, followed by **France** and **Brazil** in similar sizes.
>
> Expected top boxes (approximate):
> - USA: ~$523 → largest box
> - Canada: ~$304 → second
> - France: ~$195 and Brazil: ~$190 → similar medium boxes
> - Germany, Czech Republic, UK → smaller boxes
>
> Compare proportions visually against the treemap in `mockup.html`.
>
> **If only one rectangle shows:** check that COUNTRY is in the **Category** field well, not Values. Values should only contain `Total Revenue`.

---

## Step 8: Top 10 Customers Table

1. Click a blank area below the map/treemap row
2. Select **Table** visual
3. Add columns in this order:
   - `CUSTOMER[Full Name]` → Columns
   - `CUSTOMER[COUNTRY]` → Columns
   - `Total Revenue` measure → Columns
4. **Apply a Top N filter:**
   - In the **Filters** pane, find `Full Name` under "Filters on this visual"
   - Change filter type to **Top N**
   - Show: **Top 10** / By value: drag `Total Revenue`
   - Click **Apply filter**
5. Sort by Total Revenue (descending) — click the column header

**Format:**
- Title: `Top 10 Customers by Lifetime Value`
- Style presets: **Minimal**
- Format Total Revenue column as **Currency**

> **📋 Checkpoint — table content**
> The table should show exactly 10 rows. The top 5 rows should look like:
>
> | # | Full Name | Country | Revenue |
> |---|---|---|---|
> | 1 | Helena Holy | Czech Republic | $49.62 |
> | 2 | Richard Cunningham | USA | $47.62 |
> | 3 | Luis Rojas | Chile | $46.42 |
> | 4 | Hugh O'Reilly | Ireland | $45.62 |
> | 5 | Julia Barnett | USA | $43.62 |
>
> **If the table shows more than 10 rows:** the Top N filter isn't applied. Go to Filters pane → make sure filter type is "Top N" (not "Basic filtering") and the "By value" field contains `Total Revenue`.
>
> **If the names are showing as separate First/Last columns:** you're dragging `FIRST_NAME` and `LAST_NAME` separately instead of the calculated `Full Name` column.

---

## Step 9: Top Cities Table

1. Click a blank area to the right of the customers table
2. Select **Table** visual
3. Add fields:
   - `CUSTOMER[CITY]` → Columns
   - `Total Customers` → Columns
   - `Total Revenue` → Columns
4. **Apply Top N filter** on `CITY`:
   - Top N: **10** / By value: `Total Customers`
   - Apply filter
5. Sort by Total Customers (descending)

**Format:** Title: `Top Cities by Customer Count`, Minimal style

> **📋 Checkpoint — table content**
> Cities with 2 customers should appear first. The top rows should include cities like **Prague**, **Mountain View**, **São Paulo**, **London**, **Berlin**, and **Paris** (all with 2 customers). Cities with 1 customer fill the remaining rows.
>
> **If cities are missing or wrong:** check that the `CITY` column is from the `CUSTOMER` table (not INVOICE). The data is on the customer record, not the transaction.

---

## Step 10: Revenue by Support Rep — Bar Chart

1. Click a blank area below the two tables
2. Select **Clustered Bar Chart** (horizontal bars)
3. Add fields:
   - `EMPLOYEE[Full Name]` → **Y-axis**
   - `Total Revenue` → **X-axis**
4. **Filter to Sales Support Agents only:**
   - Filters pane → "Filters on this visual" → drag `EMPLOYEE[TITLE]`
   - Change to **Basic filtering**
   - Check the box for **"Sales Support Agent"**
   - Click **Apply filter**

**Format:**
- Title: `Revenue by Support Representative`
- Data labels: **On**
- X-axis display units: **None** (so full dollar amounts show)
- Y-axis title: **Off**

> **📋 Checkpoint — bar chart**
> The chart should show exactly **3 bars** (one per Sales Support Agent), all horizontal:
>
> ```
> Jane Peacock    ████████████████████████████  $833
> Margaret Park   ██████████████████████████    $775
> Steve Johnson   █████████████████████████     $720
> ```
>
> **If you see more than 3 employees:** the Title filter is not applied. In the Filters pane, make sure TITLE is filtered to "Sales Support Agent" only.
>
> **If bars are vertical instead of horizontal:** you selected "Clustered Column Chart" instead of "Clustered Bar Chart". Switch the visual type — same field mapping, different orientation.
>
> **If employee names don't appear:** check that you're using `EMPLOYEE[Full Name]` (the calculated column you created), not `EMPLOYEE[FIRST_NAME]`.

---

## Step 11: Support Rep Performance Table

1. Click a blank area next to the bar chart
2. Select **Table** visual
3. Add fields:
   - `EMPLOYEE[Full Name]` → Columns
   - `Total Revenue` → Columns
   - `Total Customers` → Columns
4. **Apply the same filter** as the bar chart (TITLE = "Sales Support Agent")
5. Sort by Total Revenue (descending)

**Format:** Title: `Support Rep Performance`, format Total Revenue as Currency

> **📋 Checkpoint — table content**
> The table should show 3 rows:
>
> | Employee Name | Total Revenue | Customers |
> |---|---|---|
> | Jane Peacock | $833.04 | 21 |
> | Margaret Park | $775.40 | 20 |
> | Steve Johnson | $720.16 | 18 |
>
> Total customers across all 3 reps (21 + 20 + 18 = 59) should match your `Total Customers` KPI card.
>
> **If Total Customers shows a different number in this table:** the EMPLOYEE → CUSTOMER relationship might be filtering in the wrong direction. Go to Model View → double-click the relationship line → check Cross filter direction.

---

## Step 12: Final Formatting and Polish

### Add a report title
- **Insert** tab → **Text Box**
- Type: `Customer Analytics & Geographic Distribution`
- Font size 20, Bold, centered
- Place at the very top of the canvas (above the KPI cards)

### Apply consistent colors
- Select each visual → **Format** pane → **Data colors**
- Use a consistent teal/green palette across all visuals (match the color you used for the KPI card top borders)

### Align visuals
- Select multiple visuals (Ctrl+click) → **Format** ribbon → **Align**
- Use **Align Left** and **Distribute Vertically** to clean up spacing

### Test cross-filtering
Click on:
- A **country in the map** → all other visuals should filter to show only that country's data
- A **customer name** in the Top 10 table → the map should highlight that customer's country
- A **support rep name** in the bar chart → the customer tables should filter to their customers

> **📋 Final visual check — compare to mockup**
> Open `mockup.html` and compare each section to your report:
> - KPI cards: same 4 values in the same order
> - Map: correct global spread of customer dots
> - Treemap: USA clearly the largest, proportions look reasonable
> - Tables: 10 rows each, sorted correctly
> - Bar chart: 3 horizontal bars, Jane Peacock longest
> - Support Rep table: 3 rows with correct revenue totals

---

## Step 13: Save Your Work

**File → Save As** → name it `Chinook_Customer_Dashboard.pbix`

---

## Common Issues Quick-Reference

| Problem | Most likely cause | Fix |
|---|---|---|
| Map shows blank / "no data" | COUNTRY column not recognized as geographic | Right-click COUNTRY column → Data category → Country |
| Treemap shows one huge block | Values field has wrong column | Make sure COUNTRY is in Category, Total Revenue in Values |
| Top N filter shows all rows | Filter type not changed | Filters pane → change from "Basic filtering" to "Top N" |
| Support rep visuals show 9 employees | TITLE filter not applied | Add EMPLOYEE[TITLE] to Filters pane, select "Sales Support Agent" |
| Customer Lifetime Value = $0 | INVOICE_LINE[Revenue] column missing | Create the Revenue calculated column in INVOICE_LINE first |
| Cross-filtering doesn't work between tables | Missing relationship | Check Model View — all 3 relationship lines must exist |
| Names show as blanks | Full Name column references wrong table | Re-create: `CUSTOMER[FIRST_NAME] & " " & CUSTOMER[LAST_NAME]` |
