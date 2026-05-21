# Modality Dropout for Multimodal Skin Lesion Classification

An empirical study evaluating whether Modality Dropout (randomly hiding patient metadata during training) makes skin cancer classifiers robust to missing metadata at inference.

## 🔍 Findings

* **HAM10000 was unsuitable:** The metadata was too weak (F1 = 0.242), and the baseline model already ignored it. There was no robustness problem to solve.
* **PAD-UFES-20 worked:** This dataset features rich metadata (22 fields) with 35% real missingness. The metadata-only F1 score (0.609) actually beat the image-only baseline (0.558).
* **Macro F1 Impact:** Modality Dropout never beats the baseline on macro F1. While higher dropout flattens the decay, it also lowers the peak performance—effectively canceling each other out.
* **Per-Class Impact:** Modality dropout *does* help on a per-class basis. It perfectly preserves melanoma recall under full metadata removal and stabilizes majority classes. However, performance on the minority class (SCC) collapses.

> **💡 Takeaway:** Evaluate performance per-class with clinical severity in mind, rather than relying solely on macro F1. Always verify dataset suitability by establishing image-only and metadata-only diagnostic baselines first.

## 🛠️ Method

* **Architecture:** ResNet-18 + MLP + concatenation fusion.
* **Training:** The metadata vector is zeroed out with probability *p* during training.
* **Testing:** Tested with *p* ∈ {0.00, 0.10, 0.15, 0.30, 0.50} across three different seeds. Models were evaluated at 0%, 25%, 50%, and 100% missing metadata.

## 📓 Notebooks

* https://www.kaggle.com/code/tanvirulhoque99/ham10000
* https://www.kaggle.com/code/tanvirulhoque99/pad-ufes-20-dataset
