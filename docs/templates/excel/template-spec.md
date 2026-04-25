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

State all conventions used. Clarity here ensures reproducibility.

Example list:
- All figures reported in millions unless otherwise noted.
- Tax rate assumed at statutory 21% (or effective rate from financial statements).
- Cost of capital estimated at [X]% based on [method/assumption].
- Interest rates quoted on a simple annual basis.
- Depreciation figure taken from [Income Statement / Cash Flow Statement].
- Start-of-year values use prior fiscal year's Balance Sheet.
- No off-balance-sheet items or contingent liabilities included.

---

## 4. Calculation Flow

Describe the logic and sequencing of your analysis — as if briefing a junior analyst or AI model builder. Use named-range pseudocode.

### Step 1: Derived Inputs
1. Compute `market_capitalization` = `share_price` x `shares_outstanding`
2. Compute `currentYear_after_tax_operating_income` = `INC_net` + (1 - `tax_rate`) x `INC_interest_expense`
3. Compute daily figures: `currentYear_daily_sales_average` = `INC_sales` / 365
4. Compute averages: `avg_equity` = AVERAGE(`startYear_equity`, `currentYear_equity`)
5. Compute `currentYear_working_capital_net` = `currentYear_assets_current` - `currentYear_liabilities_current`

### Step 2: Performance Ratios
- MVA = `market_capitalization` - `currentYear_equity`
- Market-to-Book = `market_capitalization` / `currentYear_equity`
- EVA = `currentYear_after_tax_operating_income` - (`cost_capital` x `startYear_total_capitalization`)

### Step 3: Profitability Ratios
- ROA = `currentYear_after_tax_operating_income` / `startYear_total_assets`
- ROC = `currentYear_after_tax_operating_income` / `startYear_total_capitalization`
- ROE = `INC_net` / `startYear_equity`
- (Repeat with average denominators)

### Step 4: Efficiency Ratios
- Asset Turnover = `INC_sales` / `startYear_total_assets`
- Receivables Turnover, Inventory Turnover, margins, etc.

### Step 5: Leverage Ratios
- Long-term Debt Ratio, Debt-Equity, Total Debt Ratio
- Times Interest Earned, Cash Coverage, Debt Burden, Leverage Ratio

### Step 6: Liquidity Ratios
- NWC-to-Assets, Current Ratio, Quick Ratio, Cash Ratio

### Step 7: Du Pont Decomposition
- Du Pont ROA = Asset Turnover x Operating Profit Margin
- Du Pont ROE = Leverage x Asset Turnover x Operating Profit Margin x Debt Burden

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

Reflect candidly on your Stage 2 Excel model. This section is what makes a post-build spec more valuable than a pre-build plan.

- **What worked well?** Which formulas, layouts, or named ranges operated as intended?
- **What would you change?** Were there workarounds, naming inconsistencies, or layout issues worth fixing?
- **What would make the model more auditable?** Consider formula documentation, color coding, or structural changes.
- **What additional analysis is worth including?** Industry comparisons, trend data, additional ratios?

> *Tip:* Be honest and specific. "The Du Pont decomposition matched my direct ROA within 0.1%" is useful. "Everything worked fine" is not.

---

## 8. Limitations & Next Steps

Briefly note any analytical limits and outline your next step.

Example phrasing:
> This specification does not incorporate industry peer comparisons, multi-year trend analysis, or off-balance-sheet items. The next phase will involve writing a structured AI prompt and final analysis interpreting the ratio results for senior management.

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
