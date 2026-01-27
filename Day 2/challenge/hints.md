# Dashboard 2: Customer Analytics & Geographic Distribution

## Activity Overview

In this activity, you'll build a customer-focused dashboard using the Chinook music store database. You'll analyze customer behavior, geographic distribution, and support representative performance.

**What you'll create:**

- 4 KPI summary cards
- Customer distribution map
- Customer segmentation treemap
- Top 10 customers by lifetime value
- Top cities by customer count
- Support representative performance visuals

**Estimated time:** 45-60 minutes

------

## Step 1: Connect to Snowflake and Load Data

1. **Open Power BI Desktop** (or start a new file)
2. **Get Data**:
   - Click **Home** tab → **Get Data** → **More**
   - Search for "Snowflake" and select **Snowflake**
   - Click **Connect**
3. **Enter Connection Details**:
   - Server: `[your-snowflake-account].snowflakecomputing.com`
   - Warehouse: `[your-warehouse-name]`
   - Database: `CHINOOK_DB` (or your database name)
   - Click **OK**
4. **Authenticate**:
   - Enter your Snowflake username and password
   - Click **Connect**
5. **Select Tables**:
   - In the Navigator window, expand your database and schema
   - Check the boxes for these 4 tables:
     - ✅ `CUSTOMER`
     - ✅ `INVOICE`
     - ✅ `INVOICE_LINE`
     - ✅ `EMPLOYEE`
   - Click **Load**
6. **Wait for data to load**

> [!note]
>
> This dashboard uses fewer tables than Dashboard 1 since we're focusing on customer and employee analytics rather than product sales.

------

## Step 2: Create Relationships in Model View

1. Click **Model View** icon (left sidebar, bottom icon)
2. **Arrange your tables**:
   - Center: `INVOICE` (fact table)
   - Around it: `CUSTOMER`, `INVOICE_LINE`, `EMPLOYEE`
3. **Create the following relationships** (drag from one column to another):
   - `CUSTOMER[CUSTOMER_ID]` → `INVOICE[CUSTOMER_ID]`
   - `INVOICE[INVOICE_ID]` → `INVOICE_LINE[INVOICE_ID]`
   - `CUSTOMER[SUPPORT_REP_ID]` → `EMPLOYEE[EMPLOYEE_ID]`
4. **Verify relationships** - each line should show "1" on one side and "*" on the other
5. Click **Report View** to return to canvas

> [!important]
>
> The relationship between CUSTOMER and EMPLOYEE connects customers to their support representatives. This enables analysis by sales rep.

------

## Step 3: Create Calculated Columns

We need to create some calculated columns for better reporting.

### Create Customer Full Name

1. Click on `CUSTOMER` table in the **Data** pane

2. **Modeling** tab → **New Column**

3. Type:

   ```DAX
   Full Name = CUSTOMER[FIRST_NAME] & " " & CUSTOMER[LAST_NAME]
   ```

4. Press **Enter**

### Create Employee Full Name

1. Click on `EMPLOYEE` table

2. **Modeling** tab → **New Column**

3. Type:

   ```DAX
   Full Name = EMPLOYEE[FIRST_NAME] & " " & EMPLOYEE[LAST_NAME]
   ```

4. Press **Enter**

### Create Revenue in Invoice Line

1. Click on `INVOICE_LINE` table

2. **Modeling** tab → **New Column**

3. Type:

   ```DAX
   Revenue = INVOICE_LINE[UNIT_PRICE] * INVOICE_LINE[QUANTITY]
   ```

4. Press **Enter**

------

## Step 4: Create Key Measures

Now we'll create the measures needed for our KPIs and charts.

### Customer Metrics

1. **Total Customers**:

   - Right-click `CUSTOMER` table → **New Measure**

   - Type:

     ```DAX
     Total Customers = DISTINCTCOUNT(CUSTOMER[CUSTOMER_ID])
     ```

2. **Customer Lifetime Value**:

   - Right-click `CUSTOMER` table → **New Measure**

   - Type:

     ```DAX
     Customer Lifetime Value = DIVIDE(    SUM(INVOICE_LINE[Revenue]),    [Total Customers])
     ```

3. **Countries Served**:

   - Right-click `CUSTOMER` table → **New Measure**

   - Type:

     ```DAX
     Countries Served = DISTINCTCOUNT(CUSTOMER[COUNTRY])
     ```

4. **Avg Orders per Customer**:

   - Right-click `INVOICE` table → **New Measure**

   - Type:

     ```DAX
     Avg Orders per Customer = DIVIDE(    COUNTROWS(INVOICE),    [Total Customers])
     ```

### Revenue Metrics

1. **Total Revenue**:

   - Right-click `INVOICE_LINE` table → **New Measure**

   - Type:

     ```DAX
     Total Revenue = SUM(INVOICE_LINE[Revenue])
     ```

> [!tip]
>
> Format your measures for better readability: Right-click measure → Format → Choose currency or number format.

------

## Step 5: Create KPI Cards

Create 4 KPI cards across the top of the dashboard.

### KPI Card 1: Total Customers

1. Click blank area on canvas
2. Select **Card** visual from Visualizations pane
3. Drag `Total Customers` measure → **Fields** well
4. Resize and position in top-left corner
5. **Format the card**:
   - **Callout value**:
     - Font size: **28**
   - **Category label**:
     - Text: "Total Customers"
     - Font size: **10**

### KPI Card 2: Average Lifetime Value

1. Click blank area
2. Select **Card** visual
3. Drag `Customer Lifetime Value` measure → **Fields**
4. Position next to first card
5. **Format**:
   - Right-click the measure in Fields well → Format → Currency → Decimal places: **0**
   - Update label: "Avg Lifetime Value"

### KPI Card 3: Countries Served

1. Click blank area
2. Select **Card** visual
3. Drag `Countries Served` measure → **Fields**
4. Position next to second card
5. Format label: "Countries Served"

### KPI Card 4: Avg Orders per Customer

1. Click blank area
2. Select **Card** visual
3. Drag `Avg Orders per Customer` measure → **Fields**
4. Position next to third card
5. **Format**:
   - Right-click measure → Format → Number → Decimal places: **1**
   - Label: "Avg Orders/Customer"

> [!tip]
>
> Select all 4 cards, then Format → Align → Align Top and Distribute Horizontally.

------

## Step 6: Create Customer Distribution Map

1. Click blank area below KPI cards
2. Select **Filled Map** (or **Map**) from Visualizations pane
3. Add fields:
   - `CUSTOMER[COUNTRY]` → **Location**
   - `Total Customers` measure → **Color saturation** (Filled Map) or **Size** (Map)
   - `Total Revenue` measure → **Tooltips**
4. Resize to take up about 60% width of canvas
5. **Format the map**:
   - **Title**: "Customer Distribution by Country"
   - **Data colors**:
     - Choose a color gradient (e.g., light to dark blue)
   - **Map settings**:
     - Style: **Aerial** or **Road**
   - **Zoom buttons**: Turn **On**

> [!note]
>
> If you see a warning about geographic data, click "Yes, use as geographic" to enable geocoding.

------

## Step 7: Create Customer Segmentation Treemap

1. Click blank area (to the right of map)
2. Select **Treemap** from Visualizations pane
3. Add fields:
   - `CUSTOMER[COUNTRY]` → **Category**
   - `Total Revenue` measure → **Values**
4. Position next to the map (remaining 40% width)
5. **Format the treemap**:
   - **Title**: "Customer Segmentation by Value"
   - **Data labels**: Turn **On**
   - **Category labels**: Turn **On**
   - **Data colors**: Choose a color scale

> [!tip]
>
> Treemaps are great for showing hierarchical data. Each rectangle's size represents the revenue from that country.

------

## Step 8: Create Top 10 Customers Table

1. Click blank area below the map and treemap
2. Select **Table** visual
3. Add fields (in this order):
   - `CUSTOMER[Full Name]` → **Columns**
   - `CUSTOMER[COUNTRY]` → **Columns**
   - `Total Revenue` measure → **Columns**
4. **Add Top N Filter**:
   - Select the table
   - In **Filters** pane, find `Full Name` under "Filters on this visual"
   - Change filter type to **Top N**
   - Set to show **Top 10**
   - By value: drag `Total Revenue` measure
   - Click **Apply filter**
5. Resize to take up about 50% width of row
6. **Format the table**:
   - **Title**: "Top 10 Customers by Lifetime Value"
   - **Style**: **Minimal**
   - **Grid**:
     - Horizontal dividers: **On**
     - Vertical dividers: **Off**
     - Row padding: **8**
   - **Column headers**:
     - Font size: **12**
     - Bold: **On**
   - **Values**:
     - Total Revenue: Format as currency

------

## Step 9: Create Top Cities Table

1. Click blank area next to customers table
2. Select **Table** visual
3. Add fields:
   - `CUSTOMER[CITY]` → **Columns**
   - `Total Customers` measure → **Columns**
   - `Total Revenue` measure → **Columns**
4. **Add Top N Filter**:
   - Filter `CITY` to **Top 10**
   - By value: `Total Customers`
   - Apply filter
5. Resize to remaining 50% width
6. **Format the table**:
   - **Title**: "Top Cities by Customer Count"
   - Format similar to customers table
   - Sort by Total Customers (descending)

------

## Step 10: Create Support Rep Performance Bar Chart

1. Click blank area below tables
2. Select **Clustered Bar Chart** from Visualizations pane
3. Add fields:
   - `EMPLOYEE[Full Name]` → **Y-axis**
   - `Total Revenue` measure → **X-axis**
4. **Filter to support representatives only**:
   - In **Filters** pane, find `EMPLOYEE[TITLE]`
   - Select filter type: **Basic filtering**
   - Check boxes for titles containing "Sales Support Agent" or similar
   - Apply filter
5. Resize to take up about 60% width
6. **Format the chart**:
   - **Title**: "Revenue by Support Representative"
   - **Data labels**: Turn **On**
   - **X-axis**:
     - Title: "Total Revenue"
     - Display units: **Thousands (K)**
   - **Y-axis**:
     - Title: Turn **Off**
   - **Bars**:
     - Choose a color

> [!note]
>
> If you're not sure which titles to filter for, check the EMPLOYEE table in Table View to see the job titles.

------

## Step 11: Create Support Rep Performance Table

1. Click blank area next to bar chart
2. Select **Table** visual
3. Add fields:
   - `EMPLOYEE[Full Name]` → **Columns**
   - `Total Revenue` measure → **Columns**
   - `Total Customers` measure → **Columns** (to show how many customers they support)
4. **Apply same filter** as bar chart (support reps only)
5. Resize to remaining 40% width
6. **Format the table**:
   - **Title**: "Support Rep Performance"
   - **Style**: **Minimal**
   - Sort by Total Revenue (descending)
   - Format Total Revenue as currency

------

## Step 12: Optional - Create Customer Segmentation by Country/City

If you have extra space, add this bonus visual:

1. Click blank area
2. Select **Stacked Column Chart**
3. Add fields:
   - `CUSTOMER[COUNTRY]` → **X-axis**
   - `Total Customers` → **Y-axis**
4. **Add drill-down capability**:
   - Click the chart
   - In the Fields well, add `CUSTOMER[CITY]` below COUNTRY
   - This creates a hierarchy
   - Click the drill-down arrow in the visual header
5. Format as desired

------

## Step 13: Final Formatting and Polish

1. **Add a report title**:
   - **Insert** tab → **Text box**
   - Type: "Customer Analytics & Geographic Distribution"
   - Format: Font size **20**, Bold, Center align
   - Position at very top of page
2. **Apply consistent colors**:
   - Use a color theme (green/teal to match customer focus)
   - Select visuals → Format → Data colors → Choose theme
3. **Add page background** (optional):
   - Click blank area on canvas
   - **Format** → **Canvas background**
   - Color: Light gray (#F3F2F1) or light green (#F0F8F0)
4. **Align all visuals**:
   - Use Format → Align tools
   - Ensure consistent spacing
5. **Add borders to cards** (optional):
   - Select KPI cards
   - Format → General → Effects → Background → Turn **On**
   - Add subtle shadow or border
6. **Test interactions**:
   - Click on a country in the map
   - Verify all visuals filter properly
   - Click on a support rep name
   - See customers filter by that rep

------

## Step 14: Save Your Work

1. **File** → **Save As**
2. Name: `Chinook_Customer_Dashboard.pbix`
3. Click **Save**

------

## Expected Results

When complete, your dashboard should show:

- 4 KPI cards showing customer metrics
- A filled map showing customer distribution by country
- A treemap showing customer segmentation by revenue
- A table of top 10 customers with their countries and lifetime value
- A table of top 10 cities by customer count
- A bar chart showing revenue by support representative
- A table showing support rep performance metrics

**Total visuals**: 9 (4 cards + 5 charts/tables)

------

## Common Issues and Solutions

### Issue: Employee Relationship Shows Wrong Cardinality

**Solution**: The relationship between CUSTOMER[SUPPORT_REP_ID] and EMPLOYEE[EMPLOYEE_ID] should be Many-to-One (*:1). Check in Model View.

### Issue: Customer Lifetime Value Shows Very Large Numbers

**Solution**: Make sure you're dividing by Total Customers, not summing individual customer values. Use the DIVIDE function as shown in the DAX formula.

### Issue: Map Shows Wrong Locations

**Solution**: Some countries may have different names in your data (e.g., "USA" vs "United States"). Create a calculated column to standardize country names if needed.

### Issue: Support Reps Don't Show in Visuals

**Solution**: Check that SUPPORT_REP_ID in CUSTOMER table has values and matches EMPLOYEE_ID in EMPLOYEE table. NULL values won't show relationships.

### Issue: Top 10 Filter Doesn't Work

**Solution**: Make sure you're filtering by the correct field and using the measure in "By value" field. The filter should be on the dimension (Full Name), sorted by the measure (Total Revenue).

------

## Bonus Challenges

If you finish early, try these enhancements:

1. **Add a date slicer** to filter by invoice date range
2. **Create a drill-through page** - right-click a customer name to see their individual purchase history
3. **Add a scatter plot** showing customers by Total Revenue (X) vs Total Orders (Y)
4. **Create customer segments** using DAX:
   - High Value: >$45
   - Medium Value: $40-45
   - Low Value: <$40
5. **Add conditional formatting** to the Top 10 Customers table - highlight top 3 in green
6. **Create a new measure** for "Average Revenue per Order" by customer

------

## Advanced: Customer Cohort Analysis

If you want to go deeper, create a cohort analysis:

1. Create a calculated column for **First Purchase Month**:

   ```DAX
   First Purchase Month = 
   CALCULATE(
       MIN(INVOICE[INVOICE_DATE]),
       ALLEXCEPT(CUSTOMER, CUSTOMER[CUSTOMER_ID])
   )
   ```

2. Create a **Months Since First Purchase** calculated column in INVOICE:

   ```DAX
   Months Since First = 
   DATEDIFF(
       RELATED(CUSTOMER[First Purchase Month]),
       INVOICE[INVOICE_DATE],
       MONTH
   )
   ```

3. Create a matrix visual showing cohort retention by month

------

## What You Learned

✅ How to build customer-focused analytics dashboards ✅ Working with customer and employee tables ✅ Creating geographic visualizations with filled maps ✅ Using treemaps for hierarchical data ✅ Filtering visuals with Top N filters ✅ Analyzing sales representative performance ✅ Creating customer lifetime value calculations ✅ Building relationships between fact and dimension tables

------

## Comparing Dashboard 1 vs Dashboard 2

| Aspect          | Dashboard 1                  | Dashboard 2                          |
| --------------- | ---------------------------- | ------------------------------------ |
| **Focus**       | Product/Sales Analysis       | Customer Analysis                    |
| **Tables**      | 7 tables (product-centric)   | 4 tables (customer-centric)          |
| **Key Metrics** | Revenue, tracks sold, genres | Customers, lifetime value, geography |
| **Visuals**     | Line charts, donut charts    | Maps, treemaps, bar charts           |
| **Use Case**    | What are we selling?         | Who is buying from us?               |

Both dashboards work together to give a complete picture of the music store business!

------