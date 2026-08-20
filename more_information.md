# Natural Language Inference

**Team Members:** 
- Daniel Koshovyy
- Aran Cambell

---

## Code Structure

Submission contains two distinct solutions addressing the Natural Language Inference (NLI) shared task. For each problem, we have separated the training process from the final inference step:

### Transformer-based Approach 
1. **`transformer_training.ipynb`**
   - This notebook contains the entire pipeline for our Category C solution. 
   - It includes the data loading, preprocessing, custom Focal Loss implementation, layer-wise learning rate configuration, mixed-precision training loop (AMP), and post-training threshold sweeping logic.
   - It also uses torch + cuda to perform training using an nVIDIA GPU.
2. **`transformer_demo.ipynb` (Demo Code)**
   - This is the lightweight inference script designed for the marking team. 
   - It skips the training process entirely. It initializes the `roberta-base` architecture, downloads the necessary libraries, loads our pre-trained weights (`Group_57_C_best.pt`), and runs inference on the hidden test set to produce the final predictions CSV.
3. **`transformer_model_card.md`**
   - The model card detailing the specifications, hyperparameters, and limitations of our fine-tuned RoBERTa model.

### Deep Learning without Transformer LSTM (ESIM)
1. **`lstm_esim_training.ipynb`**
   - Contains the entire pipeline for the category B solution.
   - It includes data loading, preprocessing, 
2. **`lstm_esim_demo.ipynb` (Demo Code)**
   - This is a demo inference scirpt designed for displaying how the model works. 
   - It skips the training process entirely. It initialises the keras model, downloads necessary libraries and outputs a result file as a .csv.
3. **`lstm_esim_model_card.md`**
   - The model card detailing the specifications, hyperparameters, and limitations of our Bi-Directional LSTM.

---

## Model Weights (External Links)

* **Category C Weights (`transformer_best.pt`, ~490MB):** [Download Link](https://livemanchesterac-my.sharepoint.com/:u:/r/personal/aran_campbell_student_manchester_ac_uk/Documents/Group_57_C_best.pt?csf=1&web=1&e=FELRGE)
* **Category B Weights:** (`lstm_esim_model.keras + More` https://livemanchesterac-my.sharepoint.com/:u:/g/personal/daniel_koshovyy_student_manchester_ac_uk/IQB312nc2yUHRJI1_kFcGy_PAQRcKkGYp26y9QnkkzyD1Pw?e=yXJeu3

---

## Attributions and References

We acknowledge the use of the following open-source resources, codebases, and libraries in the development of our solutions:

**Transformer Attributions:**
* **Hugging Face Transformers:** We utilized the `transformers` library for the tokenizer and the `roberta-base` model architecture. (Wolf et al., 2020)
* **RoBERTa:** The foundational pre-trained model used for fine-tuning. (Liu et al., 2019)
* **PyTorch:** Used as the core deep learning framework for tensor operations, automatic mixed precision (AMP), and our custom Focal Loss implementation.
* **Scikit-Learn:** Used for calculating the `compute_class_weight` and evaluating the Macro F1-score during threshold sweeping.

**LSTM Attributions:**
- GloVe embeddings: Jeffrey Pennington, Richard Socher, Christopher Manning. GloVe: Global Vectors for Word Representation. https://nlp.stanford.edu/pubs/glove.pdf
- ESIM architecture which inspired my pipeline: Chen et al. (2017) "Enhanced LSTM for Natural Language Inference". https://arxiv.org/abs/1609.06038. I used principles from the E-Sim architecture to improve the performance of my model.
