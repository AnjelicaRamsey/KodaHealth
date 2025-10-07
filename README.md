# Koda Health Engagement Metrics Project

## Overview
This project was developed as part of a Senior Data Analyst case study for **Koda Health**. The goal was to design and implement an end-to-end data pipeline that transforms raw engagement data into actionable insights, culminating in an interactive **Engagement Metrics Dashboard** (built in Tableau).

The dashboard highlights patient engagement across multiple communication channels (email, text, phone, and app activity) and produces an **Engagement Score** to help clinical operations teams prioritize outreach.

---

## Objectives
- **Requirements Gathering** – Clarify vague reporting requests, align on definitions of “patients,” “engagement,” and “milestones.”  
- **ETL Pipeline Design** – Build a reproducible R workflow to ingest, transform, and structure event-level data into a reporting-ready data mart.  
- **Engagement Score** – Create a first-pass scoring model to quantify patient engagement across milestones, clicks, calls, and opens.  
- **Dashboard Visualization** – Deliver a monthly client-facing report that communicates engagement patterns clearly and effectively.  

---

## Data Sources
Two categories of input data were provided:contentReference[oaicite:0]{index=0}:

1. **Engagement Data**  
   - **Email Engagement**: deliveries, opens, and clicks across 3 activation emails  
   - **Text Message Engagement**: deliveries, clicks on account creation link  
   - **Phone Call Logs**: up to 3 call attempts, outcomes (answered, voicemail, unanswered)

2. **App Data**  
   - **App Engagement Data**: referral date, account creation, guide starts, completions, signatures  
   - **User-Inputted App Data**: demographics and patient-reported preferences  

---

## ETL Process
The transformation logic is implemented in **`koda_etl.Rmd`**.  
Key steps:contentReference[oaicite:1]{index=1}:

- **Staging** raw event tables (patients, email, text, phone, app).  
- **Joining** on patient ID as the hub key.  
- **Deduplication** of emails by `email_number`; normalization of booleans.  
- **Timestamp handling** – converted to a single timezone, truncated to daily grain.  
- **Aggregation** into a `mart_patient_flat` table (1 row per patient).  
- **Quality checks** – row counts, distinct keys, null scans.  

---

## Engagement Score
Patients are assigned a **0–100 score**, rolled up into tiers (High/Medium/Low):  

- **Milestones (≤40 pts)**  
  - Account created, guide started, guide completed, guide signed (10 each)  
- **Clicks (≤30 pts)**  
  - Email/text clicks (3 pts each)  
- **Phone Calls (≤20 pts)**  
  - Answered call (4 pts), voicemail (1 pt)  
- **Opens (≤10 pts)**  
  - 1 pt per email open  

**Tiers**:  
- High = ≥40 pts  
- Medium = 20–39 pts  
- Low = <20 pts  

This definition can evolve based on clinician feedback to better reflect “real” engagement.

---

## Dashboard
The **Engagement Metrics Dashboard** (see `Koda Health Demo.twb`) surfaces:  
- **Weekly Engagement Funnel** – patient progression through referrals, account creation, guide starts/completions, and signatures.  
- **Communication Rates** – performance of emails, calls, and texts.  
- **Engagement Tier Mix by State** – segmentation by High/Medium/Low engagement.  
- **Avg. Engagement Score by State** – geographic performance comparison.  
- **Summary KPIs** – Accounts Created, Guides Started/Completed, Avg. Engagement Score, Completion %, High Engagement %.  

---

## Deliverables
- `koda_etl.Rmd` – ETL pipeline in R  
- `Koda Health Demo.twb` – Tableau dashboard workbook  
- `Koda Health Case Study.pptx` – Presentation slides summarizing findings  
- `Instructions.pdf` – Case study brief  
- `dashboard.png` – Dashboard screenshot

---

## Key Insights
- Engagement fluctuated weekly with spikes around Jan 1 and Jan 21.  
- Email open rates were strong (70–80%), but click rates lagged (30–35%).  
- Call answer rates ~60%; text click rates ~20–40%.  
- Only ~50% of patients reached “High” engagement; completion rate 56%.  
- Average Engagement Score was **39**, just below the High tier threshold:contentReference[oaicite:2]{index=2}.  

---

## Next Steps
If expanded into production:
- Automate incremental loads into a warehouse.  
- Add automated data quality tests.  
- Incorporate patient profile changes over time.  
- Iterate on Engagement Score with clinical stakeholders.  

---

## Author
Created by **Anjelica Ramsey** as part of a case study submission.
