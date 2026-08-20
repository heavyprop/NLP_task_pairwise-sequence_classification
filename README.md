# Natural Language Inference (NLI) Benchmark 

A comparative NLP study evaluating deep learning architectures on a binary classification task: determining whether a hypothesis is true given a premise.

---

## Dataset Overview

* **Task:** Binary Sequence Classification (Entailment determination)
* **Training Set:** 24,000+ premise/hypothesis pairs
* **Validation Set:** 6,000+ premise/hypothesis pairs
* **Data Constraints:** Closed setup (no external datasets used)

---

## Model Architectures

### Category B: Deep Learning without Transformers
* **Embeddings & Encoding:** Pre-trained GloVe embeddings (`glove-wiki-gigaword-300`) passed into a shared 256-unit BiLSTM encoder.
* **Alignment:** Cross-attention mechanism aligns premise and hypothesis token representations.
* **Feature Extraction:** Computes absolute difference (`|u - v|`) and element-wise product (`u ⊙ v`) between encoded and aligned states, passing the concatenated representations through a second BiLSTM.
* **Classification:** Concatenates global average and max pooling vectors before passing through a final dense classification layer.

### Category C: Deep Learning with Transformers
* **Architecture & Preprocessing:** Fine-tuned `roberta-base` with tokenized input sequences truncated to a maximum length of 128 tokens.
* **Optimization:** AdamW optimizer with Cosine Warmup (over 10% of training steps) and layer-wise discriminative learning rates:
  * **Base:** `1.5e-05`
  * **Head:** `5.0e-05`
* **Loss Function:** Focal Loss (γ = 2.0) paired with balanced class weights to heavily penalize misclassifications on difficult pairs.
* **Training Stability:** Automatic Mixed Precision (`bfloat16`) to reduce VRAM consumption, alongside gradient clipping (`max_norm = 1.0`) to ensure training stability and prevent exploding gradients.

---

## Error Analysis

### Category B
* **Lexical Overlap Bias:** Occasionally over-predicted entailment when premise and hypothesis shared specific vocabulary or named entities (e.g., *"Bel Air"*), ignoring semantic context differences.
* **Paraphrase Blindness:** Failed on entailment pairs where the hypothesis relied on heavy paraphrasing rather than direct lexical overlap.
* **Coreference Resolution Failures:** Struggled to resolve pronouns and implicit referents across sentence pairs (e.g., connecting *"my life"* to *"his autobiography"*).

### Category C
* **Antonym Blindness:** Over-predicted entailment on pairs sharing topically similar context but containing contradictory actions (e.g., classifying *"exile"* as entailing *"integration"*).
* **Syntactic Paraphrasing:** Struggled to resolve semantic equivalence when sentence structures were heavily inverted (e.g., *"A is far more appealing than B"* vs. *"B is not as captivating as A"*).
* **Length Degradation:** Misclassified pairs had a higher average word count (~30.7 words vs. ~28.8 words for correct samples), indicating context dilution over longer sequence lengths.

---

## Ethical Considerations & Limitations

* **Forced Binary Outcomes (Both):** Lacking a neutral or insufficient-information class forces the model to make definitive rulings on inherently ambiguous premise/hypothesis pairs.
* **Vocabulary Constraints (Category B):** Fixed vocabulary limit of 20,000 tokens maps out-of-vocabulary and domain-specific words to an `<OOV>` token.
* **Inherited Upstream Bias (Category C):** Pre-trained `roberta-base` carries societal, cultural, and linguistic biases present in its upstream training corpora.
* **Hardware Exclusivity (Category C):** CUDA-optimized mixed-precision training pipeline requires an NVIDIA GPU for replication.

---

## Results

| Model | Accuracy | Macro F1 | Matthew's Correlation Coefficient |
| :--- | :---: | :---: | :---: |
| **LSTM Baseline** | 66.06% | 66.03% | 0.3206 |
| **LSTM implementation** | 70.99% | 70.92% | 0.4188 |
| **BERT Baseline** | 82.02% | 81.98% | 0.6400 |
| **Transformer Implementation** | **88.82%** | **88.81%** | **0.7762** |
