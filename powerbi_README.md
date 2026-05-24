## Overview

An interactive 3-page Power BI dashboard tracking the financial performance of **15 Nifty 50 companies** across **5 sectors** from **FY2020 to FY2024**.

Built to replicate the kind of management reporting dashboard used daily at GCCs and financial services firms — with cross-filtering slicers, KPI cards, trend analysis, and a risk-return scatter plot.

---

## Live Dashboard

🔗 **[View Interactive Dashboard →](https://app.powerbi.com/view?r=eyJrIjoiODc1MmE4ZGUtNjU1Yi00YWU0LTg5MjItM2QxNDIxOTc4NGY0IiwidCI6IjdhOTVhZDY1LTU1ZWUtNDFlMy1iYTQ2LWFjYmRmNjA1ZWUzZCJ9)**


---

## Companies Covered

| Sector | Companies |
|---|---|
| IT | Infosys, TCS, Wipro |
| FMCG | Asian Paints, Hindustan Unilever, ITC |
| Auto | Maruti Suzuki, Tata Motors, Bajaj Auto |
| Banking | HDFC Bank, ICICI Bank, Kotak Mahindra |
| Pharma | Sun Pharma, Dr Reddy's, Cipla |

**Note on Banking sector:** Conventional EBITDA margin and Debt/Equity ratio are not meaningful for banks. The dashboard uses **Net Interest Margin (NIM)** in place of EBITDA margin and **Capital Adequacy Ratio (CAR)** in place of Debt/Equity for banking companies. This is documented in the data file's Notes sheet.

---

## Metrics Tracked

| Metric | Description |
|---|---|
| Revenue (₹ Cr) | Total sales / net revenue |
| EBITDA Margin % | Operating profit margin (NIM for banks) |
| Net Income (₹ Cr) | Profit after tax |
| ROE % | Return on equity |
| Debt/Equity | Financial leverage ratio (CAR for banks) |
| YoY Revenue Growth % | Year-on-year revenue growth |
| Net Income Margin % | Net profit as % of revenue |
| Sector Average ROE | Benchmark ROE for each sector by year |

---

## Dashboard Structure

### Page 1 — Sector Overview
High-level view of all 5 sectors with cross-filtering slicers.

**Visuals:**
- 4 KPI cards — Average Revenue, EBITDA Margin, ROE, D/E Ratio
- Sector slicer (dropdown) and Year slicer (range slider)
- Clustered bar chart — Revenue by company, colour coded by sector
- Line chart — EBITDA Margin trend FY2020–FY2024 by sector

**Purpose:** Answers "which sectors and companies are growing fastest and most profitably?"

---

### Page 2 — Company Deep Dive
Drill into any single company — select from dropdown and all 7 visuals update instantly.

**Visuals:**
- Company slicer (dropdown — filters entire page)
- 5 KPI cards — Revenue, EBITDA Margin, Net Income, ROE, D/E Ratio
- Combo chart — Revenue (bars) vs Net Income (line) FY2020–FY2024
- Column chart — YoY Revenue Growth % with conditional colour (green = growth, red = decline)
- Line chart — Net Income Margin % trend
- Line chart — Company ROE vs Sector Average ROE (benchmarking)
- Area chart — Debt/Equity trend with risk threshold line at 1.0
- Data table — all metrics by year for exact figures

**Purpose:** Answers "how is this specific company performing and is it beating its sector peers?"

---

### Page 3 — Sector Comparison
Side-by-side benchmarking across all 5 sectors.

**Visuals:**
- Clustered bar chart — EBITDA Margin by sector with data labels
- Scatter plot — ROE vs Debt/Equity (risk vs return) — each bubble = one company, colour = sector
- Matrix heat map — ROE for all 15 companies across 5 years, conditional formatted green to red

**Purpose:** Answers "which sectors offer the best risk-adjusted returns?"

---

## Key Insights

### 1. IT sector leads on margins but growth is slowing
TCS and Infosys consistently deliver 25–27% EBITDA margins — the highest of any sector. However revenue growth decelerated from 20%+ in FY2022 to single digits in FY2024 as global tech spending cooled. Wipro is the laggard — margins compressed to 17.5% by FY2024.

### 2. Auto sector — most dramatic reversal
Tata Motors went from ₹14,395 Cr net loss in FY2020 to ₹31,807 Cr profit in FY2024 — the biggest turnaround in the dataset. Driven by JLR recovery and EV momentum. Maruti and Bajaj show consistent, steady compounding — low D/E, rising ROE.

### 3. FMCG — defensive but pressured
ITC delivers the highest EBITDA margins in FMCG (36–38%) due to its cigarettes business. HUL shows margin compression from FY2022 as input cost inflation hit. Asian Paints' FY2024 margin recovery to 21% is notable after FY2022–23 raw material pressure.

### 4. Banking — SBI's ROE overtook private peers
SBI's ROE jumped from 6.6% in FY2020 to 20.3% in FY2024 — overtaking HDFC Bank by FY2023. This reflects the payoff from NPA cleanup and government recapitalisation. ICICI's consistent improvement trajectory makes it the most compelling private sector story.

### 5. Pharma — steady compounder, low risk
All three pharma companies show declining D/E ratios — Sun Pharma from 0.07 to 0.01 — and steadily improving ROE. The sector offers low volatility with consistent margin expansion, making it attractive for conservative investors.

---

## DAX Measures Created

```dax
Total Revenue = SUM(Data[Revenue (₹ Cr)])

Previous Year Revenue =
CALCULATE(
    [Total Revenue],
    Data[Year] = MAX(Data[Year]) - 1
)

YoY Revenue Growth % =
DIVIDE(
    [Total Revenue] - [Previous Year Revenue],
    [Previous Year Revenue]
) * 100

Net Income Margin % =
DIVIDE(SUM(Data[Net_Income (₹ Cr)]), SUM(Data[Revenue (₹ Cr)])) * 100

Average ROE = AVERAGE(Data[ROE (%)])

Sector Average ROE =
VAR SelectedSector = SELECTEDVALUE(Data[Sector])
VAR SelectedYear = MAX(Data[Year])
RETURN
CALCULATE(
    AVERAGE(Data[ROE (%)]),
    FILTER(
        ALL(Data),
        Data[Sector] = SelectedSector &&
        Data[Year] = SelectedYear
    )
)

Avg EBITDA Margin = AVERAGE(Data[EBITDA_Margin (%)])

Avg DE Ratio = AVERAGE(Data[Debt_to_Equity])
```

---

## Files in This Repository

| File | Description |
|---|---|
| `nifty50_dashboard_data.xlsx` | Source data — 75 rows, 15 companies × 5 years, 3 sheets |
| `page1_sector_overview.png` | Screenshot — Sector Overview page |
| `page2_company_deepdive.png` | Screenshot — Company Deep Dive page |
| `page3_sector_comparison.png` | Screenshot — Sector Comparison page |

---

## Data Source

- [Screener.in](https://www.screener.in) — Consolidated P&L, Balance Sheet, Ratios
- BSE Annual Reports — FY2020 to FY2024
- All figures in ₹ Crores unless stated otherwise
- Fiscal year = April to March (e.g. FY2024 = April 2023 to March 2024)

---

## Power BI Features Used

| Feature | Used for |
|---|---|
| Slicers | Company and Sector filtering |
| Cross-filtering | Click any visual to filter others |
| Conditional formatting | Heat map colouring, growth bar colours |
| DAX measures | YoY growth, sector benchmarks, margins |
| Analytics pane | Constant lines (D/E risk threshold at 1.0) |
| Sync slicers | Company slicer synced across Page 2 and Page 3 |
| Scatter chart | Risk-return plot (ROE vs D/E) |
| Matrix visual | ROE heat map across all companies and years |

---

## About This Project

Built as part of a self-directed financial analysis portfolio targeting Financial Analyst roles in Bengaluru's GCC ecosystem.

Tools: Power BI Desktop, Microsoft Excel
Data source: Screener.in, BSE Annual Reports
Time to build: ~10 hours

---

*This dashboard is for educational and portfolio purposes only and does not constitute investment advice.*
