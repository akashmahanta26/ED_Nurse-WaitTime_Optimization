# Emergency Department Patient Flow & Nurse Scheduling Optimization

[![Python 3.10+](https://img.shields.io/badge/python-3.10+-blue.svg)](https://www.python.org/)
[![Optimization-Gurobi](https://img.shields.io/badge/Solver-Gurobi-red.svg)](https://www.gurobi.com/)
[![Dataset-MIMIC--IV--ED](https://img.shields.io/badge/Dataset-MIMIC--IV--ED-green.svg)](https://physionet.org/content/mimic-iv-ed/)
[![Institution-NUS](https://img.shields.io/badge/NUS-DBA5103-orange.svg)](https://www.nus.edu.sg/)

A Mixed-Integer Linear Programming (MILP) framework developed to minimize patient waiting times in hospital Emergency Departments (ED) by optimizing nursing staff allocation across treatment stages during peak operational hours.

---

## 📌 Executive Summary

Emergency departments frequently suffer from bottlenecking during peak daytime hours (9:00 AM – 4:00 PM) due to resource-demand mismatches. This project models patient trajectory through a 5-stage ED workflow using sampled real-world clinical data from **MIMIC-IV-ED** (scaled to Singapore Ministry of Health daily ED benchmarks). Using **Pyomo/Gurobi**, our MILP solver determines the mathematical lower bounds for staffing and optimal nurse allocation strategies to alleviate patient delay.

### Key Findings
* **Staffing Threshold:** A minimum of **50 nurses** is required to process 300 daytime patients within a 24-hour cycle; anything fewer results in system infeasibility.
* **Optimal Allocation Point:** Increasing staff from 50 to **80 nurses** reduces average patient waiting time by **21.1%** (from 1.47 hrs to 1.16 hrs).
* **Diminishing Returns:** Staffing beyond 80 nurses yields zero additional waiting time reduction due to discrete slot scheduling constraints and peak arrival distributions.
* **Bottleneck Mitigation:** Over **30% of newly added capacity** must be directed to the **Observation** stage (120-minute slot duration) to prevent downstream queue buildup.
* **Benchmark Alignment:** The optimal allocation yields a nurse-to-patient ratio of **1:3.75**, closely matching Singapore public hospital standards (1:4 to 1:5).


## Mathematical Model (MILP)

<img width="729" height="448" alt="image" src="https://github.com/user-attachments/assets/9945b2fe-eca7-4916-8688-cef2a3bc7ece" />

<img width="705" height="551" alt="image" src="https://github.com/user-attachments/assets/591ee38b-7d31-4deb-9183-3b373acbeb26" />

## 🛠️ Tech Stack & Prerequisites
Language: Python 3.10+

Optimization Framework: Pyomo / gurobipy

Solver: Gurobi Optimizer (MIPGap set to 0.01 / 1%)

Data Processing: pandas, numpy

Visualization: matplotlib, seaborn

## 🏥 Emergency Department Workflow

Patients move sequentially through up to 5 operational stages:

[ ARRIVAL ] ──► (1. Resuscitation)* ──► 2. Registration ──► 3. Triage ──► 4. Consultation ──► 5. Observation ──► [ DISCHARGE / ADMISSION ] 






