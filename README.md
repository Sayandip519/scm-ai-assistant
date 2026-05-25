# SCM Assistant Bot
**BQBYTE Technologies — Supply Chain RAG Chatbot**
---

## 🔗 Public Chatbot URL

```
https://cloud.flowiseai.com/chatbot/[YOUR-CHATBOT-ID]
```
> Replace `[YOUR-CHATBOT-ID]` with your actual chatbot ID after publishing.

---

## ⚙️ Tech Stack

| Component | Choice | Notes |
|---|---|---|
| Platform | Flowise Cloud (flowiseai.com) | Free tier |
| LLM | `[e.g. gpt-4o-mini / claude-3-haiku / gemini-flash]` | Replace with your choice |
| Embeddings | `[e.g. text-embedding-3-small / nomic-embed-text]` | Replace with your choice |
| Vector Store | Flowise in-memory / Pinecone / Qdrant | Replace with your choice |
| Document Store | Flowise Document Store | Both CSV + PDF loaded |

---

## 📂 Data Sources

| File | Description | Chunks (Config A) | Chunks (Config B) |
|---|---|---|---|
| `supplier_performance_data.csv` | 2,000 POs across 116 suppliers, 27 columns | `[fill in]` | `[fill in]` |
| `SupplyChain_Governance_Policy_v3_2_1.pdf` | 10-section governance policy (8 pages) | `[fill in]` | `[fill in]` |

---

## 🔧 Chunk Configuration Experiments

### Configuration A — Small Chunks (Precision-focused)
- **Chunk Size:** 1000 tokens
- **Chunk Overlap:** 200 tokens
- **Splitter:** Recursive Character Text Splitter

**Observation:** Larger chunks preserve more policy context per retrieval (better for multi-clause questions like disruption response levels), but can dilute relevance scores when the question targets a single fact. Recommended for policy-heavy queries.

## ❓ Sample Q&A — Chatbot Answers (Verbatim)

> All answers below were produced by the deployed SCM Assistant chatbot. Copy exact output from your chat panel.

---

### Q1: Which Tier-3 suppliers have an active disruption flag, and what response level applies per policy?

**Answer:**

Based on the supplier performance records included in the provided context, **no Tier-3 suppliers with an active disruption flag are present.** The only supplier record supplied (SUP-043) is a Tier-1 supplier and therefore does not meet the criteria you asked about.

**Policy reference for response levels**

When an active disruption flag is present, the response tier is determined by the supplier’s risk classification (Low, Medium, High) and by any mandatory-Level-3 disruption categories (export-control restrictions, active labour strikes, regulatory enforcement actions, or port closures > 72 hours). The applicable response levels are:

| **Risk Level / Condition** | **Response Level** | **Key Actions** |
| :--- | :--- | :--- |
| **Low-Risk** supplier with a disruption flag | **Level 1 – Monitor** | Weekly status update; safety stock +15 % |
| **Medium-Risk** supplier with a disruption flag | **Level 2 – Manage** | Bi-weekly escalation calls; safety stock +30 %; alternate supplier placed on 48-hour readiness notice |
| **High-Risk** supplier with any disruption flag or any supplier with **two simultaneous disruption flags** | **Level 3 – Activate** | Immediate escalation to CPO; alternate supplier activated for ≥40 % of volume within 10 business days; safety stock +50 %; full RCA within 15 business days |
| **Mandatory Level 3** categories (regardless of risk): export-control restrictions, active labour strikes, regulatory enforcement actions, port closures > 72 hours | **Level 3 – Activate** | Same actions as above |

If a Tier-3 supplier were to have an active disruption flag, you would first determine its overall risk level (using the criteria in Section 2 of the policy). Then you would apply the corresponding response level from the table above, with the mandatory Level 3 override if the disruption falls into one of the listed categories.

**Bottom line:** The current data set does not contain any Tier-3 suppliers flagged for disruption, so no specific response level can be assigned at this time. Should such a supplier appear in the performance register, apply the risk-based response tier as outlined.

---

### Q2: Which suppliers qualify for the annual Volume Rebate Program and how many are there?

**Answer:**

### Qualified suppliers for the Annual Volume Rebate Program

| Supplier ID | Tier | OTD Rate % | Defect Rate % | Sustainability Score | Meets all rebate criteria? |
| :--- | :--- | :--- | :--- | :--- | :--- |
| (none in the data set) | – | – | – | – | **No** |

### Explanation

The Volume Rebate Program (Section 4.2) applies only to **Tier-1 suppliers** that, for an entire calendar year, satisfy all three of the following thresholds:
*   **On-Time Delivery (OTD) ≥ 93.0 %**
*   **Defect Rate < 0.5 %**
*   **Sustainability Score ≥ 85**

The only supplier records supplied in the context are for **SUP-084 (Recife Textiles SA)**, which is classified as **Tier-2** (see docs 3‑5). Tier-2 suppliers are not eligible for the volume rebate (Section 2, “Eligible for Volume Rebate – Yes (Tier-1 only)”).

Even if we examined the quarterly PO data for SUP-084, its OTD rates (85.3 %, 79.5 %, 90.8 %) and defect rates (1.24 %, 1.72 %, 1.49 %) are well outside the rebate thresholds, and its sustainability scores (77, 52, 69) are below the required 85.

No other Tier-1 supplier data (with the required annual metrics) is present in the provided documents, so we cannot identify any supplier that meets the full-year criteria.

### Result

*   **Number of suppliers that qualify:** 0
*   **Qualified supplier list:** none (based on the supplied data)

---

### Q3: Which region has the highest total PO value, and does it breach the concentration limit?

**Answer:**

### 1. Total PO value by region (from the six PO records you supplied)

| Region | Sum of PO_Value_USD |
| :--- | :--- |
| LATAM | $2,137,248.78 |

All six purchase orders belong to the LATAM region, so **LATAM** is automatically the region with the highest total PO value.

### 2. Does this exceed the Supplier-Governance concentration limit?

The Supplier Governance Policy (Section 3.4 – “Spend Concentration Limits”) states that no single region may account for more than 30 % of the company’s total annual spend on a given supplier tier.

Because the data set you provided contains only LATAM spend, the LATAM share of spend is **100 %** of the observed spend for this supplier tier (Tier-3).

**100 % > 30 % → The concentration limit is breached.**

### 3. Implications of the breach

| Breach Trigger | Required Action (per policy) |
| :--- | :--- |
| **Region-level spend > 30 %** of total tier spend | * Immediate escalation to the Category Management Lead.<br>* Initiate a “Regional Diversification Review” within 5 business days.<br>* Develop a mitigation plan to shift ≥ 20 % of volume to alternative regions/suppliers within the next fiscal quarter.<br>* Apply a Level-2 risk-mitigation surcharge (2 % of the PO value) on all new LATAM POs until the diversification target is met. |

### Bottom line

*   **Highest-value region:** LATAM with $2.137 M in PO value.
*   **Concentration limit:** Breached (100 % vs. the 30 % threshold).
*   **Next steps:** Follow the Level-2 mitigation actions outlined above to bring regional spend back within policy limits.

---

### Q4: Which suppliers are on Supplier Watch List (SWL) status and what does it restrict?

**Answer:**

### Suppliers that are currently on the Supplier Watch List (SWL)

Based on the documents you shared, **no supplier is explicitly identified as being on SWL status.**

The policy only tells us how a supplier would be placed on SWL, not which of the listed suppliers (e.g., SUP-005, SUP-012, SUP-015, SUP-023) meet that condition. Since the compliance-score numbers for those suppliers are not included in the provided data, we cannot definitively name any SWL-qualified supplier.

### How a supplier gets onto the SWL

| Trigger (per the Supplier Governance Policy) | Effect |
| :--- | :--- |
| **Compliance Score < 60** at any audit (regardless of tier) | Supplier is placed on Supplier Watch List (SWL). |
| **Audit-overdue status** (last audit > 14 months for Tier-1, > 7 months for Tier-2, > 4 months for Tier-3) | Supplier is placed on provisional SWL until the overdue audit is completed. |

### What SWL status restricts

| Restriction | Description |
| :--- | :--- |
| **New Purchase-Order (PO) issuance** | Limited to **20 %** of the volume that was issued in the prior quarter. All other PO requests must be deferred, re-prioritized, or routed through an alternate supplier. |
| **Contractual actions** | The supplier may still fulfill existing PO commitments, but no new contracts or volume expansions can be approved until the SWL status is cleared (i.e., compliance score rises to ≥ 60 and any audit-overdue issues are resolved). |
| **Escalation** | The account manager must notify the Category Management Lead, and a corrective-action plan must be submitted within 5 business days. Failure to improve the compliance score within the next audit cycle can trigger further penalties (e.g., loss of payment-term eligibility). |

### Bottom line

*   **No specific supplier** is flagged as SWL in the supplied documents.
*   If a supplier’s compliance score falls below 60 (or they become audit-overdue), they will be placed on SWL, which **caps any new PO volume to 20 %** of the previous quarter’s volume.
*   If you have a separate data extract (e.g., the Supplier Performance Data register) that includes the latest compliance-score values, you can cross-reference those scores against the < 60 threshold to identify the exact SWL-eligible suppliers.

---

### Q5: Which product category has the highest average defect rate and does it exceed the Tier-2 limit?

**Answer:**

### Step 1 – Gather the defect-rate data that are available

| PO ID | Product Category | Defect Rate % |
| :--- | :--- | :--- |
| PO-10240 | Packaging Materials (PM) | 0.87 % |
| PO-11742 | Electronic Components (EC) | 2.33 % |
| PO-10002 | Electronic Components (EC) | 2.09 % |

(The only categories that appear in the supplied performance register are PM and EC.)

### Step 2 – Compute the average defect rate for each category

| Category | # records | Sum of defect % | Average defect % |
| :--- | :--- | :--- | :--- |
| PM | 1 | 0.87 | 0.87 % |
| EC | 2 | 2.33 + 2.09 = 4.42 | 4.42 ÷ 2 = 2.21 % |

**Result:** The **Electronic Components (EC)** category has the highest average defect rate at 2.21 %.

### Step 3 – Compare the average to the Tier-2 defect-rate limit

Policy (Section 3.2 – Defect Rate): 
Tier-2 suppliers may not exceed 2.50 % defect rate (maximum permissible).

The EC average 2.21 % is **below** the Tier-2 ceiling of 2.50 %.

### Answer

*   **Product category with the highest average defect rate:** Electronic Components (EC) – average 2.21 %.
*   **Does it exceed the Tier-2 limit?** No. The Tier-2 maximum allowed defect rate is 2.50 %, and the EC average of 2.21 % stays within that bound.

---


### Configuration B — Large Chunks (Context-focused)
- **Chunk Size:** 800 tokens
- **Chunk Overlap:** 100 tokens
- **Splitter:** Token Text Splitter

**Observation:** Smaller chunks improve retrieval precision for specific policy clauses (e.g. exact penalty percentages) but can miss context that spans multiple paragraphs. Table rows from the CSV are cleanly isolated.

**Chosen Config for Final Deployment:** Configuration B — better suited for cross-referencing policy sections with supplier data.

---

## ❓ Sample Q&A — Chatbot Answers (Verbatim)

> All answers below were produced by the deployed SCM Assistant chatbot. Copy exact output from your chat panel.

---

### Q1: Which Tier-3 suppliers have an active disruption flag, and what response level applies per policy?

**Answer:**

### Answer based on the supplied documents

| Tier-3 Supplier (as shown in the provided data) | Active Disruption Flag | Policy-Defined Response Level |
| :--- | :--- | :--- |
| No Tier-3 supplier records with an active disruption flag are present in the supplied context. | – | – |

### Why no Tier-3 suppliers can be listed

The only supplier record included in the provided documents is **SUP-043 (Tasman Circuit Boards)**, which is explicitly identified as a **Tier-1** supplier (see doc 3).

No other supplier entries (Tier-2 or Tier-3) are included in the excerpts you shared.

According to the instruction to use only the provided context and not invent data, we cannot assume the existence of additional Tier-3 suppliers.

### How the response level would be determined (for reference)

If a Tier-3 supplier did have an active disruption flag, the applicable response tier would be derived from Section 9 – Disruption Response Procedures (doc 0) and the risk-level definitions in Section 2 (doc 2):

| Supplier Risk Level | Disruption-Flag Situation | Policy-Defined Response Level |
| :--- | :--- | :--- |
| **Low Risk** (no other risk factors, compliance > 85, etc.) | Any active disruption flag | **Level 1 – Monitor** (weekly status update; safety stock +15 %) |
| **Medium Risk** (one or more medium-risk factors, compliance 75-85, etc.) | Any active disruption flag | **Level 2 – Manage** (bi-weekly escalation calls; safety stock +30 %; alternate supplier on 48-hour readiness) |
| **High Risk** (two or more medium-risk factors, or any high-risk factor such as active labour dispute, export-control restriction, etc.) | Any active disruption flag or two simultaneous flags | **Level 3 – Activate** (immediate CPO escalation; safety stock +50 %; alternate supplier ≥40 % volume within 10 business days; RCA within 15 business days) |
| **Mandatory Level 3** (regardless of risk) | Disruption category is one of: export-control restrictions, active labour strikes, regulatory enforcement actions, or port-closure > 72 h | **Level 3 – Activate** (as above) |

Thus, if a Tier-3 supplier with an active disruption flag were identified in the data, you would first determine its Risk_Level (Low, Medium, or High) using the criteria in doc 2, then apply the corresponding response tier from doc 0 (with the mandatory Level 3 overrides listed in doc 1).

### Bottom line
* The current dataset does not contain any Tier-3 suppliers flagged for disruption, so no specific supplier names or response levels can be reported at this time. 
* If you can provide the relevant Tier-3 supplier records, I can immediately apply the policy rules and tell you the exact response level for each.

---

### Q2: Which suppliers qualify for the annual Volume Rebate Program and how many are there?

**Answer:**

### Suppliers that meet the Volume-Rebate eligibility rules

| Supplier (ID) | Tier | OTD % (annual average) | Defect % (annual average) | Sustainability Score (annual) | Meets all rebate criteria? |
| :--- | :--- | :--- | :--- | :--- | :--- |
| SUP-112 – Maplewood Polymer Corp | Tier-1 | 90.3 %* | 1.03 %* | 91* | **No** |

*\* The figures shown are the simple average of the four quarterly PO records supplied (Q1-Q4 2024). None of the quarterly values reach the required thresholds, so the annual average also falls short.*

### Why no supplier qualifies

The Volume Rebate Program (Section 4.2 of the Supplier Governance Policy) applies only to **Tier-1 suppliers** that, for an entire calendar year, satisfy all of the following:

| Requirement | Threshold |
| :--- | :--- |
| **On-Time Delivery (OTD) rate** | ≥ 93.0 % |
| **Defect rate** | < 0.5 % |
| **Sustainability score** | ≥ 85 |

The only Tier-1 supplier present in the supplied data set is **SUP-112 (Maplewood Polymer Corp)**. Its quarterly performance is:

| Quarter | OTD % | Defect % | Sustainability |
| :--- | :--- | :--- | :--- |
| **Q1-2024** | 93.8 | 1.60 | 92 |
| **Q2-2024** | 88.7 | 1.48 | 96 |
| **Q3-2024** | 89.5 | 0.58 | 95 |
| **Q4-2024** | 89.1 | 1.35 | 82 |

* **OTD:** Only Q1 meets the ≥ 93 % threshold; the other three quarters are below it.
* **Defect rate:** All quarters exceed the < 0.5 % limit.
* **Sustainability:** Q4 falls below the required ≥ 85 score.

Because the supplier does not satisfy any one of the three criteria for the full year, it is ineligible for the rebate.

### Total number of qualifying suppliers

* **Qualified suppliers:** 0
* **Total qualifying count:** 0

**Conclusion:** Based on the provided supplier performance data, no supplier qualifies for the annual Volume Rebate Program, and the count of qualifying suppliers is zero. If additional Tier-1 supplier records become available, the same eligibility checks can be applied to determine any qualifiers.

---

### Q3: Which region has the highest total PO value, and does it breach the concentration limit?

**Answer:**

### Region with the highest total PO value

| Region | PO-Value (USD) – summed from the supplied PO records |
| :--- | :--- |
| **LATAM** | $856,463.08 |
| **EMEA** | $326,516.24 |
| **APAC** | $197,819.88 |

The LATAM total comes from the two LATAM purchase orders in the data set (PO-11023 = $560,124.18 and PO-11835 = $296,338.90).

### Does this breach the concentration limit?

The Supplier Governance Policy’s “concentration-limit” rule (the maximum share of total spend that any single region may represent) is not included in the documents you provided. Because the policy threshold is missing, we cannot determine from the available information whether the LATAM share of $856,463.08 exceeds the allowed concentration limit.

### What you can do next

* Provide the specific policy clause that defines the concentration-limit percentage (e.g., “no region may exceed 40 % of total annual PO spend”).
* Once the threshold is known, we can calculate the overall spend across all regions (here: $856,463.08 + $326,516.24 + $197,819.88 = **$1,380,799.20**) and compare LATAM’s share (**≈ 62 %**) to the limit.
* Until the policy limit is supplied, we can only state that **LATAM** has the highest PO value, but we cannot confirm whether it breaches the concentration rule.

---

### Q4: Which suppliers are on Supplier Watch List (SWL) status and what does it restrict?

**Answer:**

### Suppliers that are on Supplier Watch List (SWL) status

| Supplier ID | Reason for SWL (as indicated in the source documents) |
| :--- | :--- |
| No supplier can be identified from the excerpts you have provided. | – |

### Why no supplier can be listed

The policy excerpts you supplied contain the rules that place a supplier on the Supplier Watch List, but they do not include any actual supplier-specific data (e.g., compliance-score results, last-audit dates, or explicit SWL designations). The only supplier-specific mentions in the documents are:

| Supplier ID | Context in the documents |
| :--- | :--- |
| **SUP-005, SUP-012, SUP-015, SUP-023** | Lead-time values that are near the ELTRP review boundary (no compliance-score or audit data). |
| **SUP-009, SUP-017, SUP-003** | Alternate-supplier relationships (no compliance-score or audit data). |
| **SUP-043** | Identified as a Tier-1 supplier in a separate answer (no compliance-score or audit data). |

Because the policy requires quantitative triggers (e.g., “Compliance Score < 60 at any audit” or “Audit overdue beyond the tier-specific threshold”) to actually place a supplier on SWL, and none of those trigger values are present for any supplier in the material you shared, we cannot name any supplier as being on SWL.

### What SWL status restricts

Even though we cannot point to a specific supplier, the policy does define the restriction that applies to any supplier placed on the Supplier Watch List:

| Restriction | Description |
| :--- | :--- |
| **New PO issuance limit** | When a supplier is on SWL, the volume of new purchase orders that may be issued to that supplier is capped at **20 %** of the prior-quarter’s total PO volume for that supplier. |
| **Other operational impacts** | (Implied) The reduced PO limit is intended to mitigate risk while the supplier works to improve its compliance or audit standing. No other penalties (e.g., financial penalties) are listed specifically for SWL status in the excerpts you provided. |

### Bottom line

* **No supplier can be confirmed** as being on SWL from the information you have given.
* If you can supply any of the following data, we can immediately determine SWL status:
    * The most recent Compliance Score for each supplier (0-100 scale).
    * The date of the last audit for each supplier (to evaluate the audit-overdue thresholds).
    * Any explicit SWL flag that may already be recorded in the performance register.
* Once that data is available, we will apply the policy rules (Compliance < 60 → SWL; audit overdue beyond tier-specific limits → provisional SWL) and provide a definitive list of suppliers on the Watch List, together with the 20 % PO-issuance restriction that applies to each.

---

### Q5: Which product category has the highest average defect rate and does it exceed the Tier-2 limit?

**Answer:**

| Product Category | Defect-Rate (average) | Tier-2 Maximum Defect-Rate | Exceeds Tier-2 limit? |
| :--- | :--- | :--- | :--- |
| Electronic Components (EC) | 2.095 % (average of the two supplied PO records) | 2.50 % (see Tier-2 limits in the Supplier Tier Classification Framework) | **No** – the average is below the Tier-2 ceiling. |

### How the calculation was done

**Identify the product-category data that are present**
*   Both PO records you supplied (PO-10002 and PO-11278) belong to Product_Category = “Electronic Components” (SKU-EC-xxxx).
*   No other product-category rows appear in the provided documents, so EC is the only category we can compute an average for.

**Extract the defect-rate values**
*   PO-10002: Defect_Rate_Pct = 2.09 %
*   PO-11278: Defect_Rate_Pct = 2.10 %

**Compute the simple arithmetic mean**
*   (2.09 % + 2.10 %) ÷ 2 = 2.095 %

**Compare with the Tier-2 threshold**
*   Tier-2 maximum permissible defect rate (from the Tier Classification Framework, doc 0) = 2.5 %.
*   2.095 % < 2.5 % → the limit is not breached.

### Summary

*   The **Electronic Components (EC)** category has the highest (and only) average defect rate in the data you provided, at **≈ 2.10 %**.
*   This average **does not exceed** the Tier-2 defect-rate ceiling of 2.5 %, so the category remains within the allowed performance band for Tier-2 suppliers.
*   If additional PO records for other categories become available, the same method can be applied to recompute the averages and re-evaluate compliance.

---

## 📸 Screenshots

| Step | File |
|---|---|
| 1. Flowise account created | `screenshots/01_account_created.png` |
| 2. Document Store created | `screenshots/02_document_store.png` |
| 3. CSV loaded (Config A chunks) | `screenshots/03_csv_chunks_config_a.png` |
| 4. PDF loaded (Config A chunks) | `screenshots/04_pdf_chunks_config_a.png` |
| 5. CSV loaded (Config B chunks) | `screenshots/05_csv_chunks_config_b.png` |
| 6. PDF loaded (Config B chunks) | `screenshots/06_pdf_chunks_config_b.png` |
| 7. Chatflow canvas | `screenshots/07_chatflow_canvas.png` |
| 8. Chatflow settings (LLM + embed config) | `screenshots/08_chatflow_settings.png` |
| 9. Share Chatbot panel (Make Public ON) | `screenshots/09_share_chatbot_public.png` |
| 10. Q1 answer in chat panel | `screenshots/10_qa_q1.png` |
| 11. Q2 answer in chat panel | `screenshots/11_qa_q2.png` |
| 12. Q3 answer in chat panel | `screenshots/12_qa_q3.png` |
| 13. Q4 answer in chat panel | `screenshots/13_qa_q4.png` |
| 14. Q5 answer in chat panel | `screenshots/14_qa_q5.png` |
| 15. Public URL verified in incognito | `screenshots/15_incognito_verification.png` |

---

## 🚀 What I Would Improve

1. **Structured CSV pre-processing** — Instead of raw CSV rows, pre-aggregate supplier-level summaries (e.g. mean OTD, total PO value per region) as a separate document. This dramatically improves accuracy for aggregation queries like Q2 and Q3.

2. **Hybrid retrieval** — Combine BM25 keyword search with vector similarity (available in Flowise via the Hybrid Search node). Policy clauses with exact thresholds (e.g. "93%", "USD 5M") benefit from keyword matching that pure semantic search misses.

3. **Tool-calling agent over SQL** — For numerical queries (totals, averages, counts), a code-interpreter or SQL agent querying the CSV directly would be deterministic and far more accurate than RAG over unstructured text.

4. **Metadata filtering** — Tag each chunk with `source: policy` or `source: data`, `tier`, `region` fields. At query time, filter chunks by metadata before ranking — reduces noise significantly.

5. **Reranker layer** — Add a cross-encoder reranker (e.g. Cohere Rerank or a local model) between retrieval and generation to re-score the top-k chunks by relevance before passing to the LLM.

6. **Conversation memory** — Add a buffer memory node so follow-up questions like "which of those are in APAC?" work correctly without re-stating context.

7. **Evaluation framework** — Use an LLM-as-judge or RAGAs metrics (faithfulness, answer relevancy, context recall) to score the chatbot against the 5 gold questions programmatically before submission.

---

## 📁 Repository Structure

```
scm-assistant-bot/
├── scm_assistant.json        # Flowise chatflow export
├── README.md                 # This file
├── .gitignore                # Excludes .env and API keys
└── screenshots/
    ├── 01_account_created.png
    ├── 02_document_store.png
    └── ...
```

---

*BQBYTE Technologies Supply Chain Governance Policy v3.2 · Supplier Performance Data (116 suppliers, 2,000 POs)*
