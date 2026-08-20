# Model Card for LSTM (ESIM)

<!-- Provide a quick summary of what the model is/does. -->

Given a premise and a hypothesis, determine if the hypothesis is true based on the premise.


## Model Details

### Model Description

<!-- Provide a longer summary of what this model is. -->

A bidirectional LSTM, inspired by an Enhanced Sequential Inference Model. 
    Uses GloVe for embeddings, a shared BiLSTM encodes both sequences (premise and hypothesis). Attention aligns premise to 
    hypothesis and the other way round. A second BiLSTM encodes the enhanced representations. 
    Average and max pooling concatenated for each side. Absolute difference & elementwise product 
    are computed again between final premise and hypothesis vectors. Then classified with 2 dense layers.

- **Language(s):** English
- **Model type:** Supervised
- **Model architecture:** Bidirectional LSTM

### Model Resources

<!-- Provide links where applicable. -->

- **Repository:** https://keras.io/api/layers/recurrent_layers/bidirectional/
- **Paper or documentation:** https://arxiv.org/abs/1609.06038

## Training Details

### Training Data

<!-- This is a short stub of information on the training data that was used, and documentation related to data pre-processing or additional filtering (if applicable). -->

more than 24K premise-hypothesis pairs as training data.

### Training Procedure

<!-- This relates heavily to the Technical Specifications. Content here should link to that section when it is relevant to the training procedure. -->

#### Training Hyperparameters

<!-- This is a summary of the values of hyperparameters used in training the model. -->
      - embedding_dim: 300
      - lstm_units: 256
      - dropout_rate: 0.3
      - learning_rate: 0.001
      - batch_size: 32
      - seed: 42
      - num_epochs: 15

#### Speeds, Sizes, Times

<!-- This section provides information about how roughly how long it takes to train the model and the size of the resulting model. -->
      - overall training time: 30 mins
      - duration per training epoch: 3.75 minutes
      - model size: 100.9MB

## Evaluation

<!-- This section describes the evaluation protocols and provides the results. -->

### Testing Data & Metrics

#### Testing Data

<!-- This should describe any evaluation data used (e.g., the development/validation set provided). -->

A subset of the development set provided, amounting to 2K pairs.

#### Metrics

<!-- These are the evaluation metrics being used. -->
      - accuracy_score: 0.70991686460808
      - macro_precision: 0.70977271278066
      - macro_recall: 0.70903435467912
      - macro_f1: 0.70918463617279
      - weighted_macro_precision: 0.70984225971494
      - weighted_macro_recall: 0.70991686460808
      - weighted_macro_f1: 0.70966123456262
      - matthews_corrcoef: 0.41880641659551

## Technical Specifications

### Hardware
      - RAM: at least 16 GB
      - Storage: at least 2GB,
      - GPU: V100

### Software
      - keras

## Bias, Risks, and Limitations

<!-- This section is meant to convey both technical and sociotechnical limitations. -->

Premise or hypothesis sequences longer than the 95th percentile of training sequence 
lengths will be truncated and the model was also trained on a fixed vocabulary of 20,000 words, 
so out-of-vocabulary words will be mapped to an unknown token, consequently may reduce performance.

## Additional Information

<!-- Any other information that would be useful for other people to know. -->

The hyperparameters were determined by experimentation
      with different values.
