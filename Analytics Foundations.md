# Analytics Foundations
### The "Why" Behind Everything We Build in This Course

---

## Why This Document Exists

Power BI is a tool. This document is about the thinking that makes the tool useful.

Before you build a single chart, it helps to understand what kind of problem you're solving, why visualization matters, and how to choose the right way to show data. These concepts apply whether you're using Power BI, Tableau, Excel, or a whiteboard.

---

## The 4 Types of Analytics

Every analytics question falls into one of four categories. They form a progression — each type is more powerful but also harder to execute than the one before it.

```
DESCRIPTIVE  →  DIAGNOSTIC  →  PREDICTIVE  →  PRESCRIPTIVE
   What          Why did          What will      What should
 happened?      it happen?        happen?        we do?
────────────────────────────────────────────────────────────────
  Easiest                                          Hardest
  Most common                              Less common in BI
```

### Descriptive Analytics — *What happened?*
Summarizes historical data. The vast majority of dashboards are descriptive.

> "Total revenue was $2.3M last quarter. USA was our largest market at 22% of sales. January had the lowest revenue of the year."

**Tools:** Power BI, Tableau, Excel pivot tables, SQL reports
**This course:** Everything you build in Days 1 and 2 is primarily descriptive analytics.

---

### Diagnostic Analytics — *Why did it happen?*
Digs into descriptive results to find root causes. This is where drill-through, filters, and cross-filtering become essential — you're letting the data analyst chase "why."

> "Revenue was down in Q3. Drilling into the data: it was concentrated in one region. That region had a supply chain disruption. Genre sales were also down — specifically rock and pop."

**Tools:** Power BI drill-through, filters, Q&A, comparative charts (YoY)
**This course:** The drill-through activity, the YoY measures, and the cross-filtering behavior all enable diagnostic analysis.

---

### Predictive Analytics — *What will happen?*
Uses statistical models and machine learning to forecast future outcomes.

> "Based on the last 3 years of sales patterns, Q4 revenue is projected at $680K ± $40K."

**Tools:** Python (scikit-learn), R, Azure ML, Power BI AI visuals (limited)
**This course:** Not covered — requires statistical modeling beyond BI scope.

---

### Prescriptive Analytics — *What should we do?*
Goes beyond prediction to recommend specific actions, often using optimization algorithms.

> "Given current inventory levels, demand forecast, and supplier lead times — reorder 450 units of Product X from Supplier B by Thursday."

**Tools:** Operations research software, advanced ML, specialized platforms
**This course:** Not covered.

---

### Where Power BI Fits

Power BI excels at **descriptive and diagnostic** analytics — the two most common types in business settings. A well-built Power BI dashboard can answer both "what happened?" and "why did it happen?" efficiently, making it the right tool for the majority of real-world analytics needs.

---

## Why Visualization Matters

Data in a table communicates numbers. A well-chosen visualization communicates *meaning*.

### Anscombe's Quartet — A Classic Demonstration

Four datasets with nearly identical statistical summaries (same mean, variance, and correlation):

| Dataset | Mean X | Mean Y | Variance X | Variance Y | Correlation |
|---|---|---|---|---|---|
| I | 9.0 | 7.5 | 11.0 | 4.12 | 0.816 |
| II | 9.0 | 7.5 | 11.0 | 4.12 | 0.816 |
| III | 9.0 | 7.5 | 11.0 | 4.12 | 0.816 |
| IV | 9.0 | 7.5 | 11.0 | 4.12 | 0.816 |

Look identical in a table. But plotted as scatter charts, they're completely different: one is linear, one is curved, one has an outlier, one is a vertical line with an outlier. The statistics lie; the visualization tells the truth.

**The lesson:** Summary numbers hide patterns. Visualization reveals them.

### Pre-attentive Attributes

The human visual system processes certain properties *before* conscious attention — faster than reading. These are called **pre-attentive attributes**:

| Attribute | Example use in dashboards |
|---|---|
| **Color** | Red = bad, green = good; highlight the outlier |
| **Size** | Larger bubble = higher revenue |
| **Position** | Higher bar = more value |
| **Shape** | Different icon for different category |
| **Length** | Bar chart — length encodes the value |

Good dashboard design uses pre-attentive attributes intentionally. Bad design uses color randomly, misaligns sizes, or requires the reader to re-read the legend on every glance.

---

## Visual Vocabulary — Choosing the Right Chart

One of the most common mistakes in data visualization is choosing a chart type based on familiarity rather than the message you're trying to convey. The question to ask first is always: **"What relationship am I showing?"**

The [Tableau Visual Vocabulary](https://www.tableau.com/solutions/gallery/visual-vocabulary) is an excellent reference that categorizes chart types by communication purpose. Here are the most relevant categories for business dashboards:

---

### Change Over Time
*"How has this metric changed?"*

**Best charts:** Line chart, area chart, slope chart
**Use when:** Your X-axis is a date/time variable and you want to show trends, seasonality, or trajectories.
**In this course:** Revenue trend line charts, YTD cumulative area charts.

> ✅ Line chart for continuous trends
> ✅ Bar chart if the time periods are discrete and few (e.g., comparing 4 quarters)
> ❌ Pie chart — terrible for showing change over time

---

### Ranking
*"How do these items compare in order?"*

**Best charts:** Horizontal bar chart (sorted), dot plot
**Use when:** You want to show "top N" items or compare relative performance.
**In this course:** Top 10 customers, top artists by revenue, revenue by support rep.

> ✅ Always sort your bars — an unsorted bar chart is almost useless
> ✅ Horizontal bars when labels are long (country names, product names)
> ❌ Pie chart — hard to rank visually beyond the top 2–3 slices

---

### Part-to-Whole
*"What share does each piece represent?"*

**Best charts:** Treemap, 100% stacked bar, donut chart (small number of categories only)
**Use when:** The total is meaningful and you want to show composition.
**In this course:** Customer Segmentation Treemap (revenue share by country), Genre donut chart.

> ✅ Treemap when you have many categories and want to show relative size
> ✅ Donut/pie only when you have ≤5 categories and proportions are very different
> ❌ Pie chart with 12+ slices — unreadable

---

### Distribution
*"How are values spread across a range?"*

**Best charts:** Histogram, box plot, violin plot
**Use when:** You want to understand the range, spread, concentration, or outliers in a dataset.
**In this course:** Not covered explicitly — Power BI supports histograms but they require binning.

> ✅ Histogram for "how many customers have revenue in each $10 bracket?"
> ✅ Box plot for comparing distributions across groups

---

### Correlation
*"Do these two variables move together?"*

**Best charts:** Scatter chart, bubble chart
**Use when:** You have two (or three) numeric variables and want to see if they're related.
**In this course:** Scatter chart bonus challenge (revenue vs. orders per customer).

> ✅ Scatter chart for 2 variables; bubble for 3 (size = third variable)
> ❌ Line chart — that's for time series, not correlation

---

### Spatial / Geographic
*"Where is this happening?"*

**Best charts:** Filled map (choropleth), bubble map, shape map
**Use when:** The geographic dimension is meaningful to the analysis.
**In this course:** Customer Distribution by Country (bubble map), Sales by State.

> ✅ Filled (choropleth) map when every region has a value and you want shading
> ✅ Bubble map when you want to show intensity at specific points
> ❌ Map by default — if geography doesn't add insight, a bar chart is cleaner

---

### Deviation / Comparison Against a Baseline
*"How far above or below normal is this?"*

**Best charts:** Bar/column chart with a reference line, bullet chart, waterfall
**Use when:** You're comparing actuals to a target, budget, or prior period.
**In this course:** Year-over-year growth % chart (deviation from 0%).

---

## Storytelling With Data

A dashboard is not a data dump. It's a story with a beginning (context), middle (insight), and end (action).

### The 3-Question Test

Before presenting any visualization, ask:

1. **So what?** — What is the most important thing a viewer should take away?
2. **Compared to what?** — Every number needs a reference point. "$500K revenue" means nothing without context. "$500K, up 18% vs. last year" means something.
3. **What should we do?** — The best dashboards lead to decisions. If a viewer walks away without knowing what to do differently, the dashboard failed.

---

### The Attention Hierarchy

Viewers scan dashboards in a predictable order:
1. **Title** — sets the context
2. **Large numbers / KPI cards** — the headline metrics
3. **The biggest visual** — usually top-left or largest element
4. **Tables last** — tables require reading, not scanning

Design your dashboards to put the most important insight first, biggest, and most prominently positioned.

---

### Declutter

Every element that doesn't help the viewer understand the data is visual noise. Ruthlessly remove:
- Gridlines that aren't necessary
- Axis labels that repeat what the visual already shows
- Legends when the bars are already labeled
- 3D effects on any chart (they distort perception)
- Decorative colors that don't encode meaning

The goal is not to make the dashboard pretty — it's to make the insight obvious.

---

### Color as Communication

In a dashboard, color should do one of three things:
1. **Distinguish** — different categories (use distinct colors that aren't similar)
2. **Represent a value** — sequential scale (light → dark) for magnitude, diverging scale for above/below baseline
3. **Draw attention** — highlight the one thing that matters most

If you're using a different color for every bar because "it looks nice," you're using color decoratively, not communicatively.

---

## Putting It Together: The Design Checklist

Before you call a dashboard "done," ask:

- [ ] Does the title describe what this dashboard is for?
- [ ] Is the most important metric the most visually prominent?
- [ ] Is every chart type appropriate for the data relationship it's showing?
- [ ] Are all charts sorted in a meaningful direction?
- [ ] Does color encode meaning, or is it just decoration?
- [ ] Would a new viewer understand the dashboard in 30 seconds?
- [ ] Does each page answer a specific question?

---

## Further Reading

- **[Tableau Visual Vocabulary](https://www.tableau.com/solutions/gallery/visual-vocabulary)** — The definitive reference for choosing the right chart type for your message. Bookmark this.
- **Storytelling with Data** by Cole Nussbaumer Knaflic — The most practical book on data visualization for business analysts.
- **The Visual Display of Quantitative Information** by Edward Tufte — The academic foundation of data visualization principles.
- **[From Data to Viz](https://www.data-to-viz.com/)** — An interactive decision tree for chart selection.

---

*These principles apply to every visualization you build in this course and beyond it. The tool changes; the thinking doesn't.*
