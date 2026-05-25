# SCM Assistant Bot
**BQBYTE Technologies — Supply Chain RAG Chatbot**
Trinamix Inc · Junior AI Engineer Hiring Task · Ref: TX-JrAI-003

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
- **Chunk Size:** 500 tokens
- **Chunk Overlap:** 50 tokens
- **Splitter:** Recursive Character Text Splitter
- **CSV Chunk Count:** `[fill in from Flowise upsert screen]`
- **PDF Chunk Count:** `[fill in from Flowise upsert screen]`

**Observation:** Smaller chunks improve retrieval precision for specific policy clauses (e.g. exact penalty percentages) but can miss context that spans multiple paragraphs. Table rows from the CSV are cleanly isolated.

## ❓ Sample Q&A — Chatbot Answers (Verbatim)

> All answers below were produced by the deployed SCM Assistant chatbot. Copy exact output from your chat panel.

---

### Q1: Which Tier-3 suppliers have an active disruption flag, and what response level applies per policy?

**Answer:**
```

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

```

---

### Q2: Which suppliers qualify for the annual Volume Rebate Program and how many are there?

**Answer:**
```
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

```

---

### Q3: Which region has the highest total PO value, and does it breach the concentration limit?

**Answer:**
```

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

```

---

### Q4: Which suppliers are on Supplier Watch List (SWL) status and what does it restrict?

**Answer:**
```
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

```

---

### Q5: Which product category has the highest average defect rate and does it exceed the Tier-2 limit?

**Answer:**
```
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

```

---


### Configuration B — Large Chunks (Context-focused)
- **Chunk Size:** 1500 tokens
- **Chunk Overlap:** 200 tokens
- **Splitter:** Recursive Character Text Splitter
- **CSV Chunk Count:** `[fill in from Flowise upsert screen]`
- **PDF Chunk Count:** `[fill in from Flowise upsert screen]`

**Observation:** Larger chunks preserve more policy context per retrieval (better for multi-clause questions like disruption response levels), but can dilute relevance scores when the question targets a single fact. Recommended for policy-heavy queries.

**Chosen Config for Final Deployment:** Configuration B — better suited for cross-referencing policy sections with supplier data.

---

## ❓ Sample Q&A — Chatbot Answers (Verbatim)

> All answers below were produced by the deployed SCM Assistant chatbot. Copy exact output from your chat panel.

---

### Q1: Which Tier-3 suppliers have an active disruption flag, and what response level applies per policy?

**Answer:**
```
[PASTE VERBATIM CHATBOT ANSWER HERE]
```

---

### Q2: Which suppliers qualify for the annual Volume Rebate Program and how many are there?

**Answer:**
```
[PASTE VERBATIM CHATBOT ANSWER HERE]
```

---

### Q3: Which region has the highest total PO value, and does it breach the concentration limit?

**Answer:**
```
[PASTE VERBATIM CHATBOT ANSWER HERE]
```

---

### Q4: Which suppliers are on Supplier Watch List (SWL) status and what does it restrict?

**Answer:**
```
[PASTE VERBATIM CHATBOT ANSWER HERE]
```

---

### Q5: Which product category has the highest average defect rate and does it exceed the Tier-2 limit?

**Answer:**
```
[PASTE VERBATIM CHATBOT ANSWER HERE]
```

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
