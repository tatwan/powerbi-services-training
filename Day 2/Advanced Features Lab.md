# Advanced Features Lab — Bookmarks, Navigation & Natural Language


**When to do this:** After completing the Challenge activity

---

## Overview

This lab covers three features that transform a functional report into a polished, interactive experience:

1. **Bookmarks** — capture the state of a report page and attach actions to buttons
2. **Page Navigation** — let users move between pages using visual buttons instead of the tab bar
3. **Q&A Visual** — let users ask questions about the data in plain English

These are features you'll see in almost every production Power BI report, but they're rarely taught in beginner courses. By the end of this lab you'll have used all three.

---

## Part 1: Bookmarks

A bookmark captures the exact current state of a report page — which filters are active, which visuals are visible, which slicers are set. You can then attach that saved state to a button so that clicking the button restores it.

### The most practical use case: a collapsible filter panel

Many dashboards have a filter panel on the right that users can show or hide. You'll build that here.

---

### Step 1: Set up the filter panel

Open your **Customer Analytics dashboard** (the challenge file, or Dashboard 1).

1. **Insert** tab → **Text Box** → type "Filters" → format it as a panel header (white text, teal background, bold)
2. Add 2–3 slicers to a group on the right side of the canvas:
   - `CUSTOMER[COUNTRY]` slicer
   - `Date[Year]` slicer (if you completed the Time Intelligence lab)
3. Position them in a column on the far-right side of the canvas, stacked vertically
4. Add a button: **Insert** → **Buttons** → **Blank** → label it **"✕ Close"** — place it at the top of the filter panel

Your canvas should now have a filter panel group on the right side.

---

### Step 2: Create two bookmarks

Open the **View** tab → **Bookmarks** → the Bookmarks pane appears on the right.

**Bookmark 1 — "Filters Open":**
1. Make sure the filter panel (slicers + close button + header) is **visible** on canvas
2. In the Bookmarks pane, click **Add** → rename the bookmark to `Filters Open`

**Bookmark 2 — "Filters Closed":**
1. Select the filter panel items (the slicers, the header, the close button) → right-click → **Group** them
2. With the group selected, go to **Format** pane → **General** → toggle **Visible** to **Off** (the panel disappears)
3. In Bookmarks pane → **Add** → rename to `Filters Closed`
4. Turn the group visibility back **On** (so the canvas returns to normal for editing)

---

### Step 3: Wire up the buttons

**The "Open Filters" button** — this lives on the main canvas when the filter panel is hidden:

1. **Insert** → **Buttons** → **Blank** → label it **"⚙ Filters"**
2. Position it in the top-right corner of the canvas
3. With the button selected → **Format** pane → **Action** section → toggle **On**
4. Action Type: **Bookmark** / Bookmark: **Filters Open**

**The "Close" button** — this is inside the filter panel:

1. Click the **"✕ Close"** button you added earlier
2. **Format** pane → **Action** → **On**
3. Action Type: **Bookmark** / Bookmark: **Filters Closed**

---

### Step 4: Test the interaction

Hold **Ctrl** and click **"⚙ Filters"** button.

> **What you should see:** The filter panel appears (the group becomes visible). Now Ctrl+click **"✕ Close"** — the panel hides again. This is how production dashboards manage layout without changing pages.

> **Why Ctrl+click?** In Edit mode, you hold Ctrl to trigger button actions. Published reports (or in Presentation mode) respond to regular clicks.

---

## Part 2: Page Navigation Buttons

The page tabs at the bottom of Power BI are functional but visually plain. Most polished dashboards replace them with custom navigation buttons embedded in a consistent header or sidebar. Here's how.

---

### Step 1: Design a navigation bar

On your **Dashboard 1** page:

1. Add a thin rectangle at the very top of the canvas (using **Insert → Shapes → Rectangle**)
2. Fill it with your dashboard color (teal) — this is your nav bar background
3. Make it span the full canvas width, about 30px tall

---

### Step 2: Add navigation buttons

1. **Insert → Buttons → Blank** — create one button per page you want to navigate to
2. Label them: `Sales Overview` / `Revenue Trends` / `Country Detail`
3. Place them inside the nav bar rectangle, evenly spaced
4. Format each button:
   - No fill (transparent background)
   - White text
   - No border

**Wire up each button:**
1. Select a button → **Format** → **Action** → **On**
2. Action Type: **Page navigation**
3. Destination: select the target page

---

### Step 3: Copy the nav bar to all pages

1. Select all nav bar elements (rectangle + all buttons) → **Ctrl+C**
2. Go to each other page → **Ctrl+V** to paste
3. The buttons will be in the same position on every page, creating a consistent nav experience

> **What you should see:** Clicking any button navigates to that page. Users no longer need to use the tab bar at the bottom. This also means you can **hide the tab bar** in published reports (File → Options → Report settings → disable page navigation).

---

## Part 3: Q&A Visual — Natural Language Questions

The Q&A visual lets users type questions in plain English and Power BI generates the appropriate chart automatically. It's one of Power BI's most impressive demos and genuinely useful for exploratory analysis.

---

### Step 1: Add the Q&A visual

1. Go to any page of your report (or add a new page called `Explore`)
2. In the **Visualizations** pane, click the **Q&A** visual icon (it looks like a speech bubble with a question mark)
3. Resize it to take up most of the canvas

---

### Step 2: Ask some questions

Click inside the Q&A visual and type each of these questions — observe what Power BI generates:

| Type this question | What Power BI should show |
|---|---|
| `total revenue by country` | Bar chart of revenue by country |
| `top 10 customers by revenue` | Table or bar chart of top customers |
| `revenue by year as a line chart` | Line chart over time |
| `customers by country as a map` | Geographic map |
| `revenue in usa` | Single KPI value filtered to USA |
| `revenue by genre` | Chart broken out by music genre |

> **What makes this powerful:** Q&A understands your column names, table names, and measure names. It works because Power BI has indexed your data model. The better your column names (descriptive, readable), the better Q&A performs.

---

### Step 3: Improve Q&A with synonyms

Q&A may not recognize certain terms your users would naturally use. You can teach it synonyms.

1. With the Q&A visual selected → **Format** pane → scroll to **Q&A setup** → click **Review questions**
2. Or go to **Modeling** tab → **Q&A setup**
3. In the Q&A setup screen, you can add synonyms for field names:
   - "income" = `Total Revenue`
   - "clients" = `CUSTOMER`
   - "rep" = `EMPLOYEE[Full Name]`
4. Click **Submit** to save each synonym

Try typing `income by rep` — Q&A should now understand both terms.

---

### Step 4: Convert a Q&A result to a standard visual

When Q&A produces a chart you like, you can convert it to a permanent visual:

1. Ask a question that produces a good chart (e.g., `revenue by country as a map`)
2. In the top-right corner of the Q&A visual, click the **"Turn this result into a standard visual"** icon (looks like a bar chart with an arrow)
3. The Q&A visual is replaced by a regular Map visual that you can format normally

> **Practical use:** Q&A is a great way to quickly prototype a visual you're not sure how to build. Generate it with natural language, convert it, then format it.

---

## Summary: What You Learned

| Feature | What it does | When to use it |
|---|---|---|
| **Bookmarks** | Capture and restore page state | Show/hide panels, saved views, guided story navigation |
| **Bookmark Actions** | Attach bookmarks to buttons | "Toggle filters", "Reset slicers", "Go to this view" |
| **Page Navigation buttons** | Custom navigation between pages | Replace tab bar with a branded header nav |
| **Q&A Visual** | Natural language querying of the data model | Exploratory analysis, non-technical users, report prototyping |
| **Q&A Synonyms** | Teach Q&A business vocabulary | When field names differ from how users talk about data |

---

## Going Further

- **Personalized bookmarks** (Power BI Service): users can save their own bookmark states without modifying the report — useful for teams where different people want different default views
- **Report tooltips**: create a report page, set it as a tooltip page, and visuals will show it on hover — like a mini dashboard inside a tooltip
- **Mobile layout**: View tab → Mobile layout — redesign the report for phone screens using the same visuals
