# 🧮 Tableau Calculated Fields Skill
**A Claude AI skill by [DataMonster](https://Data.Monster) — Pablo Gomez**
*Tableau Visionary · Ambassador · London, UK*

> Write, review, and debug Tableau calculated fields using DataMonster's standards — LOD expressions, table calculations, date logic, NULL handling, naming conventions, and performance rules. All in one skill.

---

## What this skill does

Load this skill and Claude becomes your Tableau formula co-pilot. It knows:

- **DataMonster's calculation standards** — formatting, naming conventions, style rules
- **LOD decision logic** — when to use FIXED vs INCLUDE vs EXCLUDE, and when not to use LODs at all
- **Table calculation patterns** — running totals, period-over-period, rank, percent of total
- **Date calculations** — DATETRUNC, DATEADD, fiscal year logic, relative date comparisons
- **NULL and division safety** — never returns a broken calc
- **Performance guidelines** — knows when a calc will be slow and suggests alternatives

---

## How to use it

### In claude.ai chat ✅ Recommended for most users

> **Requires:** Free claude.ai account at [claude.ai](https://claude.ai)

1. Open a new conversation on [claude.ai](https://claude.ai)
2. Click **Raw** on this file → select all → copy
3. Paste into the chat with:
   > *"I'm going to give you a skill to follow for this conversation."* + paste
4. Ask for your calculated field — see example prompts below

### In Claude Code

> **Requires:** Claude Pro or Max subscription

1. Save this file to `.claude/skills/tableau-calculated-fields-skill.md` in your project
2. Reference it at the start of a session:
   > *"Read `tableau-calculated-fields-skill.md` then help me write a calculated field for..."*

---

## Example prompts

```
Write a calculated field for rolling 3-month average of sales.
Fields available: [Order Date] (date), [Sales] (float). One row per order line.
```

```
I need a customer retention rate. A customer is retained if they ordered
in both the current and previous calendar year.
Fields: [Customer ID], [Order Date], [Sales].
```

```
Write a FIXED LOD that calculates each customer's first order date,
then flag any order that is their first purchase.
```

```
My profit margin calc is returning nulls when Sales is zero.
How do I null-protect it? Calc: [Profit] / [Sales]
```

```
I need a date calc for fiscal year, where the fiscal year starts in April.
```

---

## Quick Reference Cheatsheet

| Task | Function |
|------|----------|
| Truncate date | `DATETRUNC('month', [Date])` |
| Add period | `DATEADD('month', 3, [Date])` |
| Diff between dates | `DATEDIFF('day', [Start], [End])` |
| Null-safe divide | `ZN([N]) / NULLIF([D], 0)` |
| Running total | `RUNNING_SUM(SUM([Field]))` |
| Rank | `RANK_DENSE(SUM([Field]), 'desc')` |
| Fixed total | `{FIXED [Dim] : SUM([Field])}` |
| String contains | `CONTAINS([Field], "text")` |
| Today's date | `TODAY()` |
| Conditional flag | `IF [Metric] >= [Target] THEN "On Track" ELSE "Off Track" END` |

---

## About DataMonster

These skills were created by **Pablo Gomez**, known in the DataFam as **DataMonster** — a Tableau Visionary and Tableau Ambassador, Data Storyteller, and public speaker based in London, UK. Passionate about applying AI responsibly to make data stories clearer and more human.

🔗 [data.monster](https://Data.Monster) · [LinkedIn](https://www.linkedin.com/in/pablolgomez) · [Tableau Public](https://public.tableau.com/app/profile/pablolgomez)

> Shared freely with the DataFam community. Use it, adapt it, build on it — a shoutout is always appreciated but never required. That's the DataFam way. 💙

---
---

<!--
  ============================================================
  CLAUDE INSTRUCTIONS — Everything below is for Claude only.
  Human readers can stop here.
  ============================================================
-->

## Purpose

This skill guides Claude when writing, reviewing, or debugging Tableau calculated fields. Always read this file before generating any Tableau formula or calculation.

---

## DataMonster's Calculation Standards

### General Rules
- Always use **UPPERCASE** for Tableau functions (e.g., `SUM`, `IF`, `FIXED`, `DATETRUNC`)
- Use **lowercase** for field names wrapped in square brackets (e.g., `[sales]`, `[order date]`)
- Add inline comments using `//` for any formula longer than 2 lines
- Prefer **readable multi-line formatting** over one-liners for complex calcs
- Never use `IIF()` — always use `IF / THEN / ELSE / END`

### Naming Conventions
| Prefix | Use for |
|--------|---------|
| `_` (underscore) | Helper / intermediate calculations not shown in viz |
| `calc_` | Core business metrics |
| `flag_` | Boolean true/false fields |
| `grp_` | Grouping / binning calculations |
| `str_` | String formatting calculations |
| `dt_` | Date calculations |

---

## LOD Expressions

### FIXED — Use when you need a value anchored to a specific dimension, regardless of view filters
```
// Total sales per customer, regardless of date filter in view
{ FIXED [Customer ID] : SUM([Sales]) }
```

### INCLUDE — Use when you need a finer granularity than the view
```
// Average order value per customer, then averaged in the view
{ INCLUDE [Order ID] : SUM([Sales]) }
```

### EXCLUDE — Use when you need a coarser granularity than the view
```
// Category total, even when subcategory is in the view
{ EXCLUDE [Sub-Category] : SUM([Sales]) }
```

### DataMonster's LOD Decision Rules
1. **FIXED** is the default — use it unless you specifically need the view's level of detail
2. **Never nest LODs** if a table calc can solve it — LODs hit the data source, table calcs run in memory
3. When using FIXED with dimension filters, remember filters run AFTER fixed LODs unless the dimension is in context

---

## Table Calculations

### Preferred patterns
```
// Running total
RUNNING_SUM(SUM([Sales]))

// Period-over-period % change
(SUM([Sales]) - LOOKUP(SUM([Sales]), -1)) / ABS(LOOKUP(SUM([Sales]), -1))

// Rank (dense, descending)
RANK_DENSE(SUM([Sales]), 'desc')

// Percent of total
SUM([Sales]) / TOTAL(SUM([Sales]))
```

### Table Calc Rules
- Always specify `COMPUTE USING` explicitly in the UI — don't rely on defaults
- For PoP comparisons, prefer `LOOKUP()` over `ZN(LOOKUP())` unless nulls are a known issue
- Avoid deeply nested table calcs — break into separate calculated fields if possible

---

## Date Calculations

### Standard date truncations
```
// Fiscal year start in July (adjust month as needed)
DATETRUNC('year', DATEADD('month', -6, [Order Date]))

// Week starting Monday
DATETRUNC('week', [Order Date] + 1) - 1

// Day of week label
DATENAME('weekday', [Order Date])
```

### Preferred date comparison pattern
```
// Is this period? (use as filter or flag)
DATETRUNC('month', [Order Date]) = DATETRUNC('month', TODAY())
```

### Date Field Rules
- Always use `DATETRUNC()` for grouping, not `DATEPART()` (DATEPART returns integers, not dates)
- For relative date filters in calcs, anchor to `TODAY()` not `NOW()` unless time-of-day matters

---

## Conditional Logic

### Preferred IF pattern
```
IF [Profit] > 0 THEN "Profitable"
ELSEIF [Profit] = 0 THEN "Break Even"
ELSE "Loss"
END
```

### Preferred CASE pattern (for fixed dimension members)
```
CASE [Region]
  WHEN "North" THEN "Northern Division"
  WHEN "South" THEN "Southern Division"
  ELSE "Other"
END
```

### Rules
- Use `CASE` when matching exact dimension values — cleaner than a chain of `ELSEIF`
- Use `IF` when conditions involve comparisons, ranges, or multiple fields
- Always include an `ELSE` clause — never leave it implicit

---

## String Calculations

### Common patterns DataMonster uses
```
// Concatenate with separator
[First Name] + " " + [Last Name]

// Trim and uppercase for matching
UPPER(TRIM([Product Name]))

// Extract domain from email
MID([Email], FIND([Email], "@") + 1, LEN([Email]))

// Pad number with leading zeros
IF LEN(STR([Order Number])) < 5
THEN STR(0) + STR([Order Number])
ELSE STR([Order Number])
END
```

---

## NULL Handling

```
// Replace null with zero
ZN([Sales])

// Replace null with string
IFNULL([Region], "Unknown")

// Check for null before dividing
IF ISNULL([Denominator]) OR [Denominator] = 0
THEN NULL
ELSE [Numerator] / [Denominator]
END
```

### Rules
- Prefer `ZN()` over `IFNULL([field], 0)` for brevity on numeric fields
- **Always null-protect divisions** — never divide without checking for zero/null denominator
- Return `NULL` (not 0 or "N/A") when a value genuinely doesn't exist — let the viz handle null display

---

## Performance Guidelines
1. **FIXED LODs** are expensive — avoid on high-cardinality dimensions
2. **String functions** on large extracts are slow — pre-process in data source where possible
3. **Nested IFs** over 4 levels should be refactored into helper calculations
4. **Table calcs** are generally faster than LODs for in-memory extracts
5. When a calc is used 3+ times in a workbook, extract it into its own named field

---

## How to Ask Claude for Help

When asking for a calculated field, provide:
1. **What you're trying to measure** (e.g., "rolling 3-month average of sales")
2. **Available fields** and their data types
3. **The granularity** of your data source (row = one order line? one transaction?)
4. **Any edge cases** to handle (nulls, zeroes, date gaps)

Example prompt:
> "Using this skill, write a calculated field for customer retention rate. I have [Customer ID], [Order Date] (date), and [Sales] (float). Each row is one order. A customer is retained if they ordered in both the current and previous calendar year."
