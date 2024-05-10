# Emotion and sentiment recognition

## Introduction
Understanding human emotions is one of the more challenging tasks in natural language processing. Not only are they a very subjective topic, but humans also often lack the capability to fully express themselves in written language. Understanding the expressed emotions can require some additional context, sometimes given by external knowledge.

Nowadays, the problem of understanding the structure and subtleties of a language as well as having knowledge that is not available in the immediate context of a text is addressed by using large pre-trained models. This solution is by no means perfect and often requires additional training to fit the task at hand. Nonetheless, having associative knowledge from a lot of unlabeled texts gives noticeable gains in tasks such as emotion recognition.

## Task Definition
The goal of this task is to create a system capable of recognizing emotions from Plutchik's wheel of emotions and the corresponding sentiments conveyed in consumer reviews, both as a whole text as well as in the individual sentences of those reviews.

It is forbidden to manually label the test examples.

## Dataset
The dataset is made up of consumer reviews written in Polish. Those reviews belong to four domains: hotels, medicine, products, and school. This collection also contains non-opinion informative texts belonging to the same domains (meaning they are mostly neutral). Each sentence, as well as all the reviews as a whole, are annotated with emotions from the Plutchnik's wheel of emotions (joy, trust, anticipation, surprise, fear, sadness, disgust, anger), as well as the perceived sentiment (positive, negative, neutral), with ambivalent sentiment being labeled using both positive and negative labels. The dataset was annotated by six people who did not see each other's decisions. These annotations were aggregated by selecting labels annotated by at least 2 out of 6 people, meaning controversial texts and sentences can be annotated with opposing emotions. While each sentence has its own annotation, they were created in the context of the whole review.

For more information about this dataset see referances [1](#ref-1) and [2](#ref-2).

### Training set
Training data consists of 776 reviews containing 6393 sentences, which were randomly selected from the whole dataset. The split was done on the level of whole reviews, meaning there are no reviews that are split between sets.

### Test sets
Two test sets consist of 167 reviews each, containing 1234 and 1264 sentence annotations, respectively.

### Dataset format
The datasets are stored in three directories (training and two test sets). All datasets have the same format.

Input rows contain ordered sentences of reviews. Each review ends with a sentence made out of only the symbol #. This sentence annotation corresponds to the annotation of the whole review and is not a sentence annotation. This sentence is not a part of the original review and should not be treated as such, it only marks the end of the current review and the row that contains the corresponding review annotation. The next row after such a sentence corresponds to the first sentence of a different review.

Example:

This fragment of the training input file:
```
Była to pierwsza wizyta ale moze i ostatnia.
Lakarz troche apatyczny, nie wypowiadajacy sie jasno.
Mam zrobic jakies badanie ale nie dardzo wiem jakie.
Nie napisal skierowania/zalecenia, chyba mowil o gastrologii.
Powinnam byla byc bardzej wymagajaca i dopytujaca.
Nie polecam tego lekarza.
###########################
```
corresponds to annotations:
```
False	False	True	False	False	True	False	False	False	True	False
False	False	False	False	False	True	True	False	False	True	False
False	False	False	True	False	True	False	False	False	True	False
False	False	False	True	False	True	False	False	False	True	False
False	False	False	True	False	True	False	True	False	True	False
False	False	False	False	False	True	False	False	False	True	False
False	False	False	True	False	True	False	False	False	True	False
```
meaning sentences are labeled as:
```
"Była to pierwsza wizyta ale moze i ostatnia." - anticipation, sadness, negative
"Lakarz troche apatyczny, nie wypowiadajacy sie jasno." - sadness, disgust, negative
"Mam zrobic jakies badanie ale nie dardzo wiem jakie." - surprise, sadness, negative
"Nie napisal skierowania/zalecenia, chyba mowil o gastrologii." - surprise, sadness, negative
"Powinnam byla byc bardzej wymagajaca i dopytujaca." - surprise, sadness, anger, negative
"Nie polecam tego lekarza." - sadness, negative
```
and the review as a whole, starting from "Była to pierwsza wizyta ale moze i ostatnia." and ending at "Nie polecam tego lekarza." is labeled as: surprise, sadness, negative.


## Evaluation
The final evaluation metric used is the arithmetic mean of two F1 macro scores. One of which is calculated on only the text annotations, and the other is calculated on only the sentence annotations.


$$ Final\:score = \frac{F1_{macro}\:sentences + F1_{macro}\:texts}{2}$$
where each $F1_{macro}$ is calculated by:
$$ F1_{macro} = \frac{\sum_{i=1}^n F1_i}{n} $$
Where n is the number of labels, and F1 for each label is given by the equation:
$$ F1 = 2\frac{precision*recall}{precision + recall}$$
The metric is only properly defined when $precision + recall \neq 0$. If this case is encountered, the calculated metric for that label will be set to 0.
$$ precision = \frac{TP}{TP + FP}$$
The metric is only properly defined when $TP + FP \neq 0$ where $TP$ and $FP$ represent the number of true positives and false positives, respectively. If this case is encountered, the calculated metric for that label will be set to 0.
$$ recall = \frac{TP}{TP + FN}$$
The metric is only properly defined when $TP + FN \neq 0$ where $TP$ and $FN$ represent the number of true positives and false negatives, respectively. If this case is encountered, the calculated metric for that label will be set to 0.

### Submission format
The goal of the task is to classify whether an emotion is contained in a text or not. The submission should consist of a single tab-separated file. Each of the eleven columns should contain one boolean value indicating whether a specific emotion is contained or not. Each line should contain annotations relevant to the matching row from the in.tsv file.

Example:
```
True	True	False	False	False	False	False	False	True	False	False
```

## References
<span id="ref-1">1. Koptyra, Bartłomiej, et al. "CLARIN-Emo: Training Emotion Recognition Models Using Human Annotation and ChatGPT." International Conference on Computational Science. Cham: Springer Nature Switzerland, 2023.</span>
<span id="ref-2">2. Kocoń, Jan, et al. "ChatGPT: Jack of all trades, master of none." Information Fusion (2023): 101861.</span>

## Metadata



