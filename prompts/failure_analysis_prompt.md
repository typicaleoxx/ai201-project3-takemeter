# Failure Analysis Prompt

This prompt was used after model evaluation to help identify error patterns.


I fine-tuned a DistilBERT classifier for public tech product discussion comments.

The labels are:

hands_on_review:
direct product experience or ownership details

buying_advice:
recommendations about what to buy, avoid, compare, or prioritize

hype_reaction:
quick excitement, disappointment, price shock, praise, criticism, or unsupported opinion

Here is the fine-tuned model confusion matrix.

Rows are true labels and columns are predicted labels.

| True Label / Predicted Label | hands_on_review | buying_advice | hype_reaction |
|---|---:|---:|---:|
| hands_on_review | 9 | 1 | 0 |
| buying_advice | 2 | 9 | 0 |
| hype_reaction | 5 | 4 | 2 |

Please identify the main error patterns.

For each pattern, explain:
1. which labels are being confused
2. why that boundary is hard
3. whether the issue seems related to labeling, data size, or model behavior
4. what kind of extra examples would help improve the model

Keep the explanation practical and focused on the classifier.