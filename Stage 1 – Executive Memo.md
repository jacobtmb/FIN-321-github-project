# FX Receivable Exposure – Executive Memo

**Created by:** Jacob Baltazar  
**Updated by:** Jacob Baltazar  
**Date Created:** 10/21/25  
**Date Updated:** 10/24/25  
**Version:** 1.0  
**LLM Used:** GPT-4 

---

## Executive Summary
Our firm expects to receive €4,500,000 in one year for solar equipment sales. With the EURUSD forward rate at 1.0875, today’s value is approximately $4,893,750. However, currency market volatility means our actual USD proceeds could vary significantly by maturity. Unhedged, we risk adverse EURUSD moves and potential revenue loss. To better manage this exposure, we recommend evaluating hedging tools—forward contracts, money market hedges, and options, which can lock in value, reduce uncertainty, and protect our bottom line.

---

## Background & Objectives
As a U.S. importer, we have a €4,500,000 receivable due in 12 months. The current EURUSD spot as of 10/24/25 (1.1625) and 1-year forward rate (1.0875) reflect market expectations. Our objective is to maximize USD proceeds and minimize downside risk from EUR depreciation. Exchange rate swings can erode expected revenue, so if the euro weakens, we receive fewer dollars.

---

## Methods
We consider three key hedging strategies:
1. **Forward Contract:** Lock in the 1-year forward rate (1.0875) for full certainty of USD proceeds ($4,893,750).  
   *Pros:* Eliminates FX risk; simple to execute.  
   *Cons:* No upside if EUR appreciates; contractual obligation.

2. **Money Market Hedge:** Borrow EUR, invest USD at prevailing rates (USD interest ≈ 4.11%, EUR interest ≈ 2.15%).  
   *Pros:* Flexibility; effective for matching cash flows.  
   *Cons:* Requires access to both markets; may involve more transactions.

3. **Options (Put on EUR):** Buy EUR put option (strike = spot or forward, premium $0.015 per contract).  
   *Pros:* Downside protection; retain upside if EUR strengthens.  
   *Cons:* Upfront cost (premium); can be more expensive than forwards.

---

## Limitations & Next Steps
Interest rate changes or market disruptions could affect hedge pricing and execution. Immediate next steps:  
- Technical Specification: a quantitative plan for a spreadsheet model.
- Excel Model Build: Implement your spec into a basic .xlsx model.
- Prompt Engineering: Write a prompt that, given inputs, asks an AI to generate the spreadsheet matching your spec.
- Final Analysis & Recommendation: Choose a hedge using your model.


---

## References
- (EURUSD=X) | Stock Price & Latest News | Reuters. (n.d.). Www.reuters.com. https://www.reuters.com/markets/quote/EURUSD=X/
- Federal Reserve Bank of New York. (2025). Effective Federal Funds Rate. Www.newyorkfed.org. https://www.newyorkfed.org/markets/reference-rates/effr
- Trading Economics. (2025). Euro Area Interest Rate. Tradingeconomics.com; TRADING ECONOMICS. https://tradingeconomics.com/euro-area/interest-rate
- Eun, C., S., Resnick, B. G., & Chuluun, T. (2023). International financial management (10th ed.). McGraw-Hill Education.
