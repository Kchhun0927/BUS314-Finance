# [Spec Title] – Technical Specification

**Created by:** [Kayla Chhun]
**Updated by:** [Kayla Chhun]
**Date Created:** [04/24/2026
**Date Updated:** [04/25/2026]
**Version:** [1.0]
**LLM Used:** [ChatGPT] (optional — if an LLM was used)

**Role:** Financial Analyst / FP&A Analyst
**Audience:** CFO or Director of FP&A

**Purpose:** Provide a professional, quantitative specification documenting your Excel model's analytical structure for computing and interpreting accounting and performance ratios from a public company's financial statements. This post-build spec captures what you built, what you learned, and how the model should be refined.

---

## 1. Problem Statement

Apple Inc. is a publicly traded consumer electronics and technology company. This specification outlines the analytical framework for computing a comprehensive set of accounting and performance ratios using Apple’s FY2022 and FY2023 financial statements.

The objective of the model is to:

Evaluate Apple’s financial health across profitability, efficiency, leverage, and liquidity dimensions
Identify operational strengths and potential risks
Decompose return metrics (ROA and ROE) using DuPont analysis
Support internal decision-making for CFO-level review and financial planning

This model is designed for use in a CFO briefing or FP&A analysis, providing structured insights into Apple’s financial performance and capital efficiency.
---

## 2. Inputs (Known Variables)

### Balance Sheet Items (Current Year and Prior Year)

| Variable |	Description |	Named Range |	Example |
|----------|-------------|-------------|---------|
| Cash & marketable securities |	Liquid assets	| `BAL_cash_[year]`	|	23,646 / 29,965 |
| Receivables	| Accounts receivable |	`BAL_receivables_[year]` | 28,184 / 29,508 |
| Inventories	| Inventory balance	| `BAL_inventory_[year]` |	4,946 / 6,331 |
| Total current assets |	Sum of current assets |	`BAL_assets_current_[year]`	|	135,405 / 143,566 |
| Net tangible fixed assets	| PP&E less depreciation |	`BAL_ppe_net_[year]`	|	42,117 / 43,715 |
| Total assets	| All assets	| `BAL_assets_total_[year]`	| 352,755 / 352,583 |
| Total current liabilities | Short-term obligations |	`BAL_liabilities_current_[year]`	|	153,982 / 145,308 |
| Long-term debt | Non-current borrowings	| `BAL_debt_long_term_[year]`	|	98,959 / 95,281 |
| Total liabilities |	All liabilities	| `BAL_liabilities_total_[year]`	|	302,083 / 290,437 |
| Shareholders’ equity	| Book value of equity | `BAL_equity_[year]`	|	50,672 / 62,146 |

### Income Statement Items

| Variable | Description | Named Range | Example |
|----------|-------------|-------------|---------|
| Revenue	| Total sales	| `INC_revenue` |	383,285 |
| Cost of goods sold |	Direct costs |	`INC_cogs` |	214,137 |
| EBIT |	Operating income |	`INC_ebit`	| 114,301 |
| Interest expense |	Cost of debt | `INC_interest` |	3,933 |
| Pre-tax income | Income before taxes |	`INC_pre_tax` |	113,736 |
| Taxes	| Income tax expense |	`INC_taxes` |	16,741 |
| Net income |	Profit after tax	| `INC_net_income` |	96,995 |
| Depreciation |	Non-cash expense |	`INC_depreciation` |	11,519 |

### Cash Flow Statement Items

| Variable | Description | Named Range | Example |
|----------|-------------|-------------|---------|
| Cash from operations |	Operating cash flow |	`CASH_operating` | 110,543 |
| Cash from investments |	Investing cash flow |	`CASH_investing` |	-3,765 |
| Cash from financing |	Financing cash flow	| `CASH_financing` |	-108,488 |

### Market / Analyst Inputs

| Variable | Description | Named Range | Example |
|----------|-------------|-------------|---------|
| Tax rate	| Effective tax rate |	`tax_rate`	| 14.7% |
| Cost of capital |	Required return	| `cost_capital` |	8.0% |

---

## 3. Assumptions & Constraints

All financial figures are reported in millions of USD
Tax rate is derived from financial statements (~14.7%)
Average values are calculated using simple averages of start and end values
No off-balance-sheet liabilities are included
Market-based inputs (share price, shares outstanding) are not included
Interest expense is treated as a single-period value
Depreciation is included for cash coverage calculations
Start-of-year values correspond to FY2022 balance sheet data

---

## 4. Calculation Flow

Describe the logic and sequencing of your analysis — as if briefing a junior analyst or AI model builder. Use named-range pseudocode.

### Step 1: Derived Inputs
1. Calculate `market_capitalization` = `share_price` × `shares_outstanding`
2. Calculate `after_tax_operating_income` = `INC_ebit` × (1 − `tax_rate`)
3. Calculate `daily_sales` = `INC_revenue` / 365
4. Calculate `avg_assets` = AVERAGE(`BAL_assets_total_2022`, `BAL_assets_total_2023`)
5. Calculate `avg_equity` = AVERAGE(`BAL_equity_2022`, `BAL_equity_2023`)
6. Calculate `avg_inventory` = AVERAGE(`BAL_inventory_2022`, `BAL_inventory_2023`)
7. Calculate `avg_receivables` = AVERAGE(`BAL_receivables_2022`, `BAL_receivables_2023`)
8. Calculate `startYear_total_capital` = `BAL_debt_long_term_2022` + `BAL_equity_2022`
9. Calculate `currentYear_total_capital` = `BAL_debt_long_term_2023` + `BAL_equity_2023`
10. Calculate `avg_total_capital` = AVERAGE(`startYear_total_capital`, `currentYear_total_capital`)
11. Calculate `currentYear_nwc` = `BAL_assets_current_2023` − `BAL_liabilities_current_2023`
    
### Step 2: Performance Ratios
- MVA = `market_capitalization` − `BAL_equity_2023`
- Market_to_Book = `market_capitalization` / `BAL_equity_2023`
- EVA = `after_tax_operating_income` − (`cost_capital` × `startYear_total_capital`)

### Step 3: Profitability Ratios
- `ROA_start` = `INC_net_income` / `BAL_assets_total_2022`
- `ROC_start` = `after_tax_operating_income` / `startYear_total_capital`
- `ROE_start` = `INC_net_income` / `BAL_equity_2022`
- (Repeat with average denominators)
- `ROA_avg` = `INC_net_income` / `avg_assets`
- `ROC_avg` = `after_tax_operating_income` / `avg_total_capital`
- `ROE_avg` = `INC_net_income` / `avg_equity`

### Step 4: Efficiency Ratios
- `Asset_Turnover` = `INC_sales` / `startYear_total_assets`
- `Receivables_Turnover` = `INC_revenue` / `avg_receivables`
- `Inventory_Turnover` = `INC_cogs` / `avg_inventory`
- `Days_in_Inventory` = 365 / `Inventory_Turnover`
- `Avg_Collection_Period` = 365 / `Receivables_Turnover`
- `Profit_Margin` = `INC_net_income` / `INC_revenue`
- `Operating_Margin` = `INC_ebit` / `INC_revenue`
  
### Step 5: Leverage Ratios
- `Long_Term_Debt_Ratio` = `BAL_debt_long_term_2023` / `BAL_assets_total_2023`
- `Total_Debt_Ratio` = `BAL_liabilities_total_2023` / `BAL_assets_total_2023`
- `Debt_to_Equity` = `BAL_debt_long_term_2023` / `BAL_equity_2023`
- `Times_Interest_Earned` = `INC_ebit` / `INC_interest`
- `Cash_Coverage` = (`INC_ebit` + `INC_depreciation`) / `INC_interest`
- `Debt_Burden` = `INC_net_income` / `INC_ebit`
- `Leverage_Ratio` = `BAL_assets_total_2023` / `BAL_equity_2023`

### Step 6: Liquidity Ratios
- `NWC_to_Asset` = `currentYear_nwc` / `BAL_assets_total_2023`
- `Current_Ratio` = `BAL_assets_current_2023` / `BAL_liabilities_current_2023`
- `Quick_Ratio` = (`BAL_assets_current_2023` − `BAL_inventory_2023`) / `BAL_liabilities_current_2023`
- `Cash_Ratio` = `BAL_cash_2023` / `BAL_liabilities_current_2023`

### Step 7: Du Pont Decomposition
- Du Pont ROA = `Profit_Margin` × `Asset_Turnover`
- Du Pont ROE = `Profit_Margin` × `Asset_Turnover` × `Leverage_Ratio`
---

## 5. Outputs

| Output | Description | Format | Purpose |
|--------|-------------|--------|---------|
| Ratio summary table | All 25+ ratios organized by category | Table | Core analytical output |
| Du Pont decomposition | ROA and ROE breakdown | Table | Identifies return drivers |
| Formula documentation | Named-range formula for each ratio | Column | Auditability |
| Executive summary | Key findings and recommendations | 1–2 paragraphs | Stage 5 input |

---

## 7. Model Review — What Worked & What to Improve

| What worked well |
- The model effectively links financial statement inputs to ratio outputs using consistent named ranges.
- Clear separation between inputs, calculations, and outputs improved readability and usability.
- Average-based ratios (ROA, ROE, ROC) produced results consistent with expected financial behavior.
- Du Pont decomposition closely matched directly calculated ROA and ROE, confirming internal consistency of the model.
  
| What would be improved |
- Naming conventions could be standardized further (e.g., consistent use of “revenue” vs. “sales”).
- Pre-tax income could more explicitly incorporate non-operating income components.
- Market-based inputs (share price and shares outstanding) were not included, limiting performance ratio analysis (e.g., MVA, Market-to-Book).
- Some formulas initially relied on start-year values only and were later adjusted to include averages for improved accuracy.

| Auditability improvements |
- Add a validation check to ensure balance sheet consistency:
`BAL_assets_total_2023` = `BAL_liabilities_total_2023` + `BAL_equity_2023`
- Expand formula documentation to include all intermediate calculations and derived inputs
- Separate assumptions (tax rate, cost of capital) into a clearly labeled section.
- Apply consistent color-coding conventions (e.g., yellow = inputs, blue = assumptions, green = formulas, gray = outputs).
  
| Additional analysis |
- Industry benchmarking against comparable firms.
- Multi-year trend analysis to evaluate performance over time.
- Inclusion of valuation metrics such as EVA, MVA, and Market-to-Book.
- Cash conversion cycle analysis for deeper operational insight.

---

## 8. Limitations & Next Steps
| Limitations |
- The model does not include market-based valuation metrics due to missing share price and shares outstanding inputs.
- Analysis is limited to a two-year comparison (FY2022–FY2023), restricting trend evaluation.
- Tax rate and cost of capital are simplified assumptions and may not reflect forward-looking conditions.
- No sensitivity or scenario analysis is incorporated.

| Next Steps |
- Convert this specification into a structured AI prompt to automate ratio calculations and analysis.
- Expand the model to include:
* Multi-year financial analysis
* Industry peer benchmarking
* Market-based valuation metrics
- Develop a formal executive summary interpreting the ratio results for senior management.
---

## Writing a Strong Specification

**Your spec should:**
- **Communicate like a professional:** clear, structured, and jargon-free.
- **Think one stage ahead:** your spec should feed directly into your Stage 4 AI prompt and final analysis.
- **Be internally consistent:** variables, labels, and steps must align with your actual model.
- **Be reproducible:** a new analyst should be able to rebuild the model from your spec without your help.
- **Be reflective:** the "Model Review" section should show honest assessment of what worked and what needs improvement.
- **Be executive-relevant:** the CFO should understand *what you built* and *why it matters*.

---

## How This Sets You Up for Stage 4

| What You Write in Stage 3 | What It Enables in Stage 4 |
|----------------------------|----------------------------|
| Named ranges with precise definitions | AI uses standardized variable names, no improvisation |
| Step-by-step calculation flow | AI generates correct, auditable formulas |
| Model review and improvement notes | AI builds the *improved* version, not just a replica |
| Explicit output requirements | AI produces the exact tables, charts, and sections you need |
| "Outputs" section | Drives your interpretation and recommendation |

> *By completing your spec after the build, you create a machine-readable blueprint that transforms your prototype into a polished, documented model — demonstrating the reflective documentation skills valued by finance teams and auditors alike.*
