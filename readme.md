# Reddit Opiates Substances & Effects NER Model
## Summary

This is a Named Entity Recognition model intended to predict SUBSTANCES and EFFECTS related to opioid use and withdrawal. The model was trained on several thousand reddit comments from the r/opioids and r/OpiatesRecovery subreddits. There are two labeled entities in this model described below:

**Effect**: a theraputic or adverse effect mentioned as a result or rationale for consuming a substance.  
**Substance**: a drug, remedy, supplement, or other consumable item used to treat an effect or induce a desired effect.

## Training & Performance Notes

The model was evaluated with a random 20% validation sample to understand performance, then retrained with the full dataset for 10 epochs. There is no "gold label" dataset for benchmarking performance. There was no fine-tuning of training hyperparameters and the [default spaCy](https://web.archive.org/web/20201023185142if_/https://spacy.io/api/cli#train-hyperparams) settings were used.

Eval Training log:

```
Created and merged data for 6507 total examples
Using 5206 train / 1301 eval (split 20%)
Component: ner | Batch size: compounding | Dropout: 0.2 | Iterations: 10
ℹ Baseline accuracy: 0.000

=========================== ✨  Training the model ===========================

#    Loss       Precision   Recall     F-Score 
--   --------   ---------   --------   --------
 1   152483.87      84.111     79.974     81.990                                                                                                                                                                            
 2   141551.18      86.424     85.535     85.977                                                                                                                                                                            
 3   135820.19      87.415     86.853     87.133                                                                                                                                                                            
 4   134907.52      88.024     88.364     88.194                                                                                                                                                                            
 5   133514.60      88.433     89.457     88.942                                                                                                                                                                            
 6   133879.57      88.787     90.100     89.438                                                                                                                                                                            
 7   133341.15      88.645     90.839     89.729                                                                                                                                                                            
 8   132637.67      88.610     90.775     89.679                                                                                                                                                                            
 9   131770.47      88.430     90.903     89.650                                                                                                                                                                            
10   132331.47      88.621     90.871     89.732                                                                                                                                                                            

============================= ✨  Results summary =============================

Label       Precision   Recall   F-Score
---------   ---------   ------   -------
EFFECT         79.724   83.768    81.696
SUBSTANCE      91.237   92.895    92.059


Best F-Score   89.732
```