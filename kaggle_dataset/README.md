# LoCoMo Conversational Memory Evaluation Slice

## Overview
This dataset provides a curated, multi-session conversational evaluation slice derived from the **LoCoMo** benchmark (*Maharana et al., ACL 2024*). It is designed to rigorously evaluate long-term conversational memory architectures for Large Language Model (LLM) agents across multiple longitudinal sessions.

Developed for the **EPITA School of Computer Science (Paris)** MSc Data Science & Artificial Intelligence Action Learning Project (Team DSA 3: Mekala Naveen Venkata Sai Kumar, Lakhveer Singh, Krishna Chetry).

---

## Dataset Contents

The dataset is provided in both relational tabular (CSV) and nested hierarchical (JSON) formats:

1. **`locomo_dialogue_turns.csv`** (48 rows):
   - `global_turn_id`: Chronological index across all sessions (1 to 48).
   - `session_id`: Session identifier (`Session_01` through `Session_12`).
   - `session_timestamp`: ISO 8601 simulated date and time of the interaction.
   - `session_turn_id`: Turn index within the individual session (1 to 4).
   - `speaker`: `User` or `Assistant`.
   - `text`: Utterance text content.
   - `word_count`: Total whitespace-delimited words.
   - `approx_tokens`: Estimated sub-word token count (~1.3 tokens/word).

2. **`locomo_evaluation_probes.csv`** (10 rows):
   - `query_id`: Probe identifier (`Q01` to `Q10`).
   - `question`: Evaluator inquiry testing knowledge retention from prior sessions.
   - `ground_truth`: Target factual information extracted from conversation history.
   - `fact_type`: Category of memory tested:
     - `Entity/Name`: Explicit named entities (e.g., pet name, sister's name).
     - `Numerical/Date`: Temporal references and numbers (e.g., marathon target date, flight departure).
     - `User Preference`: Expressed personal preferences (e.g., coffee roast, dietary choices).
     - `Overwrites/Updates`: Factual revisions over time (e.g., changed address, updated workout schedule).
     - `Event/Activity`: Historical episodic events.
   - `evidence_turns`: Comma/semicolon-separated turn indices containing source ground truth.
   - `session_id`: Source session where the fact was originally stated or updated.

3. **`locomo_evaluation_slice.json`**:
   - The unified, structured schema containing the metadata, full multi-session dialogue trees, and ground truth probes in a single JSON structure.

4. **`dataset-metadata.json`**:
   - Standard Kaggle metadata specification for CLI/API publication.

---

## Quickstart in Kaggle Notebooks

### 1. Load Dialogue Turns (Pandas)
```python
import pandas as pd

# Load dialogue history
df_turns = pd.read_csv('/kaggle/input/locomo-conversational-memory/locomo_dialogue_turns.csv')
print(f"Loaded {len(df_turns)} turns across {df_turns['session_id'].nunique()} sessions.")
display(df_turns.head())
```

### 2. Load Evaluation Probes
```python
df_probes = pd.read_csv('/kaggle/input/locomo-conversational-memory/locomo_evaluation_probes.csv')
print(f"Loaded {len(df_probes)} probes:")
display(df_probes[['query_id', 'fact_type', 'question', 'ground_truth']])
```

### 3. Load Nested JSON
```python
import json

with open('/kaggle/input/locomo-conversational-memory/locomo_evaluation_slice.json', 'r') as f:
    data = json.load(f)

print("Benchmark:", data['benchmark'])
print(f"Total sessions: {data['total_sessions']}, Total turns: {data['total_turns']}")
```

---

## Baseline Architectures Evaluated

This dataset is benchmarked across three core memory architectures:
1. **Full-History Concatenation**: Naive sliding window / full prompt stuffing. Demonstrates "Lost in the Middle" attention degradation and high latency.
2. **Recursive Structured Summarisation**: Periodic session abstraction maintaining running factual profiles with low token footprint.
3. **Conversational Semantic RAG**: Dense vector embeddings (`all-MiniLM-L6-v2`) with ChromaDB/FAISS vector retrieval for constant-context factual recall.

---

## Citation & References

```bibtex
@inproceedings{maharana2024locomo,
  title={LoCoMo: Evaluating Long-Context Memory Architectures in Conversational Agents},
  author={Maharana, Adyasha and others},
  booktitle={Proceedings of the 62nd Annual Meeting of the Association for Computational Linguistics (ACL)},
  year={2024}
}
```

- **Authors**: Mekala Naveen Venkata Sai Kumar, Lakhveer Singh, Krishna Chetry
- **Institution**: MSc DSA, EPITA School of Computer Science, Paris, France
- **License**: Creative Commons Attribution 4.0 International (CC-BY-4.0)