# DAX Fundamentals Lab


**When to do this:** After connecting to Snowflake — before building Dashboard 1

---

## Why This Lab Exists

In Dashboard 1 you'll write DAX formulas to power your visualizations. This lab gives you the conceptual foundation first — so you understand *what you're writing and why*, instead of just copying formulas.

By the end of this lab you'll understand:
- The difference between a **calculated column** and a **measure** (the single most important concept in DAX)
- What **filter context** means and why it matters
- How **CALCULATE** changes a calculation's context
- When to use **DIVIDE** instead of `/`

You'll use the Chinook tables you already loaded from Snowflake. All exercises are quick — type a formula, see the result, understand why it works.

---

## Part 1: Calculated Columns vs. Measures

This is the foundation of DAX. If you understand this, everything else follows.

### The Core Difference

| | Calculated Column | Measure |
|---|---|---|
| **When computed** | Once, when you create it | Every time a visual renders |
| **Stored where** | Inside the table (takes memory) | Not stored — computed on demand |
| **Works row-by-row** | ✅ Yes | ❌ No — aggregates across rows |
| **Can be used in filters/slicers** | ✅ Yes | ❌ No |
| **Can respond to slicers** | ❌ No | ✅ Yes — this is their superpower |

**Simple mental model:**
> Calculated column = "add a new column to this table, fill it in for every row"
> Measure = "compute one number that summarizes many rows, and update it whenever the user changes a filter"

---

### Exercise 1A — Create a Calculated Column

We'll add a `Revenue` column to `INVOICE_LINE` that computes unit price × quantity for every row.

1. In the **Data pane** (right side), click on `INVOICE_LINE`
2. Go to **Modeling** tab → **New Column**
3. In the formula bar, type:

```dax
Revenue = INVOICE_LINE[UNIT_PRICE] * INVOICE_LINE[QUANTITY]
```

4. Press **Enter**
5. Switch to **Table View** (second icon in the left sidebar) — make sure `INVOICE_LINE` is selected

> **What you should see:** A new `Revenue` column on the right side of the table. Every row has a value — the column was computed once across all rows when you pressed Enter. The values should be small dollar amounts (e.g., $0.99, $1.98, $3.96).

**Why use a column here and not a measure?**
Because you want revenue on *every individual line item* — as a permanent value you can filter by, sort by, and roll up. Measures can't live inside a row.

---

### Exercise 1B — Create a Measure

Now create a measure that *sums* all those Revenue values.

1. Right-click `INVOICE_LINE` in the Data pane → **New Measure**
2. Type:

```dax
Total Revenue = SUM(INVOICE_LINE[Revenue])
```

3. Press Enter
4. Click somewhere on the **canvas** (Report View), add a **Card** visual, drag `Total Revenue` into it

> **What you should see:** The card shows a single number — approximately **$2,328**. This is the sum of every revenue line across the entire database.

Now do something important: **add a slicer** for `CUSTOMER[COUNTRY]`. Filter to just **USA**.

> **What happens:** The card updates to ~$523. The measure recalculated based on what the slicer filtered. This is filter context in action.

Remove the slicer filter (click the eraser icon) and the card returns to ~$2,328.

**The key insight:** The measure doesn't "store" $2,328. It stores the *formula*, and recalculates its answer every time the filter context changes. The calculated column stores its result permanently — slicers don't change it.

---

## Part 2: Filter Context

Filter context is the set of filters currently active when a measure calculates. It comes from three places:
- Slicers the user has set
- Filters applied to the visual (e.g., rows in a table)
- Cross-filtering from other visuals being clicked

### Exercise 2 — See Filter Context in Action

1. Add a **Table** visual to the canvas
2. Drag in: `CUSTOMER[COUNTRY]` and `Total Revenue`
3. Look at the result

> **What you should see:** Each country row shows a *different* Total Revenue value. But it's the *same formula* (`SUM(INVOICE_LINE[Revenue])`) — the measure is recalculating itself once per row, with each row adding "COUNTRY = this row's country" to the filter context.

This is why measures are powerful: one formula, infinite contexts.

Now add `Total Customers` as a third column (you need to create this measure first):

```dax
Total Customers = DISTINCTCOUNT(CUSTOMER[CUSTOMER_ID])
```

> **What you should see:** The table now shows customers and revenue per country. USA should show 13 customers and ~$523 in revenue.

---

## Part 3: The CALCULATE Function

`CALCULATE` is DAX's most important function. It lets you *change or override the filter context* inside a measure.

**Syntax:**
```dax
CALCULATE( <expression>, <filter1>, <filter2>, ... )
```

It evaluates `<expression>` but first applies the filter(s) you specify — either adding to, modifying, or replacing the current filter context.

### Exercise 3A — Calculate with a specific filter

Create this measure (right-click any table → New Measure):

```dax
USA Revenue = CALCULATE(
    SUM(INVOICE_LINE[Revenue]),
    CUSTOMER[COUNTRY] = "USA"
)
```

Add it to a **Card** visual.

> **What you should see:** ~$523, always — even if you filter the page to a different country. The measure *ignores* the page filter for COUNTRY and always computes USA. This is CALCULATE overriding the filter context.

Now add it to the country table from Exercise 2. Drag it alongside `Total Revenue`.

> **What you should see:** Every row in the table shows the same ~$523 in the USA Revenue column — because the measure always forces COUNTRY = USA, regardless of what the row context says.

**When would you actually use this?**
When you need a fixed comparison value — e.g., "show this country's revenue vs. USA's revenue" in the same table.

---

### Exercise 3B — CALCULATE with ALL (removing a filter)

```dax
All Countries Revenue = CALCULATE(
    SUM(INVOICE_LINE[Revenue]),
    ALL(CUSTOMER[COUNTRY])
)
```

Add to the same table.

> **What you should see:** Every row shows the same total — ~$2,328. The `ALL()` function *removes* the country filter from the context, so every row computes the grand total.

**Practical use:** This is how you compute a "% of total" column in a table:

```dax
Revenue % of Total = DIVIDE(
    SUM(INVOICE_LINE[Revenue]),
    CALCULATE(SUM(INVOICE_LINE[Revenue]), ALL(CUSTOMER[COUNTRY]))
)
```

Try creating that measure and adding it to the table. Format it as a percentage.

> **What you should see:** Each country shows its revenue as a percentage of $2,328. USA should show ~22.5%.

---

## Part 4: DIVIDE — Safe Division

DAX's `/` operator crashes with a "divided by zero" error if the denominator is 0. In business data, zeros happen constantly (a new month with no sales, a region with no customers, etc.).

`DIVIDE(numerator, denominator, [alternate_result])` handles this gracefully.

```dax
-- Risky:
Customer Lifetime Value RISKY = SUM(INVOICE_LINE[Revenue]) / DISTINCTCOUNT(CUSTOMER[CUSTOMER_ID])

-- Safe:
Customer Lifetime Value = DIVIDE(
    SUM(INVOICE_LINE[Revenue]),
    DISTINCTCOUNT(CUSTOMER[CUSTOMER_ID]),
    0   -- returns 0 if denominator is 0, instead of an error
)
```

**Rule of thumb:** Always use `DIVIDE` instead of `/` in measures. The optional third argument lets you control what shows when division by zero occurs — use `0` for numeric KPIs, `BLANK()` when you want nothing to show.

---

## Part 5: Quick Reference — When to Use Each

Use this as a decision guide when you write your own formulas:

```
Do you need the value on every row of a table?
    YES → Calculated Column
    NO  ↓

Do you need to filter/slice by this value?
    YES → Calculated Column
    NO  ↓

Do you need it to update dynamically when a user clicks a slicer?
    YES → Measure
    NO  → Either works, but prefer a Measure (saves memory)
```

---

## Cleanup Before Dashboard 1

Before moving on:

1. **Delete** the Card, Table, and Slicer visuals you created during this lab (select each → Delete key)
2. **Keep** these items — you'll need them for Dashboard 1:
   - ✅ `INVOICE_LINE[Revenue]` calculated column
   - ✅ `Total Revenue` measure
   - ✅ `Total Customers` measure

> **You may also keep** `USA Revenue`, `All Countries Revenue`, and `Revenue % of Total` for reference — or delete them if you want a clean slate.

---

## What You Just Learned

| Concept | What it means |
|---|---|
| **Calculated Column** | Row-by-row permanent value stored in the table |
| **Measure** | Dynamic formula that recalculates in every filter context |
| **Filter Context** | The set of active filters when a measure runs |
| **CALCULATE** | The function that lets you override or extend the filter context |
| **DIVIDE** | Safe division that handles divide-by-zero gracefully |

You're ready for Dashboard 1. The formulas you'll write there are direct applications of everything you just practiced.
