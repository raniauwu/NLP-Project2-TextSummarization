# 📰 Liputan6 Extractive Summarization with IndoBERT
**Indonesia AI Career Bootcamp — NLP Batch | Project 2: Text Summarization**

---

## 📌 Project Overview

This project builds an automatic **extractive text summarization** system using the **Liputan6** Indonesian news dataset and **IndoBERT**. The goal is to generate concise, accurate, and informative summaries from long articles — helping readers quickly grasp the key points of complex documents.

The approach used is **sentence-level binary classification**: each sentence in an article is classified as important (label 1) or not important (label 0), and the selected sentences are assembled into a final summary.

---

## 🗂️ Dataset

| Split | Records |
|-------|---------|
| Train (after downsampling) | 46,131 |
| Dev | 15,920 |
| Test | 14,834 |
| **Total** | **76,885** |

- **Source:** Liputan6 Indonesian news corpus (`liputan6_data.tar.gz`)
- **Original train size:** 193,883 → downsampled to 60% via **stratified sampling by URL segment**
- **Fields used:** `clean_article`, `clean_summary`, `extractive_summary`, `url`

---

## 🔍 Exploratory Data Analysis

| Finding | Detail |
|---------|--------|
| Article length distribution | Right-skewed; mean ~222–234 words, max up to 3,515 words |
| Summary length | Consistently short; mean ~25–31 words |
| Compression ratio | ~12–16% of article length → strong compression required |
| Lexical overlap | High → supports **extractive** approach |
| Extractive summary | Typically only ~2 sentences per article |

> 💡 **EDA Conclusion:** Despite the low compression ratio, high lexical overlap between articles and summaries indicates that an **extractive approach** is more appropriate for this dataset.

---

## 🧠 Model

**IndoBERT** (`indobenchmark/indobert-base-p1`) — a BERT model pre-trained specifically on Indonesian text, used as the backbone for sentence-level classification.

---

## ⚙️ Pipeline

```
Data Loading (.tar.gz)
        ↓
Stratified Downsampling (train → 60%)
        ↓
EDA (text length, compression ratio, lexical overlap)
        ↓
Preprocessing & Sentence Splitting
        ↓
IndoBERT Tokenization (max_len = 128)
        ↓
Binary Classification per Sentence
        ↓
Training with Weighted Loss (class imbalance handling)
        ↓
Hyperparameter Tuning (3 configurations)
        ↓
ROUGE Evaluation (dev & test)
```

---

## 🧪 Experiments & Results

### Configurations Tested

| Run | Learning Rate | Batch Size | Epochs |
|-----|--------------|------------|--------|
| Baseline | 2e-5 | 64 | 3 |
| Tuning 1 | 2e-5 | 16 | 3 |
| **Tuning 2 ✅ (Best)** | **3e-5** | **16** | **3** |
| Tuning 3 | 2e-5 | 8 | 4 |

### ROUGE Scores on Test Set

| Model | ROUGE-1 | ROUGE-2 | ROUGE-L | Avg ROUGE |
|-------|---------|---------|---------|-----------|
| IndoBERT Baseline | 0.3016 | 0.1640 | 0.2580 | 0.2412 |
| **IndoBERT Tuned (Best)** | **0.3369** | **0.1931** | **0.2863** | **0.2721** |

> ✅ Tuning with `learning_rate = 3e-5` and `batch_size = 16` consistently improved all ROUGE metrics compared to the baseline.

---

## 📋 Qualitative Evaluation

| Sample | Quality | Notes |
|--------|---------|-------|
| Sample 12 | ✅ Good | Key topic (Hamzah Haz, US terrorism) captured accurately |
| Sample 1 | ⚠️ Fair | On-topic (Tommy Soeharto, weapons) but lacks critical details |
| Sample 6 | ❌ Poor | Boilerplate text ("liputan6. liputan6.") leaked into the summary |

---

## 🚀 Future Works

1. Experiment with **Indonesian RoBERTa** as an architecture comparison
2. Train on the **full dataset** without downsampling (if compute resources allow)
3. **Improve preprocessing** — stricter boilerplate filtering, sentence deduplication
4. Explore more adaptive sentence selection strategies (probability thresholding, re-ranking)
5. Investigate **hierarchical encoders** for handling very long articles

---

## 🛠️ Tools & Libraries

- Python, Pandas, NumPy, Matplotlib
- HuggingFace Transformers & Trainer API
- PyTorch
- NLTK
- ROUGE Score
- Google Colab

---

## 📁 Repository Structure

```
📦 NLP-Project2-TextSummarization
 ┣ 📓 Liputan6_Extractive_Summarization.ipynb    # Main notebook
 ┗ 📄 README.md                                   # Project documentation
```

---

## 👩‍💻 Contributor

**Surya Dharma Putra**
**Agil Setiawan**
**Krisna Fery Rahmantya**
**Khaerani Arista Dewi**

