# European Football Transfers (Excel) — Top 5 Countries Visualization

## Visual Overview
<img width="709" height="324" alt="image" src="https://github.com/user-attachments/assets/3dca591b-a4f4-4c23-a27d-115c1d564ed7" />

---

## Case Description
A football analytics snapshot for Europe: identify the countries with the **highest average incoming transfer fees** (2022/2023) and compare their **volume of incoming transfers**. The goal is to give a hiring manager a quick, trustworthy view that ties directly back to the spreadsheet logic.

---

## Tasks
- Filter the raw FIFA-style transfer flow dataset to **European incoming transfers** for **2022/2023**.
- Aggregate by **incoming country**:
  - `# of incoming transfers`
  - `Average transfer fee` = `Total compensation ÷ # transfers`
- Rank countries by **Average transfer fee** and **select the Top 5**.
- Build a combo chart: **bars = # transfers**, **line = avg fee ($)**.
- Provide checks: totals and a weighted average.

---

## Accounting/Analytics Steps
1. **Source data (sheet `Database`)**  
   - Columns (after fixing headers): `Season`, `Transfers incoming`, `Continent` (incoming), `Transfers outgoing`, `Continent` (outgoing), `Number of transfers`, `Total club-to-club compensation`.

2. **Filter (sheet `European transfers`)**  
   - Keep rows where `Continent (incoming) = "Europe"` and `Season = "2022/2023"`.

3. **Aggregate by incoming country (sheet `European transfers by country`)**  
   - Sum `Number of transfers` and `Total compensation`.  
   - Compute `Avg fee` = `Total compensation / Number of transfers`.

4. **Pick Top 5 by `Avg fee` (sheet `Visualization Top 5 Countries`)**  
   - Countries: **England, France, Italy, Spain, Germany**.  
   - Numbers align 1:1 with the screenshot.

5. **Visualize**  
   - Excel combo chart with **secondary axis** for the line series (avg fee).

---

## Trial Balance / Data Summary
(Top 5 countries chosen by highest **average transfer fee** in 2022/2023. Values rounded to nearest dollar.)

| Country | # of incoming transfers | Average transfer fee ($) |
|---|---:|---:|
| England | 1,020 | 2,127,361 |
| Spain | 476 | 1,231,500 |
| France | 438 | 1,753,915 |
| Germany | 664 | 969,931 |
| Italy | 542 | 1,670,692 |
| **Totals / check** | **3,140** | — |

**Checks**
- **Weighted average fee (Top 5)** = **$1,615,881**  
  (computed from raw totals, not the simple mean of the five averages)
- **Implied total compensation (Top 5)** ≈ **$5,073,866,572**  
  (sum of each country’s `avg fee × # transfers`)

---

## Financial Statements / Results
- **Top by average fee**: England ($2.13m), France ($1.75m), Italy ($1.67m), Spain ($1.23m), Germany ($0.97m).
- **Volume context**: England leads by transfer count (1,020), Germany second (664).  
- **Insight**: The countries with highest **price-per-player** are not identical to those with highest **volume** (e.g., Spain/France outrank Germany on price, but not on volume).

---

## Mapping / Logic
- `Database` → filter (`Europe` incoming + `2022/2023`) → group by `Transfers incoming (country)`  
  → compute `# transfers`, `total $`, `avg $` → rank by `avg $` → **Top 5** → `Visualization Top 5 Countries` (combo chart).

**Key formulas (Excel)**
```excel
# of incoming transfers (per country):
=SUMIFS(Database[Number of transfers],
        Database[Transfers incoming], $A2,
        Database[Continent], "Europe",
        Database[Season], "2022/2023")

Total compensation (per country):
=SUMIFS(Database[Total club-to-club compensation],
        Database[Transfers incoming], $A2,
        Database[Continent], "Europe",
        Database[Season], "2022/2023")

Average transfer fee:
=[@Total_Comp] / [@Transfers]

Weighted average check (Top 5):
=SUMPRODUCT([Transfers],[Avg_Fee]) / SUM([Transfers])
```

## How I Built It

- **Tools:** Microsoft Excel (Tables, SUMIFS, data types, combo chart with secondary axis).
**Techniques:**
- Fixed multi-row headers (set header row to the proper line).
- Converted ranges to Excel Tables for structured references.
- Aggregation with SUMIFS / ratio metrics with division.
- Ranking using either SORT/TAKE (O365) or helper LARGE + INDEX/MATCH.
- Visualized with a clustered column + line combo chart and right-hand axis.
