# Comparative Evaluation of Memory Management Strategies in Long-Term Conversational AI Agents

[![Python 3.10+](https://img.shields.io/badge/python-3.10+-blue.svg)](https://www.python.org/downloads/)
[![EPITA MSc DSA](https://img.shields.io/badge/EPITA-MSc%20DSA%20Action%20Learning-003366.svg)](https://www.epita.fr/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

Official experimental repository for the **EPITA Action Learning Master's Thesis** (Fall 2026 – Spring 2027 Cohort).

* **Institution**: EPITA School of Engineering and Computer Science, Paris
* **Program**: Master of Science in Data Science & Analytics (MSc DSA)
* **Team**:
  * **Lakhveer Singh** — Task 1: Full-History Context Strategies
  * **Mekala Naveen Venkata Sai Kumar** — Task 2: Summarisation Strategies
  * **Krishna Chetry** — Task 3: Conversational Retrieval Strategies
* **Faculty Advisors**: Prof. Alaa Bakhti & Prof. Rosemary Obi

---

## 🚀 Quick Access — Run in Google Colab

Each teammate's modeling notebook is pre-configured and can be launched directly in Google Colab with one click:

| Task Owner | Research Focus | One-Click Colab Launch |
|---|---|---|
| **Lakhveer Singh** | Task 1: Full-History Context & Lost in the Middle | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/Naveen-Mekala/conversational-memory-evaluation/blob/main/notebooks/Lakhveer_Singh_Task1_Full_History_Context.ipynb) |
| **Mekala Naveen** | Task 2: Summarisation Strategies & Fact Vulnerability (G3) | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/Naveen-Mekala/conversational-memory-evaluation/blob/main/notebooks/Mekala_Naveen_Task2_Summarisation_Strategies.ipynb) |
| **Krishna Chetry** | Task 3: Retrieval Strategies & Decoupled Diagnostics (G4) | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/Naveen-Mekala/conversational-memory-evaluation/blob/main/notebooks/Krishna_Chetry_Task3_Conversational_Retrieval.ipynb) |

---

## 📖 Research Framework & Objectives

As LLM agents transition from ephemeral assistants into persistent conversational partners, memory management faces an inescapable **tri-factor operational tension**:
1. **Token Consumption Escalation**: Naive cumulative history incurs quadratic self-attention costs and saturates context limits.
2. **Response Latency Overhead**: Expanding prompts inflates Time-to-First-Token (TTFT), violating the 2–3 second interactive latency threshold.
3. **Factual Degradation & Distraction**: Context capacity $\neq$ utilization. Models suffer position bias (*"Lost in the Middle"*; Liu et al., 2024), temporal decay (Jia et al., 2025), and temporal overwrite confusion (Ong et al., 2025).

This repository conducts an apples-to-apples comparative evaluation of the three primary non-parametric memory paradigms on a unified slice of the **LoCoMo** multi-session benchmark (Maharana et al., 2024).

---

## 📊 Consolidated Modeling Results

All three tasks were benchmarked under identical experimental controls (same multi-session traces, frozen LLM backend, greedy decoding, and 10 targeted probes):

| Memory Strategy | Task Owner | Mean Token F1 | Prompt Tokens | Response Latency | Primary Failure Mode / Trade-off |
|---|---|---|---|---|---|
| **Complete History (Baseline)** | Lakhveer | 0.784 | 1,842 tok | 1.38s | 35% recall drop in middle turns (Lost in Middle); 5.9x token cost |
| **Sliding Window ($N=12$)** | Lakhveer | 0.368 | 312 tok | 0.30s | Catastrophic forgetting: 0.0 F1 on 60% of past queries |
| **Truncated History (500 tok)** | Lakhveer | 0.445 | 498 tok | 0.39s | Drops all conversational context prior to Turn 33 |
| **Fixed-Length Summary (V1)** | Naveen | 0.742 | 245 tok | 0.38s | Overwrite blindness on temporal updates; prunes exact dates |
| **Periodic Summary (V2)** | Naveen | 0.815 | 320 tok | 0.44s | Moderate compression; latency spikes at update intervals |
| **Hierarchical Summary (V3)** | Naveen | **0.912** | **475 tok** | **0.52s** | **Pareto Sweet Spot**: 74% token reduction vs. complete history |
| **BM25 Lexical Retrieval** | Krishna | 0.799 | 185 tok | 0.35s | 78% retrieval misses on paraphrased queries (Gap G4) |
| **Dense Semantic Vector** | Krishna | 0.793 | 185 tok | 0.40s | Inability to distinguish exact dates and numerical codes |
| **Hybrid RRF Retrieval ($k=60$)**| Krishna | **0.927** | **185 tok** | **0.42s** | **Top Recall Accuracy**: 0.965 Recall@3; 90% token reduction |

---

## 📂 Repository Structure

```
conversational-memory-evaluation/
├── README.md                                       # Comprehensive project overview & Colab launchpad
├── requirements.txt                                # Project dependencies
├── DSA3_Notebook_Links.txt                         # Official EPITA submission manifest
├── notebooks/
│   ├── Lakhveer_Singh_Task1_Full_History_Context.ipynb       # Task 1 executed notebook
│   ├── Mekala_Naveen_Task2_Summarisation_Strategies.ipynb    # Task 2 executed notebook
│   └── Krishna_Chetry_Task3_Conversational_Retrieval.ipynb   # Task 3 executed notebook
├── data/
│   └── locomo_evaluation_slice.json                # Common 12-session LoCoMo benchmark slice
├── slides/
│   └── DSA3_Modeling_Defense.pptx                  # 12-slide widescreen presentation deck
└── docs/
    ├── Literature_Review_Outline_EPITA.docx        # Deliverable 1 (Outline)
    ├── Final_Literature_Review_EPITA.docx          # Deliverable 2 (Completed Lit Review Essay)
    └── Defense_Script_and_Talking_Points.md        # 15-minute speaking script & jury Q&A
```

---

## 🛠️ Local Setup & Quickstart

To run the experiments locally:

```bash
# 1. Clone the repository
git clone https://github.com/Naveen-Mekala/conversational-memory-evaluation.git
cd conversational-memory-evaluation

# 2. Create and activate a virtual environment
python3 -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

# 3. Install dependencies
pip install -r requirements.txt

# 4. Launch Jupyter Notebook
jupyter notebook
```

---

## 🎯 Defense & Milestone Roadmap

* **Deliverable 1 (Literature Review Outline)**: Submitted 20 November 2026 (Completed).
* **Deliverable 2 (Completed Literature Review)**: Due 4 December 2026, 18:00 CET.
* **🎯 Modeling Tasks Defense**: Week of 7 December 2026 (15-minute presentation & live demos).
* **Hackathon 2 (MVP Sprint)**: 17 December 2026 (Integrating modeling pipelines into the Evaluation Studio).
* **Final Thesis Submission**: 4 January 2027, 18:00 CET.
* **Action Learning Week & Final Pitch**: 18–22 January 2027.

---

## 📜 Citation & Academic References

The modeling tasks and benchmarks in this repository build upon:
* **LoCoMo**: Maharana et al. (ACL 2024), *Evaluating Very Long-Term Conversational Memory of LLM Agents*.
* **Lost in the Middle**: Liu et al. (TACL 2024), *How Language Models Use Long Contexts*.
* **SeCom**: Pan et al. (ICLR 2025), *On Memory Construction and Retrieval for Personalized Conversational Agents*.
* **MemoryAgentBench**: Hu et al. (ICLR 2026), *Evaluating Memory in LLM Agents via Incremental Multi-Turn Interactions*.
* **SimpleMem**: Liu et al. (ICML 2026 PMLR), *Efficient Lifelong Memory for LLM Agents*.
