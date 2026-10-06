# Scholarship Candidate Selection & Allocation System

An interactive, web-based decision support tool designed to automate, optimize, and rank scholarship candidate allocations using **Weighted Composite Scoring** alongside **0/1 Knapsack Dynamic Programming** and **Greedy Ranking** algorithms.

---

## Table of Contents
- [Overview](#overview)
- [Key Features](#key-features)
- [Algorithmic Models](#algorithmic-models)
  - [1. Composite Score Calculation](#1-composite-score-calculation)
  - [2. 01 Knapsack Dynamic Programming (Optimal)](#2-01-knapsack-dynamic-programming-optimal)
  - [3. Greedy Allocation (Priority Rank)](#3-greedy-allocation-priority-rank)
- [Getting Started](#getting-started)
- [How to Use](#how-to-use)
- [CSV Import/Export Format](#csv-importexport-format)
- [Tech Stack](#tech-stack)

---

## Overview

When allocating financial awards and scholarships under strict budget constraints, committee members face two common challenges:
1. **Fair candidate scoring:** Balancing academic marks, extracurricular achievements, and financial need into a transparent, composite rank.
2. **Optimal budget utilization:** Maximizing the total impact/merit score of awarded candidates without exceeding available funds, especially when scholarship request amounts vary per student.

This web application solves both problems by letting users customize criterion weights, set budget caps, compare baseline greedy ranking against mathematically optimal Dynamic Programming (0/1 Knapsack), and visualize DP state transitions step-by-step.

---

## Key Features

- **Flexible Allocation Modes:**
  - **Dynamic Programming (0/1 Knapsack):** Guarantees maximum combined merit score within budget, handling variable requested grant amounts.
  - **Greedy Strategy:** Allocates sequentially down the ranked score list until budget runs out.
- **Custom Composite Scoring:** Real-time sliders for Marks, Achievements, and Financial Need weights (automatically normalized to 100%).
- **Variable vs. Fixed Award Amounts:** Support for individual candidate requested award amounts or fixed uniform amounts per recipient.
- **Interactive DP Matrix Visualizer:** Live-rendered dynamic programming matrix demonstrating row-by-row cell transitions and backtracking traces for pedagogical insight.
- **Data Persistence & Management:** Automatically saves applicant pools to `localStorage` across page reloads.
- **Sample Presets:** Pre-loaded realistic test datasets (Standard Pool, Need-based focus, High-value variable grants, Budget bottleneck scenarios).
- **CSV Data Pipeline:** Full CSV import (drag-and-drop or file selector) and formatted CSV export for reporting.
- **Search & Filtering:** Real-time search by candidate name and filter views by awarded status.

---

## Algorithmic Models

### 1. Composite Score Calculation

Each applicant $i$ receives a normalized composite score $S_i \in [0, 100]$ computed using user-defined weights for Marks ($w_m$), Achievements ($w_a$), and Financial Need ($w_n$):

$$S_i = \frac{M_i \cdot w_m + A_i \cdot w_a + N_i \cdot w_n}{w_m + w_a + w_n}$$

*Where $M_i, A_i, N_i \in [0, 100]$ represent the individual metric scores for applicant $i$.*

---

### 2. 0/1 Knapsack Dynamic Programming (Optimal)

To maximize the overall impact score while respecting budget constraint $B$:

- **Item Weight ($c_i$):** Scholarship grant amount requested by student $i$.
- **Item Value ($v_i$):** Candidate's composite score $S_i$.
- **Knapsack Capacity ($B$):** Total available scholarship budget fund.

#### Mathematical Form
$$\text{Maximize } \sum_{i=1}^{N} S_i \cdot x_i \quad \text{subject to} \quad \sum_{i=1}^{N} c_i \cdot x_i \le B, \quad x_i \in \{0, 1\}$$

#### DP Transition Formula
For applicant index $i \in [1, N]$ and available capacity $b \in [0, B]$:

$$DP[i][b] = \begin{cases}  DP[i-1][b], & \text{if } c_i > b \\ \max\left(DP[i-1][b], \, DP[i-1][b - c_i] + S_i\right), & \text{if } c_i \le b  \end{cases}$$

- **Time Complexity:** $\mathcal{O}(N \times B)$
- **Space Complexity:** $\mathcal{O}(N \times B)$ (for step backtracking and visual state tracing)

---

### 3. Greedy Allocation (Priority Rank)

Applicants are sorted in descending order of composite score $S_i$. Awards are allocated sequentially to candidates as long as $c_i \le \text{remaining\_budget}$. 

*Note: While fast ($\mathcal{O}(N \log N)$), Greedy allocation can leave unspent budget gaps when candidate award amounts vary drastically.*

---

## Getting Started

No build tools, web servers, or third-party dependencies (like React or jQuery) are required.

1. Clone or download this repository.
2. Ensure `index.html` is saved locally.
3. Double-click `index.html` to launch it directly in any modern web browser (Chrome, Edge, Firefox, Safari).

---

## How to Use

1. **Configure Settings:**
   - Set total budget amount ($) and award amount mode (Variable vs. Fixed).
   - Adjust criterion weights for Marks, Achievements, and Need using the sliders.
   - Choose your **Allocation Method**: *0/1 Knapsack (Optimal)* or *Greedy Rank*.
2. **Add Applicants:**
   - Input student name, marks, achievements, financial need, and grant amount individually, OR
   - Click **"Load Sample Pool"** to test immediately with benchmark datasets.
3. **Execute & Review:**
   - Click **"Rank & Allocate"** to run the algorithm.
   - Inspect summary statistics (Total Allocated, Budget Utilization %, Award Count, Mean Awarded Score).
   - View detailed decisions ("Awarded" / "Not Awarded") in the applicant table.
4. **Visualize DP Steps:**
   - In 0/1 Knapsack mode, scroll down to the **"DP Execution Matrix"** panel to examine exact capacity states and trace back paths.
5. **Export Results:**
   - Click **"Export CSV"** to save your final candidate rankings and allocation decisions.

---

## CSV Import/Export Format

The system supports CSV files formatted with the following column headers:

```csv
Name,Marks,Achievements,Need,AwardRequested
"Alice Rivera",92,85,70,1500
"Bob Chen",78,90,95,2000
"Charlie Davis",88,60,40,1000