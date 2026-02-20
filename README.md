# Acoustic Feature Analysis for Speech Signals

## Overview

Speech is a communication signal that simultaneously conveys linguistic content and paralinguistic information. Different acoustic features capture different aspects of this signal. This project investigates what kind of information is encoded in speech and which acoustic features capture task-specific information. In other words, which features are most suitable for which speech processing task.

Two datasets are analyzed:

* Emo-DB, for emotion prediction
* DAIS (Delft Database of EEG Recordings of Dutch Articulated and Imagined Speech), for vowel prediction

The goal is to evaluate how effectively different acoustic features separate these categories using unsupervised clustering.
Emotion is largely reflected in prosodic properties such as pitch and intensity. Vowel identity depends on spectral structure shaped by vocal tract configuration

By extracting multiple acoustic features and clustering them with K-means, this study examines how well different features capture these distinct types of information.

## Datasets

**Emo-DB** - German emotional speech dataset with seven emotion classes.

**DAIS** -  Delft Database of EEG Recordings of Dutch Articulated and Imagined Speech with five vowel cases: aa, ee, ie, oe, oo.

## Feature Extraction

Acoustic features were extracted using the Geneva Minimalistic Acoustic Parameter Set (GeMAPS). It is a compact, theory-driven feature set designed for affective speech analysis. Instead of thousands of brute-force features, it selects a small number of physiologically meaningful descriptors with strong empirical support.
The goal is to maintain interpretability while achieving competitive performance with a minimal parameter set. More information about GeMAPS can be found [here](https://sail.usc.edu/publications/files/eyben-preprinttaffc-2015.pdf)

From the available GeMAPS features, four were analyzed in this study: pitch (F0 semitone), jitter, loudness, and MFCC.
Pitch and loudness represent prosodic information, jitter captures voice quality, and MFCCs represent spectral envelope characteristics linked to vocal tract configuration. This allows us to directly compare how different acoustic layers contribute to emotion and vowel separation.

Frame-level features were aggregated to utterance-level representations using mean statistics.
All features were standardized prior to clustering.

## Methodology

For each dataset:

1. Extract acoustic features
2. Aggregate per utterance
3. Apply K-means clustering

   * 7 clusters for emotions
   * 5 clusters for vowels
4. Evaluate clustering using heatmaps and cluster purity

Cluster purity measures how well each cluster contains samples from a single ground truth class.

Formula:

    purity = (1 / N) * Σ_{m ∈ M} max_{d ∈ D} |m ∩ d|

Where:

- M: set of clusters  
- D: set of ground truth classes (emotions or vowels)  
- N: total number of data points  
- |m ∩ d|: number of samples from class d assigned to cluster m  

Higher purity indicates stronger alignment between clusters and true labels.

## Results

### Emotion Clustering

* Pitch achieved the highest purity (~0.33)
* MFCC showed moderate performance
* Loudness slightly lower
* Jitter performed worst

Emotional information is more strongly reflected in prosodic features, especially pitch. Overall purity remains limited due to overlap between emotional expressions.

### Vowel Clustering

* MFCC achieved the highest purity (~0.49)
* Pitch and loudness showed moderate separation
* Jitter was near random baseline

Combining all features increased purity (~0.63), confirming that vowel identity is primarily encoded in spectral envelope characteristics.

## Conclusion

The experiments show that acoustic feature effectiveness is task-dependent. Prosodic features such as pitch and loudness provide moderate structure for emotion-related clustering, while spectral features, particularly MFCCs, achieve stronger separation for vowel classification. In contrast, jitter showed limited discriminative power in both tasks.

Although K-means clustering reveals underlying acoustic structure, noticeable cluster overlap indicates that single features are insufficient for clean semantic separation.
