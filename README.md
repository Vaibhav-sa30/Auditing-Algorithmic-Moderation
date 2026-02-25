# Auditing Algorithmic Moderation: A Multi-Signal Analysis of Automated Content Governance

[![Python](https://img.shields.io/badge/Python-3.9%2B-blue?logo=python)](https://www.python.org/)
[![Kaggle](https://img.shields.io/badge/Run%20on-Kaggle-20BEFF?logo=kaggle)](https://kaggle.com)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

> **Vaibhav Satish · Indian Institute of Technology Madras**  
> Research paper investigating the Transparency Gap in automated content moderation through multi-signal auditing.

---

## Abstract

This study examines how different signal classes — textual, social feedback, identity-related, and internal platform signals — jointly shape automated moderation outcomes. Using an auditing-oriented methodology, we empirically demonstrate the existence of a **Transparency Gap**: a significant portion of moderation decisions is attributable to internal platform signals that are opaque to users, researchers, and regulators. We further identify identity-linked categorization disparities and temporal escalation dynamics, positioning automated moderation as a dynamic, multi-signal governance pipeline rather than a static classification task.

**Key findings:**
- Internal platform signals alone achieve Macro-F1 of **0.703**, nearly 3× random baseline
- All-signal model achieves Macro-F1 of **0.817** (+0.046 over text-only)
- SHAP analysis: internal signals account for **21.4%** of decision influence despite being fully opaque
- Comments with identity references show **2.2× higher high-severity escalation rate**
- Toxic category prevalence nearly **doubles** from temporal Q1 → Q4

---

## Research Questions

| RQ | Question | Section |
|----|----------|---------|
| **RQ1** | How do different signal classes contribute to moderation outcomes, and what is the magnitude of the Transparency Gap? | §5.2, §6.1 |
| **RQ2** | Are identity-linked signals associated with systematic differences in categorization patterns? | §5.4, §6.3 |
| **RQ3** | How do temporal dynamics influence escalation through the moderation pipeline? | §5.5, §6.4 |

---

## Repository Structure

```
.
├── paper_companion.ipynb        # v1 — Restricted library analysis (sklearn only)
├── paper_companion_v2.ipynb     # v2 — Full analysis (NLTK, LightGBM, DistilBERT)
├── complete-nb.ipynb            # Comprehensive NLP preprocessing notebook
├── Algorithmic Moderation (with figures).docx   # Paper with embedded figures
├── Algorithmic Moderation Signal Classes and Governance Pipeline - Table 1.csv
├── paper_figures/               # Generated figures (MSMP pipeline, SHAP charts, etc.)
│   ├── fig1_msmp_pipeline.png
│   ├── fig2_transparency_gap.png
│   └── fig3_identity_temporal.png
└── README.md
```

---

## Multi-Signal Moderation Pipeline (MSMP)

The paper introduces and operationalizes the **MSMP framework**, which categorises input signals into four classes:

| Signal Class | Visibility | Key Features | Transparency |
|---|---|---|---|
| **Textual** | User-visible | TF-IDF, embeddings | Visible & contestable |
| **Social Feedback** | User-visible | Upvotes, downvotes, reactions | Visible |
| **Identity-Related** | User-visible | Race, gender, religion, disability refs | Observable but weighting opaque |
| **Internal Platform** | Platform-only | Risk scores, historical heuristics | **Fully opaque ⚠** |

---

## Methodology

- **Modeling**: Supervised multi-class classification (proxy-based auditing approach)
- **Models**: LightGBM, LinearSVC, Logistic Regression, DistilBERT, XGBoost, and ensemble methods
- **Explainability**: SHAP (SHapley Additive exPlanations) aggregated by signal class
- **Fairness**: Group-level disparate impact analysis on identity-linked signals
- **Temporal**: Category severity distributions across temporal submission quartiles

---

## Key Results

### Signal Combination Experiments (RQ1)

| Configuration | Macro-F1 |
|---|---|
| Text only | 0.771 |
| Text + Identity | 0.783 |
| Social only | 0.621 |
| **Internal only** | **0.703** |
| **All signals** | **0.817** |

### SHAP Decision Influence by Signal Class

| Signal Class | % of Decision Influence |
|---|---|
| Textual | 48.3% |
| Social Feedback | 19.1% |
| Identity-Related | 11.2% |
| Internal Platform ⚠ | **21.4%** |

---

## Requirements

```bash
# Core
pip install numpy pandas scikit-learn lightgbm xgboost

# NLP (v2 notebook)
pip install nltk transformers torch

# Explainability
pip install shap

# Document processing
pip install python-docx pdfplumber

# Visualization
pip install matplotlib seaborn
```

---

## Running the Notebooks

### Recommended: Kaggle (GPU T4 x2)
1. Upload `paper_companion_v2.ipynb` to Kaggle
2. Set accelerator to **GPU T4 x2**
3. Enable internet access
4. Run all cells — checkpointing is built in (`/kaggle/working/ckpt_v2/`)

### Local (16GB RAM, RTX 3050 or better)
```bash
jupyter notebook paper_companion_v2.ipynb
```

> **Note:** The DistilBERT fine-tuning cell uses mixed-precision (fp16) and is optimised for 4GB VRAM. On CPU, skip Section 9 (DistilBERT fine-tuning) and use Section 8 (embedding-only) instead.

---

## Figures

| Figure | Description |
|---|---|
| ![Fig 1](paper_figures/fig1_msmp_pipeline.png) | **Figure 1:** Multi-Signal Moderation Pipeline |
| ![Fig 2](paper_figures/fig2_transparency_gap.png) | **Figure 2:** Signal Combination Results & Transparency Gap |
| ![Fig 3](paper_figures/fig3_identity_temporal.png) | **Figure 3:** Identity Patterns & Temporal Escalation |

---

## Citation

If you use this work, please cite:

```bibtex
@article{auditing_algorithmic_moderation_2025,
  title   = {Auditing Algorithmic Moderation: A Multi-Signal Analysis of Automated Content Governance},
  author  = {[Author Names]},
  year    = {2025},
  school  = {Indian Institute of Technology Madras},
  note    = {Machine Learning Practice Project}
}
```

---

## License

MIT License — see [LICENSE](LICENSE) for details.
