# Trust & Safety Risk Analysis System
## Statistical Investigation & Abuse Pattern Detection in User Activity Logs

![Python 3.10+](https://img.shields.io/badge/python-3.10+-blue)
![scikit-learn](https://img.shields.io/badge/scikit--learn-1.3-orange)
![Azure DevOps](https://img.shields.io/badge/CI%2FCD-Azure%20DevOps-blue)
![License: MIT](https://img.shields.io/badge/license-MIT-green)

> Full source code and CI/CD pipeline on Azure DevOps:  
> 🔗 [View Project on Azure DevOps](https://dev.azure.com/AIML-DataAnalytics-Projects/_git/AuditRiskScoring)

---

## What This Project Does

Replicates the **end-to-end analytical workflow** a Trust & Safety analyst uses to investigate abuse in user activity logs:

1. **Detect** - statistical outlier detection (Z-scores) + ML-assisted risk scoring surfaces the top 2% of events for analyst review
2. **Investigate** - cohort comparison, trend analysis, and policy effectiveness measurement quantify *how* and *when* abuse is occurring
3. **Explain** - every flagged event includes a plain-language explanation ("flagged due to: external IP, off-hours access, role clearance exceeded")
4. **Report** - investigation memo and flagged account CSV ready to share with policy, engineering, and leadership stakeholders

---

## Problem Statement

Security and Trust & Safety teams face a core analytical challenge: identifying malicious behaviour in high-volume activity logs (50K+ events/day) where no analyst can review everything and abuse patterns evolve continuously.

This system uses **behavioural analytics** - understanding what *normal* looks like for each user to surface contextually anomalous activity at scale.

---

## Analyst Workflow

```
Raw Event Logs (50K+ events · 90 days · 300 users)
        │
        ▼
[1] Behavioural Feature Engineering - 23 signals
    Temporal · Velocity · Geographic · Role Deviation
    Session · User Baseline · Composite Risk Signal
        │
        ▼
[2] Statistical Analysis
    Z-score outlier detection     - fast first-pass filter
    Cohort comparison (t-test)    - normal vs suspicious users
    Trend analysis                - week-over-week abuse rate
    Policy effectiveness          - before/after intervention
        │
        ▼
[3] ML-Assisted Risk Scoring (supporting role)
    Isolation Forest ranks 50K events
    Analyst reviews top 2% - evaluated with Precision@K
        │
        ▼
[4] Investigation Report
    Stakeholder memo · Flagged accounts CSV · Recommendations
```

---

## Abuse Patterns Detected

| Pattern | Key Signals | Real-World Analogue |
|---------|-------------|---------------------|
| **Account Takeover** | New external IP + off-hours + sensitive resource | Compromised credential used by threat actor |
| **Credential Stuffing** | Rapid login_failure burst from rotating IPs | Automated password spray attack |
| **Data Exfiltration** | Bulk export from low-privilege role | Insider copying data before resignation |
| **Privilege Escalation** | Resource sensitivity exceeds role clearance | User accessing systems above authorisation |
| **Insider Threat** | Trusted user + off-hours + unusual event sequence | Employee exfiltrating intellectual property |

---

## Statistical Methods

### Z-Score Outlier Detection
Fast first-pass filter - no model needed, instantly explainable:
```python
z_score = (bytes_transferred - mean) / std
# e.g. user transferred 189,916 bytes vs mean of 1,017 = z = 5.2 → flagged
```

### Cohort Comparison (Welch's t-test)
Quantifies how different suspicious users are from normal users:

| metric | normal_mean | suspicious_mean | ratio | p_value |
|--------|-------------|-----------------|-------|---------|
| is_external_ip | 0.000 | 0.221 | 638.5x | <0.0001 |
| bytes_transferred | 1,017 | 189,916 | 186.7x | <0.0001 |
| login_failures_recent | 0.30 | 8.70 | 29x | 0.0003 |

### Policy Effectiveness (Two-Proportion Z-Test)
Before/after analysis with statistical significance testing - answers "did our intervention actually reduce abuse?"

### Trend Analysis
Daily abuse rate with 7-day rolling average and week-over-week change.

---

## Results

| Metric | Score |
|--------|-------|
| ROC-AUC | **0.9998** |
| Average Precision | **0.9852** |
| Precision@10 | **1.0** - top 10 flagged = 10/10 real abuse |
| Precision@50 | **1.0** - top 50 flagged = 50/50 real abuse |
| Precision@100 | **1.0** |
| Recall | **1.0** |

---

## Sample Investigation Memo Output

```
╔══════════════════════════════════════════════════════════════════╗
║          TRUST & SAFETY  ·  RISK INVESTIGATION MEMO             ║
╚══════════════════════════════════════════════════════════════════╝

EXECUTIVE SUMMARY
Analysed 50,440 events across 300 accounts.
Flagged 1,009 high-risk events (2.0%) affecting 88 unique accounts.

ABUSE PATTERN BREAKDOWN
  credential_stuffing     222 events  (22.0%)
  account_takeover        106 events  (10.5%)
  insider_threat           45 events   (4.5%)
  data_exfiltration        36 events   (3.6%)

KEY SIGNALS (p < 0.05)
  is_external_ip          638.5x above normal
  bytes_transferred       186.7x above normal
  login_failures_recent    29x above normal

RECOMMENDATIONS
  1. IMMEDIATE: Review top-50 flagged accounts
  2. SHORT-TERM: Rate-limit login failures >10 in 60 seconds
  3. MEDIUM-TERM: Tighten viewer/external role access controls
```

## Sample Flagged Account Output

```
user_id    risk_score  analyst_explanation
user_0144  1.000       Flagged due to: external IP; off-hours activity;
                       role clearance exceeded; large transfer (358,063 bytes)
user_0130  0.996       Flagged due to: external IP; off-hours activity;
                       first-time IP subnet
user_0203  0.988       Flagged due to: large transfer (4,821,302 bytes);
                       off-hours activity
```

---

## Tech Stack

| Component | Technology | Purpose |
|-----------|-----------|---------|
| Feature Engineering | Python, Pandas, NumPy | 23 behavioural signals |
| Statistical Analysis | SciPy (t-test, z-test) | Outlier detection, cohort comparison, policy measurement |
| ML Risk Scoring | Scikit-learn (Isolation Forest) | Prioritise analyst review queue |
| Evaluation | Scikit-learn metrics | Precision@K, ROC-AUC, Average Precision |
| CI/CD | Azure DevOps | Auto-run + quality gate on every commit |

---

## Project Structure

```
AuditRiskScoring/
├── run_pipeline.py                   # Entry point
├── data/simulate_logs.py             # 50K events, 5 abuse patterns
├── features/feature_engineering.py   # 23 behavioural signals
├── analysis/statistical_analysis.py  # Z-scores, t-tests, trends, policy effectiveness
├── models/risk_scorer.py             # Isolation Forest + Precision@K
├── reports/investigation_report.py   # Stakeholder memo + flagged CSV
└── azure-pipelines.yml               # CI/CD with quality gate
```

---

## How to Run

```bash
git clone https://dev.azure.com/AIML-DataAnalytics-Projects/_git/AuditRiskScoring
cd AuditRiskScoring
pip install -r requirements.txt
python run_pipeline.py
```

**Outputs:**
- `reports/investigation_memo.txt` - stakeholder summary
- `reports/flagged_accounts.csv` - top 100 flagged events with explanations
- `reports/daily_abuse_trend.csv` - abuse rate over time
- `artifacts/metrics.json` - model evaluation metrics

---

## CI/CD (Azure DevOps)

Every commit to `master` automatically:
1. Runs full analysis pipeline
2. **Quality gate:** fails build if ROC-AUC < 0.75
3. Publishes model artifacts and reports

---

## License

MIT

