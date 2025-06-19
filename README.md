# AI-Driven Audit Log Risk Scoring (Azure DevOps MLOps Pipeline)

This project demonstrates how **AI and DevOps** can be integrated to automate **risk scoring** of financial audit logs using unsupervised learning. Built and executed entirely on **Azure DevOps**, this project highlights real-world **MLOps practices** for internal audit modernization.

## Highlights

- Applied **Isolation Forest** to detect anomalies in simulated audit logs
- Built a fully automated **CI/CD pipeline** in Azure DevOps for data simulation, model scoring, and result logging
- Showcased reproducible and modular **Python + YAML**-based AI workflow
- Demonstrates transparency and automation for audit and compliance workflows

## Public Azure DevOps Repository

> 🔗 [View the full project on Azure DevOps](https://dev.azure.com/AIML-DataAnalytics-Projects/_git/AuditRiskScoring)

You’ll find:
- All code: `simulate_audit_data.py`, `run_pipeline.py`, model artifacts
- Azure pipeline YAML (`azure-pipelines.yml`)
- Logs and artifact history
- Public pipeline execution dashboard

## Tech Stack

| Tool             | Role                                |
|------------------|-------------------------------------|
| Python           | ML modeling and simulation logic    |
| Scikit-learn     | Unsupervised model (Isolation Forest) |
| Azure DevOps     | CI/CD and orchestration             |
| YAML             | Pipeline-as-Code configuration      |

## Sample Output (Top 5 Anomalies)

| Timestamp           | User         | Action       | Entity       | Risk Score | Anomaly |
|---------------------|--------------|--------------|--------------|------------|---------|
| 2023-02-09 23:00:00 | admin        | delete       | accounts     | 0.0491     | True    |
| 2023-01-31 12:00:00 | guest        | modify       | payroll      | 0.0291     | True    |
| 2023-01-22 01:00:00 | admin        | access       | inventory    | 0.0259     | True    |
| 2023-01-12 01:00:00 | admin        | access       | payroll      | 0.0170     | True    |
| 2023-01-19 02:00:00 | guest        | modify       | billing      | 0.0144     | True    |

## Use Case

This project is ideal for:
- Internal audit teams seeking automation
- Risk analysts evaluating log-based anomalies
- Engineers implementing **CI/CD for ML** pipelines

