# Decision Making Under Uncertainty and Risk

A comparison of Bayesian Networks and Fuzzy Logic for flood emergency 
decision-support, trained on a real-world dataset of 50,000 entries. 
Bachelor thesis, VU Amsterdam (Grade: 9.0).

## Overview

Emergency response decisions (e.g., evacuation calls) often need to be made 
under high uncertainty with incomplete information. This project builds two 
independent decision-support systems — a discrete Bayesian Network and a 
Fuzzy Logic system — that each take environmental flood-risk indicators 
(monsoon intensity, drainage systems, deforestation, siltation, etc.) and 
recommend one of three actions: "Evacuate!", "Warning", or "No Issue".

Since no ground-truth labels exist for "correct" flood decisions, this 
project doesn't benchmark classification accuracy. Instead, it compares 
**how differently the two models behave** when reasoning under the same 
uncertainty and risk.

## Key Findings

| Action | Bayesian Network (test set, n=10,000) | Fuzzy Logic (full set, n=50,000) |
|---|---|---|
| Evacuate! | 5,242 (52.4%) | 1,613 (3.2%) |
| Warning | 4,583 (45.8%) | 47,452 (94.9%) |
| No Issue | 175 (1.8%) | 935 (1.9%) |

- The **Bayesian Network is risk-averse**, recommending full evacuation 
  over half the time — prioritizing safety over precision, at the cost of 
  a high false-alarm rate
- **Fuzzy Logic is more measured**, defaulting to "Warning" in the vast 
  majority of cases rather than jumping straight to evacuation
- Both models rarely recommend "No Issue" under uncertainty, but reach 
  that caution through very different reasoning: probabilistic inference 
  + expected utility vs. rule-based fuzzy membership + thresholding

## Method

- **Bayesian Network** (via PyAgrum): 7 environmental features discretized 
  into Low/Medium/High via `pd.qcut()`, with conditional probability 
  distributions learned via MLE and Bayesian estimation. Decisions selected 
  by maximizing expected utility across a custom utility matrix.
- **Fuzzy Logic** (via scikit-fuzzy): Mamdani-style system with fuzzified 
  continuous inputs, 18 IF-THEN rules, and defuzzification followed by 
  threshold-based risk classification and the same utility-matrix decision 
  step.

## Repository Contents

- `Code.ipynb` — full implementation and analysis
- `Thesis Paper.pdf` — full written thesis
- `Bayesian Network Visualization.png` — model structure

![Bayesian Network Structure](./Bayesian%20Network%20Vizualization.png)
