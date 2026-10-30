# Replicating and Extending Contextual-Top2Vec for Multi-Aspect Topic Discovery in Consumer Reviews

This repository contains the code and notebooks used for the COMP8240 Applications of Data Science final project at Macquarie University.

The project partially replicates Contextual-Top2Vec (C-Top2Vec), introduced by Angelov and Inkpen (2024), and extends the method to multi-aspect TripAdvisor hotel reviews.

## Project Overview

The project contains two main experimental stages:

1. A reduced replication of C-Top2Vec using the 20 Newsgroups dataset.
2. An extension to annotated TripAdvisor hotel reviews using the Trip-MA dataset.

The TripAdvisor extension includes:

- modelling of 369 natural multi-aspect hotel reviews;
- construction of 100 controlled documents containing known aspect transitions;
- quantitative evaluation of predicted topic boundaries;
- hierarchical reduction of 188 discovered topics to 11 broader topics; and
- a supplementary LLM-as-Judge evaluation of reduced topic alignment.

## Main Results

### 20 Newsgroups Replication

A stratified sample of 200 documents was used because larger contextual runs exceeded the available Apple MPS memory.

The reduced experiment produced:

- 163 discovered topics;
- ARI = 0.0567;
- AMI = 0.1329.

The original paper reports ARI = 0.442 ± 0.008 and AMI = 0.582 ± 0.008 on the full 20 Newsgroups dataset.

The local experiment is therefore treated as a partial replication and feasibility assessment rather than an exact reproduction.

### Natural TripAdvisor Reviews

C-Top2Vec was applied to 369 natural TripAdvisor reviews.

The model discovered 188 topics. Many topics were organised around hotel brands, destinations, resorts, amenities, and other contextual information rather than showing a direct one-to-one relationship with the Trip-MA aspect categories.

### Controlled Boundary Evaluation

A controlled dataset of 100 documents was constructed using three single-aspect segments from three different source reviews.

Each document contained:

- three distinct aspects;
- two known aspect boundaries.

This produced 200 ground-truth boundaries.

At a tolerance of ±5 whitespace tokens:

- matched boundaries = 36;
- precision = 0.350;
- recall = 0.180;
- F1 = 0.238.

Seventy-two of the 100 documents contained no predicted topic transition.

### Hierarchical Topic Reduction

The 188 natural-review topics were reduced to 11 broader topics.

The target of 11 was selected using knowledge of the Trip-MA annotation scheme and is therefore treated as an ontology-informed diagnostic rather than an unsupervised estimate of the ideal number of topics.

### LLM-as-Judge

The 11 reduced topics were evaluated using a fixed topic-to-aspect alignment rubric.

The evaluation produced:

- mean alignment score = 1.18 / 3;
- 9 weak or mixed topic alignments;
- 2 moderate alignments;
- 0 strong alignments.

The LLM-as-Judge results are treated as supplementary semantic evidence rather than ground truth.

## Notebooks

### `01_feasibility_20newsgroups.ipynb`

Contains:

- 20 Newsgroups data loading;
- stratified sampling;
- reduced C-Top2Vec configuration;
- model training;
- ARI and AMI evaluation;
- qualitative topic inspection.

### `02_tripadvisor_data.ipynb`

Contains:

- Trip-MA data preparation;
- review and segment reconstruction;
- natural TripAdvisor C-Top2Vec experiment;
- controlled multi-aspect dataset construction;
- token alignment;
- boundary evaluation;
- hierarchical topic reduction;
- LLM-as-Judge evaluation;
- final experimental summary.

## Data

The 20 Newsgroups dataset can be obtained directly through scikit-learn.

The Trip-MA data originates from the TripAdvisor annotated dataset described by Marcheggiani et al. (2014) and Jiménez Zafra et al. (2015).

Raw TripAdvisor data are not redistributed in this repository. The notebooks document the preprocessing and construction procedure used for the experiments.

Processed outputs that do not require redistribution of the complete raw dataset may be included where appropriate.

## Reproducibility

Experiments were performed using Python 3.11.15.

Important recorded package versions include:

- NumPy 1.26.4
- PyTorch 2.2.2
- Transformers 4.41.2

The contextual embedding model was:

`sentence-transformers/all-mpnet-base-v2`

A fixed random seed of 42 was used for the controlled multi-aspect dataset construction.

Larger contextual experiments encountered Apple MPS memory limitations. CPU computation was therefore used for the final TripAdvisor experiments.

## Saved Models

Saved Top2Vec model directories are not included in this repository because of their size.

The notebooks contain the configurations and commands required to recreate the models.

## References

Angelov, D., & Inkpen, D. (2024). Topic Modeling: Contextual Token Embeddings Are All You Need. Findings of EMNLP 2024.

Marcheggiani, D., Tackstrom, O., Esuli, A., & Sebastiani, F. (2014). Hierarchical Multi-Label Conditional Random Fields for Aspect-Oriented Opinion Mining.

Jiménez Zafra, S. M., et al. (2015). A Multi-lingual Annotated Dataset for Aspect-Oriented Opinion Mining.