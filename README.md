# Performance Marketing & Lead Quality Optimization Analysis

An end-to-end performance marketing case study analyzing lead generation funnels, creative attribution, statistical trend validity, and CPL monetization economics for debt relief acquisition.

---

## 📌 Executive Summary

* **Total Leads Analyzed:** 3,021 leads (3,013 distinct vendor IDs) captured between April and September 2009.


* **Baseline Close Rate:** **8.13%** (245 paying enrollments).


* **Broad Lead Quality Rate:** **12.98%** (391 leads progressing through Enrollment Package stages: Closed, EP Confirmed, EP Received, EP Sent).


* **Disqualification / Waste Rate:** **16.15%** (488 leads with bad contact information, invalid identity, or insufficient debt/income).


* **Commercial Strategy Verdict:** A 20% CPL bump ($30 to $36) for a 20% quality increase (8.1% to 9.6%) causes a **-$2,430 net revenue drop** if leads are purely culled. Implementing a **Tiered Lead Routing Model** preserves volume and drives a **+$5,564 (+6.1%) net revenue gain**.



---

## 🎯 Case Study Objectives & Core Answers

### 1. Lead Quality Trends & Statistical Significance

* **Observed Trend:** Monthly close rates ranged from 10.8% in April to 4.4% in September, with broad pipeline quality peaking at 16.9% in September.


* **Statistical Rigor:** A Chi-Square test of independence on monthly close rates indicates statistical significance ($\chi^2 = 21.66$, $p = 0.0006$, $\text{df} = 5$).


* **Operational Insight:** The apparent Q3 decline is an operational artifact of sales maturation lag: **37 leads in September were still active in `EP Confirmed**` awaiting worksheet completion at export time. Controlling for funnel lag, organic traffic quality did not deteriorate.



### 2. Drivers of Lead Quality (Segmentation)

* **Publisher Channel:** Inbound call center leads (`DebtReductionCallCenter`) convert at **9.6%** (16.2% Good Rate) vs. online web forms at **8.0%** (12.7% Good Rate) due to live agent pre-screening.


* **Creative Alignment:** Branded ads (`creditsolutions-branded-shortform`) outperform generic forms (`Debt Settlement1 Master`), preventing drop-off when the sales team reaches out.


* **Form Structure:** Two-page forms (`2DC`) introduce intentional qualification friction, filtering out accidental clicks and delivering higher close rates than low-friction single-page forms (`1DC`).


* **Debt Bracket:** Debt tiers between **$10k–$30k** and **$70k–$90k** achieve top-tier conversion (11.7%–13.7%). Leads reporting under **$10k** fail qualification at a 24.8% rate (`Contacted - Doesn't Qualify`).


* **Identity Validation:** Third-party identity scores (`PhoneScore` $\le 2$, `AddressScore` $\le 2$) identify invalid/uncontactable leads with $>58\%$ error rates.



### 3. Commercial Opportunity: +20% CPL vs. +20% Quality

* **Target:** Raise conversion quality from 8.1% to **9.6%** to secure a **$36.00 CPL** (+20% payout).


* **Operational Action:** Real-time API rejection of submissions with `PhoneScore` $\le 2$ and auto-disqualification of debts under $10,000.


* **Unit Economics Model:**

| Monetization Strategy | Volume | Average CPL | Gross Revenue | Net Revenue vs. Baseline |
| --- | --- | --- | --- | --- |
| **Status Quo Baseline** | 3,021

 | $30.00

 | **$90,630** | Baseline |
| **Single-Buyer Cull (-18.9% Vol)** | 2,450 | $36.00

 | **$88,200** | -$2,430 (-2.7%)

 |
| **Tiered Routing (Recommended)** | 2,450 (Tier 1) + 571 (Tier 2) | $36.00 / $14.00

 | **$96,194** | **+$5,564 (+6.1%)**<br> |

---

## 🛠️ Data Modeling & Power BI Architecture

### Funnel KPIs (DAX Measures)

```dax
-- Total Lead Volume
Total Leads = COUNTROWS(LeadsData)

-- Closed Customers (Bottom Funnel Success)
Closed Leads = CALCULATE(COUNTROWS(LeadsData), LeadsData[CallStatus] = "Closed")

-- Close Rate %
Closed % = DIVIDE([Closed Leads], [Total Leads], 0)

-- Broad Lead Quality (Middle & Bottom Funnel)
Good Leads = 
CALCULATE(
    COUNTROWS(LeadsData),
    LeadsData[CallStatus] IN {"Closed", "EP Confirmed", "EP Sent", "EP Received"}
)

-- Lead Quality %
Lead Quality % = DIVIDE([Good Leads], [Total Leads], 0)

```

### Funnel Sort Order (Power Query M Code)

To prevent circular dependency errors in Power BI DAX:

```powerquery
if [CallStatus] = "Unable to contact - Bad Contact Information" then 1
else if [CallStatus] = "Contacted - Invalid Profile" then 2
else if [CallStatus] = "Contacted - Doesn't Qualify" then 3
else if [CallStatus] = "Unknown" or [CallStatus] = null then 4
else if [CallStatus] = "EP Sent" then 5
else if [CallStatus] = "EP Received" then 6
else if [CallStatus] = "EP Confirmed" then 7
else if [CallStatus] = "Closed" then 8
else 9

```

---

## 📁 Repository Structure

```text
├── dataset/
│   └── Analyst_case_study_dataset_1_(1).xls    # Raw acquisition & disposition dataset[cite: 1]
├── docs/
│   ├── Assignment_Prompt.pdf                   # Case study requirements[cite: 1]
│   └── Executive_Summary.md                    # C-Suite brief & findings
├── powerbi/
│   └── Performance_Marketing_Report.pbix       # Interactive Power BI dashboard[cite: 11]
├── reports/
│   └── Final_Analyst_Briefing.pdf              # Final report deliverable[cite: 11]
└── README.md                                   # Project documentation

```

---

## 🚀 Key Takeaways & Recommendations

1. **Deploy Real-Time Phone Validation:** Reject `PhoneScore <= 2` at the form level to stop paying for disconnected numbers and fake inputs.


2. **Shift Ad Spend to Inbound & Branded Creatives:** Prioritize inbound call centers and `CreditSolutions` branded form variants.


3. **Execute Tiered Monetization:** Never cull sub-threshold leads unilaterally. Route primary qualified volume to the primary advertiser at $36 CPL and monetize the remainder via secondary credit counseling networks at $14 CPL to maximize gross margins.

---

## 📬 Contact

If you have questions or would like to discuss this project, feel free to connect:

**Nikhil Kattaguri**

- [LinkedIn](https://www.linkedin.com/in/nikhilkattaguri)
- [Email](mailto:nikhilkattaguri27@gmail.com)
