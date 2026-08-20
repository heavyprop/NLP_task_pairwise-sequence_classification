# Model Card for Transformer Implementation

<!-- Provide a quick summary of what the model is/does. -->

This is a Natural Language Inference (NLI) transformer model trained to classify the 
      relationship between a premise and a hypothesis.


## Model Details

### Model Description

<!-- Provide a longer summary of what this model is. -->

This model is based on the RoBERTa-base architecture, fine-tuned 
      using differential learning rates for the base and head with an AdamW optimiser and a Focal Loss function 
      to better handle difficult training pairs.

- **Language(s):** English
- **Model type:** Supervised
- **Model architecture:** RoBERTa
- **Finetuned from model [optional]:** roberta-base

### Model Resources

<!-- Provide links where applicable. -->

- **Repository:** https://huggingface.co/FacebookAI/roberta-base
- **Paper or documentation:** https://arxiv.org/abs/1907.11692

## Training Details

### Training Data

<!-- This is a short stub of information on the training data that was used, and documentation related to data pre-processing or additional filtering (if applicable). -->

Dataset consisting of 24K premise-hypothesis pairs.

### Training Procedure

<!-- This relates heavily to the Technical Specifications. Content here should link to that section when it is relevant to the training procedure. -->

#### Training Hyperparameters

<!-- This is a summary of the values of hyperparameters used in training the model. -->


      - learning_rate_base: 1.5e-05
      - learning_rate_head: 5e-05
      - train_batch_size: 64
      - max_length: 128
      - seed: 7
      - num_epochs: 10
      - optimizer: AdamW with Cosine Warmup (10% steps)
      - loss_function: Focal Loss (gamma=2.0) with balanced class weights

#### Speeds, Sizes, Times

<!-- This section provides information about how roughly how long it takes to train the model and the size of the resulting model. -->


      - overall training time: 8 minutes
      - duration per training epoch: 40-50 seconds
      - model size: ~499MB

## Evaluation

<!-- This section describes the evaluation protocols and provides the results. -->

### Testing Data & Metrics

#### Testing Data

<!-- This should describe any evaluation data used (e.g., the development/validation set provided). -->

The provided development set (dev.csv).

#### Metrics

<!-- These are the evaluation metrics being used. -->


      - Macro F1-score
      - Accuracy

### Results

The model obtained a best Val F1-score of 88.76% and a Val Accuracy of 88.78%.

## Technical Specifications

### Hardware


      - RAM: 32GB
      - Storage: at least 2GB
      - GPU: NVIDIA RTX 5080

### Software


      - Transformers 5.4.0
      - Pytorch 2.8.0+cu128
      - Accelerate 1.13.0
      - Tokenizers 0.22.2

## Bias, Risks, and Limitations

<!-- This section is meant to convey both technical and sociotechnical limitations. -->

Inputs longer than 128 tokens are truncated. 
      The model performance is dependent on the quality of the NLI pair annotations 
      in the training set. A powerful GPU was used to train this model, training times may vary significantly on weaaker hardware.

## Additional Information

<!-- Any other information that would be useful for other people to know. -->

Training utilized Mixed Precision (bfloat16) and 
      gradient clipping (max_norm=1.0) to stabilize the fine-tuning process.
