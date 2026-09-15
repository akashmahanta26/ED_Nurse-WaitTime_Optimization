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
## 4. Optimization Model

The patient scheduling problem is formulated as a Mixed-Integer Optimization Model with the objective of minimizing the total cumulative waiting time across all patients and stages.

### Objective Function

Minimize the total cumulative waiting time across all $m$ patients ($m=300$) and five stages ($k \in \{1,\ldots,5\}$):

$$
\min \sum_{i=1}^{m}\sum_{k=1}^{5} waiting\_time_{i,k}
$$

### Key Decision Variables

- $y_{j,k} \in \{0,1\}$: Binary variable indicating whether nurse $j$ is assigned to stage $k$.
- $x_{i,k,a} \in \{0,1\}$: Binary variable indicating whether patient $i$ is treated in stage $k$ during time slot $a$.
- $waiting\_time_{i,k} \geq 0$: Continuous variable representing the waiting time of patient $i$ before service at stage $k$.

### Core Constraints

The optimization model incorporates the following key constraints:

1. **Nurse Assignment:** Each nurse is assigned to exactly one stage, while each stage must have at least one nurse.

2. **Capacity per Time Slot:** The number of patients assigned to a time slot cannot exceed the number of nurses allocated to that stage.

3. **Sequential Patient Flow:** Patients can only be assigned to a stage after arriving at that stage.

4. **Stage-Specific Treatment Rules:** Patients can only be assigned to eligible stages and time slots according to their treatment requirements.

5. **Patient Treatment Order:** Patients are scheduled according to the defined arrival and treatment sequence.

6. **Waiting Time Calculation:** Waiting time is calculated as the difference between the patient's assigned treatment slot and their arrival time at the respective stage.

<img width="705" height="551" alt="image" src="https://github.com/user-attachments/assets/591ee38b-7d31-4deb-9183-3b373acbeb26" />

> **Note:** The equations shown above represent the core formulation of the optimization model. The complete mathematical formulation, including all decision variables, constraints, assumptions, and Big-M formulations, is provided in the accompanying technical report.




## 🛠️ Tech Stack & Prerequisites
Language: Python 3.10+

Optimization Framework: Pyomo / gurobipy

Solver: Gurobi Optimizer (MIPGap set to 0.01 / 1%)

Data Processing: pandas, numpy

Visualization: matplotlib, seaborn

## 🏥 Emergency Department Workflow

Patients move sequentially through up to 5 operational stages:

[ ARRIVAL ] ──► (1. Resuscitation)* ──► 2. Registration ──► 3. Triage ──► 4. Consultation ──► 5. Observation ──► [ DISCHARGE / ADMISSION ] 






