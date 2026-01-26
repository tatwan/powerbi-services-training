# PowerBI Desktop (Day 1)

## Learning Objectives

By the end of Day 1, you will be able to:

- Understand the difference between Power BI Desktop and Power BI Service
- Navigate the Power BI Desktop interface confidently
- Import data from CSV files and create basic visualizations (column, bar, and line charts)
- Explain what a data model is and why it matters
- Use Power Query to clean and transform data (remove rows, change data types, split columns, handle errors)
- Create relationships between multiple tables
- Build interactive reports with slicers and filters

------

## Power BI Desktop vs Power BI Service

### What is Power BI Desktop?

Power BI Desktop is a **free Windows application** you install on your computer. It's the primary authoring tool for creating Power BI reports, and it works completely offline.

**Download**: https://powerbi.microsoft.com/desktop

### Desktop vs Service: Key Differences

| Feature               | Power BI Desktop              | Power BI Service (Web)                      |
| --------------------- | ----------------------------- | ------------------------------------------- |
| **Cost**              | Completely free               | Free tier with limitations; Pro for sharing |
| **Installation**      | Windows desktop app           | Web browser (no install)                    |
| **Working Mode**      | Offline, saved as .pbix files | Online, stored in cloud workspaces          |
| **Power Query**       | ✅ Full functionality          | ✅ Full functionality                        |
| **Visualizations**    | ✅ Full functionality          | ✅ Full functionality                        |
| **Data Modeling**     | ✅ Full functionality          | ✅ Full functionality                        |
| **Sharing Reports**   | Email .pbix files             | Publish and share via web links             |
| **Scheduled Refresh** | ❌ Manual only                 | ✅ Scheduled refresh (Pro/Premium)           |
| **Dashboards**        | ❌ Not available               | ✅ Available                                 |
| **Mobile Access**     | ❌ Windows only                | ✅ Access from any device                    |

> [!important] **For this course, we use Power BI Desktop**. All the core skills you learn (data transformation, modeling, DAX, visualizations) work identically in both Desktop and Service. If you later get Power BI Pro, you can simply publish your .pbix files to the Service.

### What is a .pbix File?

Unlike the Service (which stores data models and reports separately), Power BI Desktop saves everything in a single file with the `.pbix` extension:

- Data model (tables, relationships)
- Power Query transformations
- DAX measures and calculations
- All report pages and visualizations
- Formatting and themes

Think of it like an Excel workbook - one file contains everything.

------

## Installing Power BI Desktop

### System Requirements

- Windows 10 or Windows 11
- .NET Framework 4.8 or later
- At least 4 GB RAM (8 GB recommended)
- 2.5 GB available disk space

### Installation Steps

1. Go to https://powerbi.microsoft.com/desktop
2. Click **Download Free**
3. Choose:
   - **Microsoft Store** (recommended - auto-updates)
   - **Download Center** (standalone .exe installer)
4. Run the installer and follow prompts
5. Launch Power BI Desktop from Start menu

> [!tip] The Microsoft Store version automatically updates when new features are released. The standalone installer requires manual updates.

------

## Power BI Desktop Interface Tour

When you first open Power BI Desktop, you'll see three main views accessible from the left sidebar:

### Main Views (Left Sidebar Icons)

| Icon              | View               | Purpose                                            |
| ----------------- | ------------------ | -------------------------------------------------- |
| 📊 **Report View** | Report canvas      | Create visualizations and design report pages      |
| 📋 **Table View**  | Data grid          | View and verify loaded data (formerly "Data View") |
| 🔗 **Model View**  | Data model diagram | Create and manage table relationships              |

### Report View Components

When in Report view, you'll work with:

**Top Ribbon**

- **Home**: Common tasks (Get Data, Save, Publish)
- **Insert**: Add text boxes, images, shapes, buttons
- **Modeling**: Create measures, calculated columns, manage relationships
- **View**: Show/hide panes, gridlines, page navigation
- **Optimize**: Performance analyzer
- **Help**: Documentation and support

**Right Panes**

1. **Filters**: Page-level, visual-level, and report-level filters
2. **Visualizations**: Chart types and visual formatting options
3. **Data**: Fields from your data model

> [!note]
>
> You can show/hide any pane from the **View** tab. Common workflow: maximize canvas space by hiding Filters pane, show it only when needed.

### Understanding the Canvas

The white central area is your **report canvas** where you design pages:

- Each report can have multiple pages (tabs at bottom)
- Drag visuals to resize and position them
- Grid and snap-to-grid help with alignment

------

# Part 1: Creating Your First Report

## Step 1: Getting the Data

1. **Open Power BI Desktop**
2. **Get Data**:
   - Click **Home** tab → **Get Data** → **Text/CSV**
   - Navigate to and select `ny_property_sales.csv`
   - Click **Open**
3. **Preview Window Appears**:
   - You'll see a preview of the data
   - Power BI auto-detects data types
   - For now, click **Load** to import the data as-is

> [!note]
>
> When you click **Load**, Power BI loads the data directly. If you click **Transform Data** instead, it opens Power Query Editor for cleaning (we'll do this in Part 2).

## Step 2: Verify Data Loaded

1. Click the **Table View** icon (left sidebar, middle icon)
2. You should see the `ny_property_sales` table
3. Review the columns and data types
4. Click back to **Report View** (top icon in left sidebar)

## Step 3: Creating Visualizations

Now let's create three charts on your report canvas.

### Chart 1: Total Sales By Area (Stacked Column Chart)

1. Click on a blank area of the canvas (to deselect any existing visual)
2. From the **Visualizations** pane (right side), click the **Stacked Column Chart** icon
3. A blank chart appears on the canvas
4. From the **Data** pane (far right), find the `ny_property_sales` table and:
   - Drag `Area` to the **X-axis** well
   - Drag `SALES PRICE` to the **Y-axis** well (it automatically sums)
5. Resize and position the chart

**Formatting (Optional)**:

- Click the chart to select it
- In **Visualizations** pane, click the **Format visual** tab (paint roller icon)
- Expand **Visual** → **Columns** → **Colors** to change bar color
- Expand **General** → **Title** to rename the chart (e.g., "Total Sales by Area")

### Chart 2: Top 10 Building Class Category by Sales (Stacked Bar Chart)

1. Click blank area of canvas
2. Select **Stacked Bar Chart** from Visualizations pane
3. Add fields:
   - `BUILDING CLASS CATEGORY` → **Y-axis**
   - `SALES PRICE` → **X-axis** (auto-sums)
4. **Add a Top N Filter**:
   - Select the bar chart
   - In **Filters** pane, under **Filters on this visual**, find `BUILDING CLASS CATEGORY`
   - Change filter type to **Top N**
   - Set to show **Top 10**
   - Drag `SALES PRICE` to the **By value** field
   - Click **Apply filter**
5. Format as desired (colors, title)

### Chart 3: Sales by Month (Line Chart)

1. Click blank area of canvas
2. Select **Line Chart** from Visualizations pane
3. Add fields:
   - `SALE DATE` → **X-axis**
   - Power BI creates a date hierarchy (Year > Quarter > Month > Day)
   - Click the dropdown on `SALE DATE` in the X-axis well and select **Month** (remove hierarchy)
   - `SALES PRICE` → **Y-axis**
4. Format as desired

## Step 4: Save Your Work

1. **File** → **Save**
2. Choose a location and name your file (e.g., `NY_Property_Sales.pbix`)
3. Click **Save**

> [!tip]
>
> Unlike the Service (which auto-saves), Desktop requires manual saves. Save frequently with **Ctrl+S**.

## Step 5: Test Interactivity

1. Click on a bar in one of your charts
2. Notice the other charts filter/highlight based on your selection
3. This is **cross-filtering** - it happens automatically in Power BI
4. Click the bar again (or click empty space) to deselect

------

# Part 2: Working with Sales 2017 Data

Now we'll work with messy, real-world data and use **Power Query** to clean it.

## The Dataset

- **sales2017_raw.csv**: Sales data from an e-commerce store (messy, needs cleaning)
- **stores.csv**: Store location information
- **products.csv**: Product details

**Goal**: Visualize sales by category, promotions, time trends, and delivery metrics.

## Step 1: Import the Data Files

### Import sales2017_raw.csv

1. **Home** → **Get Data** → **Text/CSV**
2. Select `sales2017_raw.csv`
3. In the preview window, click **Transform Data** (NOT Load)
   - This opens **Power Query Editor**

> [!important]
>
> Clicking **Transform Data** opens Power Query Editor where you can clean data before loading it into the model. This is where we'll spend most of our time.

### Power Query Editor Interface

When Power Query opens, you'll see:

**Left Pane**: Queries (list of tables you're working with) **Center**: Data preview with column headers **Right Pane**:

- **Query Settings**: Name of current query and Applied Steps
- **Properties**: Query-level settings

**Bottom Options**:

- **Diagram View**: Visual representation of query relationships
- **Data View**: The preview table (current view)
- **Schema View**: Column names and types only

**Ribbon Tabs**:

- **Home**: Common transformations (Remove rows, Split column, Data type)
- **Transform**: Column operations (Replace values, Format text, Extract)
- **Add Column**: Create new columns (Custom, Conditional, Index)
- **View**: Show/hide panes, advanced options

### Understanding Applied Steps

In the **Query Settings** pane (right), you'll see **Applied Steps**:

```
Source
Promoted Headers
Changed Type
```

Each step represents a transformation. You can:

- Click a step to see the result at that point
- Delete a step (X icon)
- Reorder steps (drag and drop)
- Edit a step (gear icon)

> [!warning]
>
> Be careful deleting or reordering steps - later steps may depend on earlier ones. Power BI will warn you if there's a dependency issue.

## Step 2: Clean the Sales Data

Let's transform this messy data step by step.

### A. Enable Data Profiling

1. In Power Query, go to **View** tab
2. Check:
   - ☑️ **Column quality** (shows valid/error/empty percentages)
   - ☑️ **Column distribution** (shows value frequency)
   - ☑️ **Column profile** (shows detailed stats)
3. At bottom, change "Column profiling based on" from **Top 1000 rows** → **Entire dataset**

This helps you spot data quality issues at a glance.

### B. Remove Empty Rows

1. Look at the data - you'll see rows with all blank values
2. **Home** tab → **Remove Rows** → **Remove Blank Rows**
3. Notice a new step appears in Applied Steps: "Removed Blank Rows"

### C. Promote First Row to Headers

The first row contains column names, not data:

1. **Home** tab → **Use First Row as Headers**
2. Column names update
3. A step "Promoted Headers" is created

### D. Change Data Types

Power BI auto-detects types, but let's verify:

1. Look at the column headers - the icon shows the detected type:
   - `ABC` = Text
   - `123` = Whole Number
   - `1.2` = Decimal
   - `📅` = Date
2. To change a data type:
   - Click the icon next to the column name
   - OR: Right-click column → **Change Type**
   - OR: **Home** tab → **Data Type** dropdown

**For our sales data, ensure:**

- `order_date` → **Date**
- `ship_date` → **Date**
- `revenue` → **Decimal Number** or **Fixed Decimal Number**
- `quantity` → **Whole Number**
- `store_id`, `product_id` → **Text** (even if numeric - these are IDs, not measures)

> [!tip] 
>
> **Why keep IDs as Text?** Product IDs like "001" and "1" are different. As text, they're preserved. As numbers, they'd both become 1.

### E. Handle Locale Issues in Revenue Column

The `revenue` column may have formatting issues (e.g., commas, currency symbols).

1. Right-click the `revenue` column
2. **Replace Values**:
   - Value to Find: `,`
   - Replace With: (leave blank)
   - Click **OK**
3. Ensure data type is **Decimal Number**

If you still see errors:

- Right-click column → **Replace Errors**
- Enter `0` or `null` (or investigate errors first with **Keep Errors**)

### F. Trim and Clean Text Columns

Text fields may have extra spaces:

1. Select the `status` column
2. Hold **Ctrl** and click `category` and `sub_category` to multi-select
3. **Transform** tab → **Format** → **Trim** (removes leading/trailing spaces)
4. Also apply **Clean** if needed (removes non-printable characters)

### G. Split the Product ID Column (Example)

If your `product_id` looks like "CAT-001", you might want to extract the category prefix:

1. Right-click `product_id` column
2. **Split Column** → **By Delimiter**
3. Select delimiter: `-` (dash)
4. Choose **At the left-most delimiter**
5. Click **OK**
6. Rename new columns: `product_category`, `product_number`

> [!note] This is just an example. Only do this if your data structure requires it.

### H. Rename the Query

In **Query Settings** pane (right), under **Properties**:

- Change **Name** from `sales2017_raw` to `sales` (cleaner name)

## Step 3: Import Stores and Products Tables

### Import stores.csv

1. In Power Query Editor: **Home** → **New Source** → **Text/CSV**
2. Select `stores.csv`
3. Click **OK** (opens in Power Query)
4. Clean if needed:
   - Promote headers if needed
   - Change data types (ensure `store_id` is **Text**)
5. Leave the query name as `stores`

### Import products.csv

1. **Home** → **New Source** → **Text/CSV**
2. Select `products.csv`
3. Clean if needed:
   - Promote headers
   - Ensure `product_id` is **Text**
   - Ensure `price` is **Decimal Number**
4. Leave query name as `products`

## Step 4: Load Data into Power BI Desktop

1. In Power Query Editor, click **Home** → **Close & Apply**
2. Power BI loads all three queries into your data model
3. You're back in Report View

> [!tip] If you need to edit queries later: **Home** tab → **Transform Data** reopens Power Query Editor.

## Step 5: Create Relationships Between Tables

Now we need to connect the three tables.

### Switch to Model View

1. Click the **Model View** icon (left sidebar, bottom icon)
2. You'll see three tables: `sales`, `stores`, `products`
3. Power BI may have auto-created relationships - we'll verify

### Understanding Relationships

Relationships connect tables through common columns (keys):

- **One-to-Many (1:\*)**: One product can appear in many sales (most common)
- **Many-to-One (\*:1)**: Many sales can reference one product (same as above, different perspective)
- **One-to-One (1:1)**: Rare in real-world data
- **Many-to-Many (\*:\*)**: Avoid if possible; can cause unexpected behavior

**Relationship Line Indicators:**

- `1` on one end, `*` on the other = One-to-Many
- Solid line = Active relationship
- Dashed line = Inactive relationship

### Create sales → products Relationship

1. Drag `product_id` from the `sales` table onto `product_id` in the `products` table
2. A line appears connecting them
3. Double-click the line to verify:
   - **Cardinality**: Many to One (*:1) - many sales rows can reference one product
   - **Cross filter direction**: Single (from products to sales)
   - **Make this relationship active**: ☑️ Checked
4. Click **OK**

### Create sales → stores Relationship

1. Drag `store_id` from `sales` to `store_id` in `stores`
2. Verify the relationship settings (should be Many-to-One)

### Verify Relationships

Your model should now look like a **star schema**:

- **Fact table** (center): `sales` (transactional data)
- **Dimension tables** (around it): `products`, `stores` (lookup/descriptive data)

> [!important] **Why relationships matter**: Without relationships, you can't create charts like "Sales by Product Category" because Power BI doesn't know how to link sales data to product data.

------

## Step 6: Build Visualizations

Now that data is clean and related, let's create meaningful visuals.

### Chart 1: Total Revenue by Category (Clustered Column Chart)

1. Switch to **Report View** (top icon in left sidebar)
2. Click blank canvas area
3. Select **Clustered Column Chart** from Visualizations pane
4. Add fields:
   - From `products` table: `category` → **X-axis**
   - From `sales` table: `revenue` → **Y-axis**
5. Power BI automatically sums revenue by category (because of the relationship!)

**Format**:

- Rename title: "Total Revenue by Category"
- Change bar colors if desired
- Add data labels: **Format visual** → **Visual** → **Data labels** → On

### Chart 2: Revenue Over Time (Line Chart)

1. Select **Line Chart**
2. Add fields:
   - `order_date` → **X-axis** (from `sales` table)
   - Expand the date hierarchy and keep only **Month**
   - `revenue` → **Y-axis**
3. Format:
   - Title: "Revenue Trend by Month"
   - Enable markers: **Format visual** → **Visual** → **Markers** → On

### Chart 3: Top 10 Products by Revenue (Stacked Bar Chart)

1. Select **Stacked Bar Chart**
2. Add fields:
   - From `products`: `product_name` → **Y-axis**
   - From `sales`: `revenue` → **X-axis**
3. Add Top N Filter:
   - In **Filters** pane → **Filters on this visual**
   - Find `product_name`
   - Filter type: **Top N**
   - Show **Top 10**
   - By value: drag `revenue` field here
   - Apply filter
4. Format as desired

### Chart 4: Sales by Store Location (Table Visual)

1. Select **Table** visual
2. Add fields from `stores` table:
   - `city`
   - `state`
3. Add field from `sales` table:
   - `revenue` (automatically sums)
   - `quantity` (automatically sums)
4. Sort by revenue (descending): Click the `revenue` column header

------

## Step 7: Add Slicers for Interactivity

**Slicers** are visual filters that let users interact with the report.

### Add a Category Slicer

1. Click blank area of canvas
2. Select **Slicer** from Visualizations pane
3. From `products` table, drag `category` to the **Field** well
4. Position at top-left of report
5. Test: Click "Beverages" → all visuals update to show only beverages data

### Add a Year Slicer

1. Add another **Slicer**
2. Drag `order_date` from `sales` to Field well
3. Expand the hierarchy and keep only **Year**
4. Change slicer style to **Dropdown**:
   - Select the slicer
   - Click dropdown arrow (⌄) on the slicer visual
   - Select **Dropdown**
5. Position next to category slicer

### Add a Store State Slicer

1. Add another **Slicer**
2. From `stores`, drag `state` to Field well
3. Keep as list or change to **Tile** style for visual buttons

------

## Step 8: Formatting and Polish

### Page Background

1. Click blank area (deselect all visuals)
2. **Visualizations** pane → **Format page** (paint roller)
3. **Canvas background** → Change color or add image

### Align Visuals

1. Select multiple visuals: Hold **Ctrl** and click each
2. **Format** tab → **Align** → Choose alignment option
   - **Align Left**: Line up left edges
   - **Distribute Horizontally**: Space evenly

### Add a Title

1. **Insert** tab → **Text box**
2. Type: "Sales Data Analysis - 2017"
3. Format (increase font size, bold, center align)
4. Position at top of page

### Add a Page

1. At the bottom, click **+** to add a new page
2. Name it (right-click tab → Rename)
3. Build additional visuals on page 2

------

## Step 9: Save Your Work

1. **File** → **Save As**
2. Name: `Sales_Data_Analysis.pbix`
3. Save to your preferred location

------

## Understanding the Data Model (Deeper Dive)

### What is a Data Model?

In Power BI Desktop, the **data model** is:

- The tables you've loaded
- The relationships between them
- Any calculated columns or measures (DAX formulas)

Everything lives in the `.pbix` file.

### Fact vs Dimension Tables

**Fact Tables** (Transactional):

- Contain measurable events (sales, orders, clicks)
- Usually have foreign keys to dimension tables
- Examples: `sales`, `orders`, `transactions`
- Often have large row counts

**Dimension Tables** (Descriptive):

- Contain attributes/properties
- Provide context to facts
- Examples: `products`, `customers`, `stores`, `dates`
- Usually smaller row counts

**Star Schema** (Recommended):

- Fact table in center
- Dimension tables around it
- Simple, fast performance

### Calculated Columns vs Measures (Preview)

While we haven't created these yet, here's the difference:

| Calculated Column                  | Measure                                      |
| ---------------------------------- | -------------------------------------------- |
| Row-by-row calculation             | Aggregation across rows                      |
| Stored in the table                | Calculated on-the-fly                        |
| Uses disk space                    | No storage cost                              |
| Example: `Profit = Revenue - Cost` | Example: `Total Sales = SUM(Sales[Revenue])` |

> [!note] We'll cover **DAX** (Data Analysis Expressions) for creating measures in Day 2. For now, focus on using the built-in aggregations (Sum, Average, Count).

------

## Data Refresh in Power BI Desktop

### How Refresh Works in Desktop

Unlike the Service (which can schedule automatic refreshes), Desktop requires **manual refresh**:

1. Click **Home** → **Refresh** button
2. Or press **F5**
3. Power BI re-runs all Power Query steps and reloads data

### When to Refresh

- When source CSV files have changed
- After editing Power Query transformations
- Before publishing to Service (ensure latest data)

### Data Sources and Refresh

**Static Files (CSV, Excel)**:

- Power BI stores the file path
- If file moves, refresh will fail (you'll need to update the path)

**Databases (SQL, etc.)**:

- Refresh pulls latest data from the live connection
- Requires credentials to remain valid

> [!tip] To change a data source path: **Home** → **Transform Data** → In Power Query, click the gear icon next to "Source" step in Applied Steps.

------

## Exporting and Sharing (Desktop)

### Export Options

From any report page:

**File Menu**:

- **Export to PDF**: Creates a PDF of the current page
- **Export to PowerPoint**: Each page becomes a slide
- **Print**: Print current page

**Visual-Level Export**:

- Click a visual → Three dots (⋯) → **Export data**
- Saves underlying data as CSV or Excel

### Sharing the .pbix File

Since we're not using Power BI Service, you can share reports by:

1. **Email the .pbix file**:
   - Recipients need Power BI Desktop installed (free)
   - They open the file and interact with it
   - File size can be large if lots of data
2. **Save to shared network drive**:
   - Team members access the same file location
   - Only one person can edit at a time (like Excel)
3. **Publish to Service** (if you get Pro later):
   - **Home** → **Publish**
   - Choose workspace
   - Creates web link others can access

> [!important] When sharing .pbix files, ensure data sources are accessible to recipients. If using a file path like `C:\MyData\sales.csv`, the recipient won't have access unless they have the same file in the same location.

------

## Power Query: Additional Concepts (Reference)

### Handling Errors

When transformations fail on some rows:

**Error indicator**: Red "Error" text in cells **Column quality bar**: Shows percentage of errors (top of column when profiling is on)

**Common error handling:**

- **Replace Errors**: Right-click column → Replace Errors → Enter replacement value (e.g., `0` or `null`)
- **Remove Errors**: Right-click column → Remove Errors (deletes rows with errors)
- **Keep Errors**: Right-click column → Keep Errors (useful for investigating what's breaking)

### Append Queries

Combine tables with the **same structure** (like stacking data):

**Example**: Combining monthly sales files

1. **Home** → **Append Queries**
2. Select tables to combine
3. Creates one unified table

**When to use**: Multiple files with identical columns (Jan_Sales, Feb_Sales, Mar_Sales)

### Merge Queries

Join tables on a key column (like SQL JOIN):

**Example**: Adding customer details to orders

1. **Home** → **Merge Queries**
2. Select matching columns from each table
3. Choose join type:
   - **Left Outer**: Keep all rows from left table
   - **Inner**: Only matching rows
   - **Full Outer**: All rows from both
4. Expand the merged column to bring in desired fields

> [!important] **Merge vs Relationships**: Use **Merge** in Power Query when you want to denormalize data (flatten into one table). Use **Relationships** in the model for star schema (keeps tables separate, better performance for large datasets).

### Conditional Columns

Add a column with if/then/else logic:

1. **Add Column** → **Conditional Column**
2. Define conditions:
   - If `revenue` > 100 then "High"
   - Else if `revenue` > 50 then "Medium"
   - Else "Low"

**When to use**: Categorizing data (Revenue Tiers, Age Groups, Performance Ratings)

### Group By (Aggregations)

Roll up data by categories:

1. **Transform** → **Group By**
2. Select grouping columns (e.g., `category`)
3. Add aggregation (e.g., Sum of `revenue`)
4. Result: One row per category with total revenue

**When to use**: Pre-aggregating data before loading (reduces model size)

------

## Organizing Your Work

### File Naming Conventions

Since everything is in `.pbix` files, use clear names:

**Good**:

- `Sales_Analysis_2017.pbix`
- `Customer_Dashboard_v2.pbix`
- `Inventory_Report_Jan2025.pbix`

**Bad**:

- `Report.pbix`
- `Final.pbix`
- `Test123.pbix`

### Version Control

Desktop doesn't have built-in version control like the Service, so:

**Option 1**: Manual versions

- `Sales_Report_v1.pbix`
- `Sales_Report_v2.pbix`
- Keep old versions in an archive folder

**Option 2**: Use cloud storage with version history

- OneDrive/SharePoint (auto-versioning)
- Google Drive
- Dropbox

**Option 3**: Git/Source control (advanced)

- Use `.pbix` as binary files
- Or use **Power BI Projects** (PBIP format, new feature) for text-based version control

> [!tip] Before making major changes, **Save As** a new version. Desktop doesn't have an undo history across sessions.

------

## Day 1 Summary

Today we covered:

| Topic                  | Key Takeaways                                                |
| ---------------------- | ------------------------------------------------------------ |
| **Desktop vs Service** | Desktop is free, offline, saves as .pbix files; all core functionality is identical |
| **Interface**          | Report View (design), Table View (verify data), Model View (relationships) |
| **Getting Data**       | Import CSV via Get Data; use **Load** for clean data, **Transform Data** for messy data |
| **Power Query**        | Data transformation tool: remove rows, change types, split columns, clean text, handle errors |
| **Data Model**         | Tables connected via relationships; fact tables (center) + dimension tables (around) |
| **Relationships**      | Connect tables on key columns (e.g., product_id); enables cross-table analysis |
| **Visualizations**     | Column charts, bar charts, line charts, tables - drag fields to wells, format as needed |
| **Slicers**            | Interactive filters on the canvas; let users filter the entire report |
| **Applied Steps**      | Each Power Query transformation is a step; can edit, delete, reorder |
| **Refresh**            | Manual only in Desktop (F5); re-runs Power Query and updates data |

### Key Files Created Today

1. `NY_Property_Sales.pbix` - First report with basic charts
2. `Sales_Data_Analysis.pbix` - Multi-table model with relationships and slicers

------

## Practice Exercises (Homework)

1. **Add a new chart** to the Sales Data Analysis report:
   - Create a pie chart showing revenue by sub_category
   - Add a slicer for `promotion` (Yes/No)
2. **Create a new page**:
   - Add visuals showing delivery performance:
     - Average days from order to ship (need to create a calculated column: `ship_date - order_date`)
     - Count of orders by status
3. **Explore Power Query**:
   - Add a custom column that calculates `profit = revenue * 0.3` (assuming 30% margin)
   - Try grouping the sales data by `category` and `year` to create a summary table
4. **Experiment with visual interactions**:
   - Add a new page
   - Create 3 visuals
   - Test cross-filtering behavior
   - Try **Format** → **Edit interactions** to control which visuals filter each other

------

## Next Steps: Day 2 Preview

Tomorrow we'll dive into:

- **DAX (Data Analysis Expressions)**: Create custom measures and calculated columns
- **Time Intelligence**: Year-over-year growth, running totals, moving averages
- **Advanced Visuals**: Drill-down, drill-through, tooltips, bookmarks
- **Performance Optimization**: Reduce file size, improve query speed

------

## Additional Resources

- [Power BI Desktop Download](https://powerbi.microsoft.com/desktop)
- [Microsoft Learn: Power BI Documentation](https://learn.microsoft.com/en-us/power-bi/)
- [Power Query M Function Reference](https://learn.microsoft.com/en-us/powerquery-m/power-query-m-function-reference)
- [Power BI Community Forums](https://community.powerbi.com/)
- [DAX Guide](https://dax.guide/) (for Day 2)

------

## Troubleshooting Common Issues

### Data Won't Load

**Problem**: Error when clicking Load or Close & Apply **Solutions**:

- Check if CSV file still exists at the original path
- Verify data types don't cause conflicts (e.g., text in a number column)
- Look at error messages in Applied Steps - click the failed step

### Relationship Won't Create

**Problem**: Can't drag to create relationship **Solutions**:

- Ensure both columns are the same data type (both Text, or both Number)
- Check if relationship already exists (can only have one active relationship per column pair)
- Verify there are matching values in both columns

### Visual Doesn't Show Data

**Problem**: Chart is blank even though data exists **Solutions**:

- Check Filters pane - may be filtered to empty results
- Verify the field is actually in the correct well (X-axis vs Y-axis)
- Ensure relationships exist between tables if using fields from multiple tables

### File Size Too Large

**Problem**: .pbix file is huge (>100 MB) **Solutions**:

- Remove unnecessary columns in Power Query (before loading)
- Filter out historical data you don't need
- Use Import mode instead of DirectQuery if applicable
- Check for large text/image columns

### Can't Find a Feature

**Problem**: Interface looks different from screenshots **Solutions**:

- Check which View you're in (Report/Table/Model) - features differ by view
- Some options only appear when a visual is selected
- Ribbons collapse on smaller screens - click the small arrow to expand groups

------

