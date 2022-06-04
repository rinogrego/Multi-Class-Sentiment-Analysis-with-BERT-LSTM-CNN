# Multi-Class-Sentiment-Analysis-with-BERT-LSTM-CNN

A class project to predict sentiment of reviewers using datasets of reviews of 3 e-commerce giants using Indonesian language.

## Data

The data consists of 3 different datasets representing each e-commerce giant (Tokopedia, Shopee, Lazada) and manually relabeled by the team. There are 6 possible labels for 1 text to possibly have with only 1 label for each text. The labels are:

- Happy / Senang
- Enthusiast / Antusias
- Curious / Penasaran
- Apathetic / Apatis
- Disappointed / Kecewa
- Angry / Kesal

### Tokopedia

The tokopedia dataset that are used have 4361 rows with sentiment distribution as shown in the picture:
![Tokopedia Distribution](/images/Tokopedia_Distribution.png)

### Shopee

The tokopedia dataset that are used have 4998 rows with sentiment distribution as shown in the picture:
![Shopee Distribution](/images/Shopee_Distribution.png)

### Lazada

The tokopedia dataset that are used have 5281 rows with sentiment distribution as shown in the picture:
![Lazada Distribution](/images/Lazada_Distribution.png)

*NOTE: All 3 datasets suffer imbalance problem but for this project we didn't deal with them*

## Pre-Processing

Some steps are applied towards the text data before being fed into the model:

1. Mapping colloquial words to their original meaning with [colloquial-indonesian-lexicon](https://raw.githubusercontent.com/nasalsabila/kamus-alay/master/colloquial-indonesian-lexicon.csv) dataset
2. Text cleaning process:
    - converte all the characters to lower case
    - remove URLs
    - remove username tagging
    - remove numbers and symbols
    - remove emoticons
    - remove words that consists of just 1 character
    - remove repeated characters
3. Split ratio of training and validation data is 8:2

## Model and Hyperparameter Tuning

For our BERT model we are using [IndoBERT](https://huggingface.co/indobenchmark/indobert-base-p2), a pre-trained BERT model for Indonesian language trained on 23.43 GB text data. As for the Hybrid Deep Learning model, we are using a combination of LSTM-CNN-MaxPool layers and then finalized with 6 Dense units for 6 outputs representing each sentiments.

For hyperparameter tuning, we are using Bayesian Optimization with the following choices of hyperparameter.

| Layer | Hyperparameter | Values |
| --- | --- | --- |
| | Units | 100; 150; 200 |
| LSTM | L2 Kernel | 0.001; 0.01 |
| | L2 Recurrent | 0.001; 0.01 |
| | Filters | 200; 250; 300 |
| CNN | Region Size | 3; 4; 5 |
| | L2 Cnn | 0.001; 0.01 |
| Fully Connected Layers | L2 Dense | 0.001; 0.01 |

With additional hyperparameter and configuration:

| Configuration | Choice/Value |
| --- | --- |
| Optimizer | Adam |
| Loss | Categorical Crossentropy |
| Batch Size | 32 |
| Epochs | 50 |

## Results

After 5 different trials, the results are as follows:

| Metrics | Tokopedia | Shopee | Lazada |
| --- | --- | --- | --- |
| Accuracy | 0.618 ± 0.0044 | 0.732 ± 0.0045 | 0.662 ± 0.0044 |
| Precision (Weighted Avg) | 0.606 ± 0.0114 | 0.722 ± 0.0083 | 0.666 ± 0.0054 |
| Recall (Weighted Avg) | 0.618 ± 0.0044 | 0.732 ± 0.0045 | 0.662 ± 0.0044 |
| F1-Score (Weighted Avg) | 0.588 ± 0.0083 | 0.722 ± 0.0045 | 0.65 ± 0.0071 |
