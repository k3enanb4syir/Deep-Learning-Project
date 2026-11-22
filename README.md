# BioASQ 12b: Biomedical Yes/No Question Answering

## Project Overview

This project focuses on developing a deep learning model for biomedical Yes/No Question Answering using the BioASQ 12b dataset.

The objective is to build a model that can understand biomedical questions alongside their supporting evidence and classify the answer into two categories: **YES** or **NO**.

The proposed approach uses a Transformer-based cross-encoder architecture, with biomedical language models such as PubMedBERT or BioLinkBERT.

## Project Objectives

- Acquire and prepare biomedical question-answering data.
- Extract supporting evidence snippets for each question.
- Develop a binary YES/NO classification dataset.
- Implement baseline machine learning models.
- Develop a Transformer-based question-answering model.
- Evaluate model performance using Accuracy, F1-Yes, F1-No, and Macro-F1.

## Dataset Acquisition

### Dataset Source

The project uses a publicly accessible, third-party processed version of BioASQ 12b obtained from Hugging Face.

- **Dataset:** [BioASQ 12B RAG](https://huggingface.co/datasets/mattmorgis/bioasq-12b-rag)
- **Configuration:** `question-answer-passages`
- **License declared by the dataset publisher:** CC BY-NC-SA 4.0

The alternative source was selected because access to the official BioASQ participant download platform was unavailable due to account activation restrictions.

### Dataset Description

The dataset contains biomedical questions, natural-language answers, relevant passage identifiers, and supporting evidence snippets.

Only records with the question type `yesno` are selected for this project.

### Dataset Statistics

| Description | Number of Records |
|---|---:|
| Original development records | 5,049 |
| Original evaluation records | 340 |
| Development YES/NO questions | 1,357 |
| Evaluation YES/NO questions | 102 |
| Successfully processed development records | 914 |
| Successfully processed evaluation records | 59 |
| Rejected development records | 443 |
| Rejected evaluation records | 43 |

### Processed Dataset Splits

The processed development dataset is divided into training, validation, and internal testing subsets using an 80:10:10 ratio.

| Dataset Split | Records |
|---|---:|
| Training | 731 |
| Validation | 91 |
| Internal Testing | 92 |
| Separate Source Evaluation | 59 |

A random seed of 42 is used to maintain reproducibility. Stratified sampling is applied to preserve the YES/NO class distribution across the development splits.

The separate source evaluation split is kept apart from model training and validation.

## Data Preprocessing

The preprocessing pipeline consists of the following steps:

1. Download the BioASQ 12b dataset from Hugging Face.
2. Inspect the dataset structure and question types.
3. Filter questions belonging to the `yesno` category.
4. Extract biomedical questions and their corresponding answers.
5. Extract supporting evidence from the available snippets.
6. Convert answers into binary classification labels.
7. Remove invalid or incomplete records.
8. Create training, validation, and internal testing splits.
9. Export the processed datasets and dataset statistics.

### Label Mapping

| Label | Numerical Representation |
|---|---:|
| YES | 1 |
| NO | 0 |

Candidate labels are extracted from the beginning of the natural-language answer field. Records without an identifiable YES/NO prefix are excluded from the processed dataset and documented separately.

### Evidence Preparation

Supporting evidence is extracted from the dataset's snippets field.

Different evidence variations are prepared to investigate the effect of evidence quantity on model performance:

- First 1 evidence snippet
- First 3 evidence snippets
- First 5 evidence snippets
- All available evidence snippets

The first-N variations follow the original stored snippet order. No additional relevance ranking is assumed.

## Data Quality Assurance

Data quality assurance is an important part of the acquisition process because the alternative dataset contains natural-language ideal answers rather than the original structured BioASQ exact-answer field.

The following validation procedures are included:

- Checking for missing questions and supporting evidence.
- Identifying duplicate question IDs.
- Checking whether YES/NO labels can be extracted from answer prefixes.
- Reviewing the distribution of YES and NO labels.
- Separating invalid or incomplete records into rejection logs.
- Creating a 50-record manual label audit sample.

### Data Quality Limitations

Out of 1,357 development YES/NO questions, 914 were successfully processed, while 443 were rejected because they did not meet the current extraction requirements.

For the separate evaluation split, 59 out of 102 questions were successfully processed, while 43 were rejected.

The extracted labels are candidate labels and require manual verification against the original answers and supporting evidence before final model training.

The current dataset also contains class imbalance, with YES examples occurring more frequently than NO examples.

These limitations will be considered during subsequent data preparation and model development.

## Repository Structure

```text
Deep-Learning-Project/
│
├── data/
│   ├── raw/
│   │   └── README.md
│   │
│   └── processed/
│       ├── README.md
│       ├── dataset_manifest.json
│       └── dataset_summary.csv
│
├── notebooks/
│   └── 01_data_acquisition.ipynb
│
├── src/
│   └── preprocess.py
│
├── requirements.txt
├── .gitignore
└── README.md
```

Raw and processed dataset files are maintained separately as local backups and are not included in the public repository. Dataset source information, processing statistics, and metadata are provided through the manifest and summary files.

## Tools and Technologies

- Python
- Google Colab
- Hugging Face Datasets
- Pandas
- NumPy
- Scikit-learn
- GitHub

## Model Evaluation

The planned evaluation metrics are:

- Accuracy
- F1-Yes
- F1-No
- Macro-F1

These metrics will be used to compare baseline machine learning approaches with the proposed Transformer-based cross-encoder.

## Project Status

**Completed:**
- Initial dataset acquisition
- Question type filtering
- Binary label extraction
- Supporting evidence extraction
- Initial preprocessing
- Dataset splitting
- Dataset statistics and documentation

**Remaining:**
- Manual label quality review
- Final dataset validation
- Baseline model implementation
- Transformer model development
- Model evaluation and error analysis

## Acknowledgements and Data Attribution

The dataset is based on BioASQ 12b and was obtained through the third-party Hugging Face repository:

https://huggingface.co/datasets/mattmorgis/bioasq-12b-rag

The dataset publisher declares the CC BY-NC-SA 4.0 license. Users should follow the applicable license and original BioASQ distribution terms when using or sharing the data.

This repository does not redistribute the original raw or processed dataset files.
