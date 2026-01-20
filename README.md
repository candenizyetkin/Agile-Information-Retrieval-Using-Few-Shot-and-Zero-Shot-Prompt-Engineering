# Agile Biomedical Information Retrieval in Rapidly Evolving Literature: A Comparative Evaluation of Dense Retrieval Models and Prompt-Based Large Language Models

This repository contains the implementation and experimental artifacts for the **CENG543 Term Project (2025–2026)** conducted by **Can Deniz Yetkin** at **İzmir Institute of Technology (IZTECH)**.

The project investigates whether **general-purpose Large Language Models (LLMs)**, guided solely by **prompt engineering**, can serve as an *agile and cost-effective alternative* to **dense retrieval models** for **biomedical question answering**, or whether **retrieval-based grounding remains essential**.



## 1. Research Motivation

Biomedical literature is expanding at an unprecedented pace, with thousands of new articles published daily. While **dense retrieval models** have demonstrated strong performance in biomedical QA systems, they require:

- Domain-specific training  
- Continuous retraining as literature evolves  
- Significant computational and maintenance cost  

In contrast, **Large Language Models (LLMs)** can operate in **zero-shot** and **few-shot** settings without any fine-tuning, enabling rapid deployment and flexibility.

This project explores whether such **prompt-based inference** can realistically replace or approximate retrieval-based systems in a **high-stakes biomedical domain**.



## 2. Core Research Question

**Can a general-purpose LLM, guided by careful zero-shot and few-shot prompt engineering, match the retrieval effectiveness and answer reliability of specialized dense retrievers (e.g., DPR, SapBERT) on the PubMedQA dataset?**



## 3. Task Definition

The task is **Biomedical Question Answering** on the **PubMedQA** dataset, framed as a controlled comparison between:

- **Dense Retrieval Models**
- **Prompt-Based LLM Inference (LLM-only, no retrieval)**

The study evaluates:

- Retrieval effectiveness  
- Answer correctness  
- Model behavior under uncertainty (the *maybe* label)



## 4. Dataset

- **Dataset:** PubMedQA  
- **Subset Used:** 1,000 question–abstract pairs  
- **Labels:** `yes`, `no`, `maybe`  

Each question is derived from a specific PubMed abstract, which serves as the **ground-truth relevant document** for retrieval experiments.



## 5. Experimental Structure

The project consists of **two complementary experimental pipelines**, each implemented as a separate notebook.



### 5.1 Dense Retrieval Evaluation

This experiment evaluates **retrieval-only performance**, independent of answer generation.

**Models Evaluated**
- **SapBERT** (UMLS synonym-aligned biomedical encoder)
- **PubMedBERT** (mean-pooled embeddings)
- **DPR Context Encoder** (trained on Natural Questions)

**Infrastructure**
- Embeddings indexed using **FAISS**
- Similarity metric: **L2 distance**

**Evaluation Metrics**
- **Recall@5**
- **Recall@10**

**Key Observation**  
SapBERT achieves **near-saturated retrieval performance**, highlighting the importance of retrieval-specific fine-tuning and explicit modeling of biomedical synonymy.



### 5.2 Prompt-Based LLM Evaluation

This experiment evaluates **LLM-only inference**, without access to retrieved documents.

**Models Evaluated**
- **Phi-3 Mini** (3.8B parameters)
- **MedAlpaca** (7B parameters)

**Prompting Strategies**
- **Zero-shot prompting**
- **Few-shot prompting** (limited in-context examples)

**Evaluation Focus**
- Binary answer accuracy (`yes` / `no`)
- Behavior on *maybe*-labeled questions
- Bias and uncertainty handling

**Key Observation**  
Few-shot prompting does **not** yield measurable improvement over zero-shot prompting. All configurations achieve approximately **0.55 accuracy**.



## 6. Uncertainty and Error Analysis

Handling uncertainty is critical in biomedical QA, as evidence is often incomplete or conflicting. This is explicitly captured in PubMedQA through the **`maybe`** label.

**Finding**
- All evaluated LLM configurations predict **“yes” for 100% of `maybe` questions**

This indicates:
- Strong affirmative bias  
- Inability to abstain or express uncertainty  
- A fundamental limitation of LLM-only inference under constrained output formats  



## 7. Key Findings

- Retrieval performance is driven primarily by **retrieval-oriented fine-tuning**, not model size alone  
- Prompt-based LLMs show **limited reliability** without evidence grounding  
- Few-shot prompting provides **no meaningful benefit** under strict binary constraints  
- Dense retrieval remains **essential** for reliable biomedical question answering  



## 8. Reproducibility Notes

- Implemented using **HuggingFace Transformers** and **FAISS**
- **No fine-tuning** performed on LLMs
- Random seeds fixed where applicable
- All experimental scripts are publicly available in this repository

---

## 9. Limitations and Future Work

### Limitations
- Evaluation limited to relatively small open-weight LLMs  
- Binary output format restricts uncertainty expression  
- Retrieval-Augmented Generation (RAG) not included in current experiments  

### Future Directions
- Evaluation with **larger-scale LLMs**
- Integration of **Retrieval-Augmented Generation (RAG)**
- Uncertainty-aware prompting strategies
- Faithfulness and evidence-grounding metrics
- Extension to open-ended biomedical QA tasks



## Author

**Can Deniz Yetkin**  
Department of Computer Engineering  
İzmir Institute of Technology (IZTECH)  
📧 canyetkin@std.iyte.edu
