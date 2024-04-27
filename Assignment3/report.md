# Report

## Personal Information
- Name and Surname: Kirill Batyshchev
- University Email: k.batyshchev@innopolis.university
- CodaLab Nickname: Nightborn

## GitHub Repository
Link to the GitHub repository: [Link to GitHub Repository](https://github.com/BatyshchevKirill/nlp-assignments/new/main/Assignment3)

## Solution Approach

### Solution 1
This is a baseline solution for the problem. It is dictionary-based. We put in a dictionary all labels corresponding to all labeled phrases in a dictionary. 
Then, in the test set we search for the occurrences of the phrases and label them based on the labels in the dictionary

**Dev F1 score**: Unknown
**Test F1 score**: 0.40

### Solution 2
This is a BERT-based solution. I used a [pretrained BERT](https://huggingface.co/DeepPavlov/rubert-base-cased) for Russian language. First, I parsed the text using the tokenizer of the model
and labeled each token. Then, I split the sentences to parts of 510 tokens (the pretrained model takes only 512 tokens, 2 of which are CLS and EOS). In case the sentences were too small (which is the most often case)
I padded them. Then, I constructed a model, that consisted of Linear layer matching each token to 58 labels (29 types of B-tags and I-tags) and a sigmoid activation function. I used a Binary Cross Entropy as a loss
to evaluate the performance of the model. This version seemed to have a weakness in predicting sequences of labels (it could predict sequence like this: "B-DATE, I-DATE, no-tag, I-DATE..."; or start a sequence with
I-tag). So, I added a convolutional layer preceded by a tanh activation function. In this way, the model could take into account the labels of neighbouring elements. This helped to improve the model's performance 
significantly. The model shows quite good results in terms of human evaluating, but the F1-score is not high. This is related to the nature of the F1-score: situation like this (correct: "B-DATE, I-DATE, I-DATE,
I-DATE, I-DATE", predicted: "B-DATE, I-DATE, I-DATE, I-DATE, no-tag") will result to a drop in F1 score even though the label is predicted correctly and most of the tokens were marked successfully. The model could 
have shown a better result if IOU or something similar was used instead for each label.


**Dev F1 score**: Unknown
**Test F1 score**: 0.47

## Best Solution
The better solution in terms of the F1-score on the test set is a model based on BERT. BERT-based models leverage contextualized word representations learned from large unlabeled corpora, allowing them to better capture semantic and syntactic information compared to dictionary-based approaches that rely on predefined lexicons and rules, resulting in superior performance for complex tasks like Nested Named Entity Recognition.
