# FX Hedge Technical Specification – €4,500,000 receivable (12 months)

**Created by:** Jacob Baltazar  
**Updated by:** Jacob Baltazar  
**Date Created:** 2025-11-06  
**Date Updated:** 2025-11-06  
**Version:** 1.0  
**LLM Used:** GPT-5 mini
**Role:** Financial / Treasury Analyst  
**Audience:** CFO / Director of Treasury

---

## 1. Problem statement
Our company will receive EUR 4,500,000 in exactly 12 months for equipment sales. This creates foreign exchange exposure because if the euro weakens against the U.S. dollar, the realized proceeds in dollars will decline and profit margins will shrink. The objective is to quantify the potential U.S. dollar outcomes under different hedging strategies, compare the trade-off between certainty and optionality, and recommend a hedging approach that aligns with the firm’s risk tolerance and cost constraints. The key decision is whether to prioritize protecting a minimum U.S. dollar value or to maintain the ability to benefit if the euro strengthens.

---

## 2. Inputs (Known variables)

| Variable | Description | Unit | Example / Value (used in spec) | Source |
|---|---:|---:|---:|---|
| FC_AMT | Foreign-currency receivable | EUR | 4,500,000 | Company data |
| S0 | Current EURUSD spot | USD per EUR | 1.167 | Market quote (Oct–Nov 2025) — verify live |
| F0_1y | 1‑year EURUSD forward | USD per EUR | 1.0875 | Provided |
| r_USD_1y | USD 1‑year interest (deposit/borrowing proxy) | % pa | 5.375% (midpoint of 5.25–5.50) — confirm live | Central bank / market |
| r_EUR_1y | EUR 1‑year interest (deposit/borrowing proxy) | % pa | 4.50% (ECB main) — confirm live | ECB / market |
| K_put | Put strike (EUR) | USD per EUR | set = S0 (1.167) unless instructed otherwise | Analyst choice |
| Premium_put | Put premium | USD per EUR | 0.015 (scenario input) | Scenario / dealer quote |
| K_call | Call strike (EUR) | USD per EUR | set = S0 (1.167) | Analyst choice |
| Premium_call | Call premium | USD per EUR | 0.018 (scenario input) | Scenario / dealer quote |
| t | Time to maturity | Years | 1.0 | Derived |
| Tx_costs | Transaction costs / bid‑ask | USD | Assume 0 unless specified | Assumption (see section 3) |

Notes: confirm whether premiums are quoted per EUR notional or per contract; this spec assumes USD per EUR notional (i.e., premium_per_EUR). If premium is per option contract, adjust using contract notional.

---

## 3. Assumptions & constraints
- Rates quoted as simple annual percentages; no daycount adjustment for 1‑year horizon.  
- Forward F0_1y given and used as market forward; money‑market parity will be used to validate.  
- Option premiums paid upfront in USD and quoted per EUR notional (premium_per_EUR). Premiums non‑refundable.  
- No transaction costs, collateral, or credit charges unless specified; bid/ask spread ignored for base case. These can be added in stress runs.  
- Counterparty credit / margining is out of scope for current model.  
- Tax and accounting impacts (e.g., hedge accounting) excluded from numerical comparison but noted in recommendations.  
- Option contract size and exercise style (European vs. American) must be confirmed in Stage 3.

---

## 4. Calculation flow (logic & sequence)
All results expressed in nominal USD at hedge maturity (T = 1 year). Order of operations:

1. Baseline: compute unhedged USD outcome at assumed S_T scenarios:
   - USD_unhedged(S_T) = FC_AMT * S_T.

2. Forward hedge:
   - USD_forward = FC_AMT * F0_1y.
   - Report: locked USD proceeds, and difference vs. spot and scenarios.

3. Money‑market hedge (synthetic forward):
   - Compute EUR present value required today: PV_EUR = FC_AMT / (1 + r_EUR_1y).
   - Borrow USD today equal to PV_EUR * S0 (or borrow USD equivalent to fund EUR purchase), convert to EUR at S0, invest to reach FC_AMT at T.
   - Repay USD loan at T with proceeds from invested EUR converted at prevailing spot used at initiation; net USD outcome derived—should economically equal forward (validate parity).  
   - Report USD_mm and flag financing/credit assumptions.

4. Option hedge (protective put):
   - Net USD if put bought with strike K_put and premium p_put per EUR:
     - USD_put(S_T) = FC_AMT * max(S_T, K_put) - (FC_AMT * Premium_put).
     - If S_T < K_put, exercise: receive K_put * FC_AMT minus premium. If S_T ≥ K_put, convert at S_T and subtract premium.
   - Compute alternative: collar (buy put + sell call) if requested—outline structure and net premium.

5. Option call (if sold or bought for synthetic structures):
   - If buying a call, USD_call(S_T) = FC_AMT * S_T - (FC_AMT * Premium_call) + optional exercise payoff logic (not commonly used for receivable hedges).

6. Scenario table & comparisons:
   - For each S_T in scenario set (see section 6), compute USD_unhedged, USD_forward, USD_mm, USD_put.
   - Calculate deltas vs. forward and unhedged; compute metrics below.

7. Key metrics:
   - Certainty equivalent: USD_forward.
   - Floor (net of premium): K_put - Premium_put (USD per EUR). Express as total USD floor = FC_AMT * (K_put - Premium_put).
   - Cost of protection: Premium_total = FC_AMT * Premium_put.
   - Breakeven S_T where option net equals forward/unhedged.

---

## 5. Outputs (spreadsheet cells & visualizations)
Numeric outputs (per maturity):

- USD_unhedged(S_T) — table across scenarios.
- USD_forward — single value (4,893,750 using F0_1y = 1.0875).
- USD_mm — single value (validate equal or near USD_forward).
- USD_put(S_T) — table across scenarios (net of premium).
- Premium_total (USD) = FC_AMT * Premium_put.
- Effective_floor_per_EUR = K_put - Premium_put; Effective_floor_total = FC_AMT * Effective_floor_per_EUR.
- Breakeven spots vs. each hedge.

Charts & tables:
- Line chart: USD proceeds (y) vs. S_T (x) for unhedged, forward, money‑market, and put‑hedge.
- Tornado or bar chart: differences vs. forward at stress spots (1.00, 1.05, 1.10, 1.20).
- Summary table: cost, floor, upside participation, operational complexity, credit needs.

Presentation outputs:
- 1‑page executive summary with recommended hedge and rationale.
- Appendix: full scenario table and model assumptions.

---

## 6. Sensitivity & testing plan
Scenario grid:
- Primary grid: S_T = {0.95×S0, 0.98×S0, 1.00×S0, 1.02×S0, 1.05×S0, 1.10×S0, 1.15×S0, 1.20×S0}
- Stress cases: extreme EUR depreciation to 1.00, 0.95 and appreciation to 1.25.
- Vary interest rates ±50 bps to test money‑market parity and funding cost sensitivity.
- Vary premium ±20% (dealer quote uncertainty).

For each node compute all USD outcomes and present:
- Dollar difference vs. forward (loss / gain).
- Percent impact on expected gross margin.
- Option ROI (if exercised) and payoff table.

Visuals: overlay lines for each hedge on the same chart; highlight floor lines.

---

## 7. Limitations & next steps
Limitations:
- Model ignores implied volatility dynamics, bid/ask spreads, collateral/margin funding, and accounting/tax effects.
- Premium treatment assumes per‑EUR quoting; must confirm contract specifications (size, European style).
- Counterparty credit and operational execution risk are not monetized.

Immediate next steps (Stage 3):
1. Confirm live market quotes: S0, F0 (1y), dealer option premiums and contract specs.  
2. Build Excel model following this spec with named ranges matching the Inputs table.  
3. Run scenario grid, produce charts, and prepare an executive one‑page recommendation with quantified tradeoffs (cost vs. protection).  
4. Obtain treasury credit limits and execution preference (bank forwards vs. exchange‑traded options) prior to implementation.

This specification is intentionally implementation‑ready: each input maps to a named cell, each calculation step maps to a spreadsheet block, and outputs drive the executive decision in Stage 5.
