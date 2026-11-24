# EPITA Action Learning — Modeling Tasks Defense Script & Talking Points

**Project Title**: Comparative Evaluation of Memory Management Strategies in Long-Term Conversational AI Agents  
**Team**: Lakhveer Singh (Task 1), Mekala Naveen Venkata Sai Kumar (Task 2), Krishna Chetry (Task 3)  
**Total Defense Duration**: 15 Minutes (Strictly ~5 Minutes per Teammate)  
**Jury**: Prof. Alaa Bakhti & Prof. Rosemary Obi  

---

## Overall Structure & Flow

```
00:00 - 01:00 | Team Intro & Overarching Framework (Joint / Naveen leads)
01:00 - 05:00 | Task 1: Lakhveer Singh (3 min slides + 1 min demo + 1 min Q&A)
05:00 - 09:00 | Task 2: Mekala Naveen Venkata Sai Kumar (3 min slides + 1 min demo + 1 min Q&A)
09:00 - 13:00 | Task 3: Krishna Chetry (3 min slides + 1 min demo + 1 min Q&A)
13:00 - 15:00 | Cross-Task Synthesis, MVP Bridge & Wrap-up (Joint)
```

---

## 1. Joint Introduction (Slides 1–2) — 1 Minute
* **Speaker**: Mekala Naveen Venkata Sai Kumar
* **Script**:
  > "Good morning Professors. Today, our team—Lakhveer, Krishna, and myself—presents the modeling defense for our Action Learning thesis: *Comparative Evaluation of Memory Management Strategies in Long-Term Conversational AI Agents*.
  >
  > In real-world multi-session deployments, conversational agents face an inescapable **tri-factor bottleneck**: context bloat escalates token API costs, long prompts cause quadratic latency scaling violating the 2-to-3-second interactive threshold, and models suffer from position bias and temporal hallucinations.
  >
  > To solve this, our project investigates the three primary memory paradigms under a unified, apples-to-apples protocol using the LoCoMo benchmark across 12 sessions, 48 turns, and 10 ground-truth probes. We strictly controlled the LLM backend, decoding temperature, and test splits. Lakhveer investigates Full-History Context Strategies; I investigate Summarisation Strategies; and Krishna investigates Conversational Retrieval Strategies. Lakhveer will begin with Task 1."

---

## 2. Task 1: Lakhveer Singh (Slides 3–4) — 4 Minutes

### Presentation (~3 Minutes)
* **Slide 3 (Setup)**:
  > "Thank you, Naveen. In Task 1, I evaluated **Full-History Context Strategies**. Supplying verbatim dialogue directly within the prompt is the default baseline in literature. My research question asks: *How does prior conversation history selection affect recall quality, token usage, and latency as dialogues compound?*
  >
  > I implemented and compared three variations:
  > 1. **Complete History**: Supplying all prior turns that fit within the context limit.
  > 2. **Sliding Window**: Bounding the prompt to the most recent 12 conversation turns.
  > 3. **Truncated History**: Imposing a strict 500-token prompt budget, cutting off earlier turns.
  >
  > Experiments were evaluated using token-level F1, input token count, and end-to-end response latency."

* **Slide 4 (Results & Analysis)**:
  > "Looking at the empirical results on Slide 4:
  > Complete History achieved a mean F1 of **0.784**, but consumed an average of **1,842 tokens** and incurred a **1.38-second latency penalty**—a 5.9-fold increase in cost and a 4.6-fold increase in latency compared to windowed baselines.
  >
  > More critically, I discovered a severe **'Lost in the Middle' effect** (Liu et al., 2024): recall accuracy dropped by over 35% when target facts were buried in the middle 20% to 80% of long contexts filled with distractor dialogue.
  >
  > Conversely, Sliding Window and Truncation reduced latency to under 0.4 seconds, but suffered from **catastrophic long-term forgetting**: scoring **0.0 F1 on 60% of test queries** because earlier sessions were completely omitted."

### Live Demo (~1 Minute)
* **Demo Action**:
  > "In my notebook, let's look at Probe Q01 (*dog's name, Barnaby, from Session 1*) versus Probe Q07 (*mentee Marcus Vance from Session 11*).
  > Under Complete History, both are recalled, but latency spikes to 1.4 seconds. Under Sliding Window, Q07 is answered with 0.925 F1 in 0.30s, but Q01 fails completely with 0.0 F1 because Session 1 was dropped. This proves raw context windowing cannot support persistent conversational agents without external indexing."

### Anticipated Q&A (~1 Minute)
* **Jury Question**: *"Why not just use an LLM with a 1-million token context window?"*
* **Lakhveer's Defense**: *"Because context capacity does not equal context utilization. As shown in our U-curve plot and verified by Liu et al. (2024), long-context models suffer from severe attention dispersion. Furthermore, dense self-attention scales quadratically with prompt length, making 1M-token prompts economically and latently non-viable for interactive 2-second user chats."*

---

## 3. Task 2: Mekala Naveen Venkata Sai Kumar (Slides 5–6) — 4 Minutes

### Presentation (~3 Minutes)
* **Slide 5 (Setup)**:
  > "Moving to Task 2: I investigated **Summarisation Strategies for Conversational Memory Retention**. Conversation summarisation condenses historical turns into natural language summaries, directly capping token consumption. My research question asks: *How do different summarisation update methods affect an LLM's ability to retain facts, and how do they trade off recall quality against latency?*
  >
  > Crucially, I directly targeted **Research Gap G3**: prior literature reports only macro F1, obscuring how specific fact types degrade. I tested three variations:
  > 1. **Fixed-Length Summary (V1)**: A single-budget summary capped strictly at 250 tokens.
  > 2. **Periodic Incremental Summary (V2)**: Updating stored memory every 4 turns (320 tokens).
  > 3. **Hierarchical Multi-Level Summary (V3)**: Maintaining session-level summaries combined with a global thematic profile (475 tokens)."

* **Slide 6 (Results & Fact-Type Vulnerability)**:
  > "The results on Slide 6 reveal an **asymmetric fact vulnerability**:
  > - High-level **Named Entities** (e.g. Barnaby, Dr. Rostova) survived compression with **0.942 F1**.
  > - However, **Numerical Dates and exact appointment times** dropped significantly to **0.760 F1** in single-pass summaries because compression algorithms prune fine numeric digits.
  > - Most importantly, single-pass fixed summaries suffered from **temporal overwrite blindness**: when the user switched from oat milk to almond milk in Session 8, the flat summary merged both preferences or retained the older token (0.680 F1).
  >
  > The breakthrough was **Hierarchical Summarisation**: by isolating chronological sessions in child nodes and keeping a global thematic summary, it achieved **0.912 overall F1** while consuming **74% fewer tokens than complete history** (475 vs. 1,842 tokens) in just **0.52 seconds**."

### Live Demo (~1 Minute)
* **Demo Action**:
  > "In my notebook, I demonstrate Probe Q05 (*user milk preference update*).
  > When I query the Fixed-Length Summary, the model hallucinates and says 'oat milk with zero sugar'. But when querying the Hierarchical Multi-Level Summary, the model correctly answers: 'The user currently prefers almond milk with zero sugar, having switched from oat milk due to inflammation in Session 8.' This demonstrates that multi-tier memory successfully resolves temporal overwrites."

### Anticipated Q&A (~1 Minute)
* **Jury Question**: *"Doesn't generating hierarchical summaries add extra latency every time a turn happens?"*
* **Naveen's Defense**: *"We amortize that cost through asynchronous batching. Session summaries are only synthesized at session boundaries (not on every single turn), keeping the user-facing response latency to 0.52s while maintaining an up-to-date memory hierarchy."*

---

## 4. Task 3: Krishna Chetry (Slides 7–8) — 4 Minutes

### Presentation (~3 Minutes)
* **Slide 7 (Setup)**:
  > "Thank you, Naveen. In Task 3, I evaluated **Conversational Retrieval Strategies**. Retrieval-based memory (conversational RAG) treats previous dialogue turns as an external vector index, injecting only top-k relevant fragments into the prompt.
  >
  > I directly targeted **Research Gap G4**: standard RAG evaluation reports only answer accuracy, conflating retrieval misses with LLM hallucinations. I tested three variations:
  > 1. **BM25 Lexical Retrieval**: Exact keyword matching on inverted indices.
  > 2. **Dense Semantic Retrieval**: Bi-encoder sentence embeddings scoring by cosine similarity.
  > 3. **Hybrid Retrieval with Reciprocal Rank Fusion (RRF, k=60)**: Fusing lexical precision with dense semantic recall."

* **Slide 8 (Results & Decoupled Diagnostics)**:
  > "Looking at the decoupled empirical results on Slide 8:
  > - **BM25** achieved 0.750 Recall@3 and 0.799 Answer F1. It excelled at exact numeric codes (e.g. Flight LH452, Dr. Moreau), but suffered vocabulary mismatch when users paraphrased questions. My decoupled audit proved that **78% of BM25 errors were pure retrieval misses**, not model hallucinations.
  > - **Dense Semantic Retrieval** achieved 0.730 Recall@3 and 0.793 Answer F1. It handled paraphrases seamlessly (e.g. finding the Fontainebleau hike), but failed to distinguish specific numeric dates and flight numbers.
  > - **Hybrid RRF** was the definitive winner: achieving **0.965 Recall@3** and **0.927 Answer F1**. It combined the complementary strengths of lexical and dense signals.
  >
  > From an efficiency perspective, retrieval supplied only top-3 chunks (185 tokens), slashing token consumption by **90% compared to complete history** (185 vs 1,842 tokens) with sub-half-second latency (**0.42s**)."

### Live Demo (~1 Minute)
* **Demo Action**:
  > "In my notebook, let's examine Probe Q09 (*asking where the weekend hike took place without using the word Fontainebleau*).
  > BM25 retrieves turns about dog training and fails to retrieve turn 13, scoring 0.58 F1. Dense retrieval instantly retrieves turn 13 with 0.87 F1. Hybrid RRF ranks turn 13 at rank 1, producing a perfect 0.915 F1. This proves hybrid fusion is essential for conversational queries."

### Anticipated Q&A (~1 Minute)
* **Jury Question**: *"Why did you use Reciprocal Rank Fusion (RRF) instead of just adding the similarity scores together?"*
* **Krishna's Defense**: *"Because BM25 scores are unbounded and scale with document term frequencies, whereas cosine similarity scores are bounded between -1 and 1. Direct addition requires fragile score calibration. RRF is scale-invariant and relies strictly on relative ranks, making it robust across diverse query types without manual threshold tuning."*

---

## 5. Joint Synthesis & MVP Bridge (Slides 9–12) — 2 Minutes
* **Speaker**: Mekala Naveen Venkata Sai Kumar (with Lakhveer & Krishna)
* **Script**:
  > "On Slide 9, we synthesize our cross-task findings. Production conversational memory is governed by a strict Pareto frontier:
  > - Complete History is too expensive and suffers position decay.
  > - Sliding windows forfeit long-term memory.
  > - The optimal production architecture is a **hybrid memory pipeline**: **Hierarchical Summarisation** for ongoing conversational continuity and user profiling, combined with **Hybrid RRF Retrieval** for pinpoint episodic fact extraction.
  >
  > On Slide 10, these three tasks directly power our **Conversational Memory Evaluation Studio (MVP)**:
  > - Lakhveer's Task 1 powers the *Context Saturation Simulator*.
  > - My Task 2 powers the *Semantic Compression Inspector* and *Fact Retention Visualizer*.
  > - Krishna's Task 3 powers the *Interactive Memory Inspector* and *RAG Diagnostic Failure Suite*.
  >
  > We are on track for Hackathon 2 on December 17th and our final thesis submission on January 4th. Thank you, and we welcome any questions."
