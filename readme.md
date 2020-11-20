# Reddit Opiates Substances & Effects NER Model
## Summary

This is a Named Entity Recognition model intended to predict SUBSTANCES and EFFECTS related to opioid use and withdrawal. The model was trained on several thousand reddit comments from the r/opioids and r/OpiatesRecovery subreddits. There are two labeled entities in this model described below:

**Effect**: a theraputic or adverse effect mentioned as a result or rationale for consuming a substance.  
**Substance**: a drug, remedy, supplement, or other consumable item used to treat an effect or induce a desired effect.

## Model Use

```python
import spacy
nlp = spacy.load("en_reddit_opioid_substance_effects_ner")
t = """Yeah I know subs. Just never used them. Mdone is much cheaper to get and you dont have to wait untill you can use them. But I think if you really want to get off your DOC then subs are more powerfull cause they block the receptors. I myself have taken once 120mg of mdone and craved H so much that I used it to and nearly OD'd but everytime I detox I use Diazepam too. It really helps me with the insomnia and the RLS. But I prefer Xanax more. They are stronger and most of the detox time I sleep cause of them but I take high doses like 10-15mg per day plus 100mg of Diazepam through out the day and take 3 times a day the 5mg red devil Xans. It works wonders for me. Then I also use Loperamid against the diarrhea. And yeah that makes wds easy for me. Benzos are really a godsend for wds. If you have enough then detox is super easy. The last time I detoxed I had nothing because of my poor planing amd it was super hard but it was bearable. Just the puking was the hell and the RLS and insomnia. But after 5 days everything was over and I managed to stay sober for a few weeks."""

print(*[(ent.text, ent.label_) for ent in nlp(t).ents], sep="\n")
```
```
('subs', 'SUBSTANCE')
('Mdone', 'SUBSTANCE')
('subs', 'SUBSTANCE')
('mdone', 'SUBSTANCE')
('H', 'SUBSTANCE')
('Diazepam', 'SUBSTANCE')
('insomnia', 'EFFECT')
('RLS', 'EFFECT')
('Xanax', 'SUBSTANCE')
('Diazepam', 'SUBSTANCE')
('red devil Xans', 'SUBSTANCE')
('Loperamid', 'SUBSTANCE')
('diarrhea', 'EFFECT')
('Benzos', 'SUBSTANCE')
('puking', 'EFFECT')
('RLS', 'EFFECT')
('insomnia', 'EFFECT')
```

## Model Notes

There are actually two models available. To install the **smaller model** trained from scratch:

```bash
pip install git+https://github.com/RTIInternational/en_reddit_opioid_substance_effects_ner/
```

There is a **large model** that uses spacy's `en_core_web_lg` model as a base. This will have a different vocabulary, and could be more robust when applied to non-reddit text. Due to the model size, this model is available as a github release, which can be downloaded using:

```bash
pip install https://github.com/RTIInternational/en_reddit_opioid_substance_effects_ner/releases/download/v0.0.1/en_reddit_opioid_substance_effects_ner-base_lg-0.0.1.tar.gz
```

To use the large model, append the model name with `-base-lg`, e.g. `nlp = en_reddit_opioid_substance_effects_ner-base_lg`

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