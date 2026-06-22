# TakeMeter Planning

## Project Title

TakeMeter: Tech Product Discussion Classifier

## Project Idea

For this project, I am building a text classifier that labels public tech product discussion comments by the type of contribution the comment makes.

The classifier will not judge the person who wrote the comment. It will only classify the comment itself. The goal is to separate hands-on product reviews, buying advice, and quick hype or reaction comments.

This topic fits TakeMeter because tech product communities are full of opinions. People talk about laptops, monitors, keyboards, headphones, smartphones, chargers, and PC accessories in different ways. Some comments are based on real product use, some comments tell another person what to buy, and some comments are mostly quick reactions to price, specs, or brand reputation.

## Community Choice

I chose public tech product discussion communities, mainly public Reddit-style discussions about laptops, monitors, keyboards, headphones, smartphones, chargers, and PC accessories.

This is a good community for a classification task because the discourse is active, text-heavy, and varied. A regular reader in these communities would understand the difference between someone who has actually used a product, someone giving buying advice, and someone just reacting to a deal or product.

These distinctions matter because people often use tech product comments to make real buying decisions. A hands-on review gives different value than a short hype reaction.

## Labels

I will use three labels:

1. `hands_on_review`
2. `buying_advice`
3. `hype_reaction`

These labels are intended to be mutually exclusive. Each comment should fit into the label that best describes its main purpose.

## Label 1: `hands_on_review`

A `hands_on_review` comment describes direct experience using a product. It may mention pros, cons, setup, quality, defects, comfort, performance, battery life, display quality, sound quality, or problems noticed after use.

Example 1:

> I have used this monitor for about two weeks. The colors are good after calibration, but the stand feels cheap and the IPS glow is noticeable in a dark room.

Example 2:

> I bought this keyboard last month. The switches feel smoother than I expected, but the stabilizers are still a little rattly out of the box.

Why this label matters:

A hands-on review is useful because it gives the reader information from actual product experience instead of just an opinion or guess.

## Label 2: `buying_advice`

A `buying_advice` comment recommends what someone should buy, avoid, compare, or prioritize based on budget, use case, product features, specs, or personal needs.

Example 1:

> If you mostly code and do school work, get a 27 inch 1440p IPS monitor instead of a cheap VA panel because text clarity and viewing angles matter more.

Example 2:

> If battery life is your top priority, get the MacBook. If repairability and Linux support matter more, get the Framework.

Why this label matters:

Buying advice is useful because it helps someone make a decision. These comments usually connect product features to a person’s actual needs.

## Label 3: `hype_reaction`

A `hype_reaction` comment mainly expresses excitement, disappointment, price shock, brand loyalty, quick praise, quick criticism, or a simple opinion without much detail.

Example 1:

> 240Hz at this price is actually insane.

Example 2:

> That laptop is overpriced garbage.

Why this label matters:

Hype reactions are common in product communities, but they usually give less detailed information than reviews or buying advice. They can still show community sentiment, but they are not the same as evidence-based advice.

## Hard Edge Cases

Some comments can sit between two labels. I will use decision rules to keep the labels consistent.

### Edge Case 1: Review vs. Advice

Example:

> I have this monitor and it looks great after calibration, so I would buy it if you can get it under $180.

Possible labels:

* `hands_on_review`
* `buying_advice`

Decision rule:

If the comment mainly describes the writer’s own experience with the product, I will label it `hands_on_review`. If the main purpose is telling another person what to buy or avoid, I will label it `buying_advice`.

Final decision for this example:

`buying_advice`, because the comment ends with a direct buying recommendation.

### Edge Case 2: Hype vs. Buying Advice

Example:

> This deal is crazy. Buy it before it sells out.

Possible labels:

* `hype_reaction`
* `buying_advice`

Decision rule:

If the comment only says to buy something but does not explain why, I will label it `hype_reaction`. If it gives a reason based on use case, specs, price comparison, or product quality, I will label it `buying_advice`.

Final decision for this example:

`hype_reaction`, because it is mostly excitement and urgency without reasoning.

### Edge Case 3: Review vs. Hype Reaction

Example:

> I got this yesterday and it is amazing.

Possible labels:

* `hands_on_review`
* `hype_reaction`

Decision rule:

A comment must include at least one specific product detail to count as `hands_on_review`. If the comment only says the product is amazing, terrible, insane, or disappointing without details, I will label it `hype_reaction`.

Final decision for this example:

`hype_reaction`, because it mentions ownership but does not describe anything specific about the product.

## Data Collection Plan

I will collect at least 200 public comments from tech product discussion communities. My target is 210 comments so that the dataset can stay balanced across the three labels.

Target distribution:

| Label             | Target Count |
| ----------------- | -----------: |
| `hands_on_review` |           70 |
| `buying_advice`   |           70 |
| `hype_reaction`   |           70 |
| **Total**         |      **210** |

Planned sources include public tech product discussions about:

* laptops
* monitors
* keyboards
* headphones
* smartphones
* chargers
* PC accessories
* repairable or upgradeable hardware

I will only use public comments. I will not include usernames, private messages, profile links, or personal identifying information.

The dataset will be saved as one CSV file with these columns:

```text
text,label,notes,source
```

The `text` column will contain the comment. The `label` column will contain one of the three labels. The `notes` column will explain any difficult labeling decisions. The `source` column will describe the general source, such as a public product discussion thread.

Before collecting the full dataset, I will read a smaller pilot batch to check that the labels work on real comments. If too many comments are hard to label, I will revise the decision rules before labeling the full dataset.

If one label is underrepresented, I will collect more examples for that label. For example, if I need more `hands_on_review` examples, I will look for comments where people describe products they own. If I need more `buying_advice` examples, I will look for recommendation threads. If I need more `hype_reaction` examples, I will look for deal threads and new product discussions.

## Annotation Process

I will label each comment by reading it and asking what the main purpose of the comment is.

The annotation rules are:

1. If the comment mainly describes direct product use, label it `hands_on_review`.
2. If the comment mainly helps another person decide what to buy or avoid, label it `buying_advice`.
3. If the comment mainly reacts with excitement, disappointment, quick praise, quick criticism, or price shock, label it `hype_reaction`.

I will not create an `other` label. If a comment does not fit any label clearly, I will skip it instead of forcing it into the dataset.

I will also use the `notes` column for comments that are difficult to label so I can discuss those examples later in the README.

## Evaluation Metrics

I will evaluate both the fine-tuned model and the Groq zero-shot baseline on the same held-out test set.

The metrics I plan to use are:

1. Overall accuracy
2. Per-class precision
3. Per-class recall
4. Per-class F1 score
5. Confusion matrix
6. Wrong prediction analysis

Accuracy is useful because it gives a simple overall score. However, accuracy alone is not enough because the model could perform well overall while failing on one label.

Per-class precision, recall, and F1 are important because each label represents a different type of comment. I want to know whether the model can handle all three labels, not just the easiest one.

The confusion matrix will show which labels are being confused. I expect the hardest mistakes to happen between `hands_on_review` and `buying_advice`, or between weak `buying_advice` and `hype_reaction`.

Wrong prediction analysis will help explain what the model actually learned. I will review at least three wrong predictions and explain why each one failed.

## Baseline Plan

The baseline model will be Groq’s `llama-3.3-70b-versatile` used as a zero-shot classifier.

I will give the model my label definitions and ask it to classify each test example without task-specific training. The prompt will require the model to return only one valid label name.

Valid labels:

```text
hands_on_review
buying_advice
hype_reaction
```

This baseline matters because it shows whether fine-tuning DistilBERT actually helped compared to a strong general-purpose model.

## Fine-Tuning Plan

I will fine-tune `distilbert-base-uncased` using the starter Colab notebook.

The notebook will handle:

* loading the CSV
* mapping string labels to numeric labels
* splitting the dataset into train, validation, and test sets
* tokenizing the comments
* training DistilBERT
* evaluating on the test set
* generating the confusion matrix
* exporting `evaluation_results.json`

Planned training settings:

| Setting       |                       Planned Value |
| ------------- | ----------------------------------: |
| Base model    |           `distilbert-base-uncased` |
| Epochs        |                                   3 |
| Learning rate |                                2e-5 |
| Batch size    |                                  16 |
| Split         | 70% train, 15% validation, 15% test |

I am using 3 epochs because the dataset is small. Training for too many epochs could cause the model to memorize the dataset instead of learning general patterns.

## Definition of Success

I will consider the model successful if it meets these conditions:

1. The fine-tuned model performs better than random guessing.
2. The fine-tuned model performs close to or better than the Groq zero-shot baseline.
3. No label has an F1 score of 0.
4. The model does not collapse into predicting only one label.
5. The confusion matrix shows that the model learned at least some difference between reviews, advice, and reactions.

A strong result would be around 70% or higher accuracy with reasonable per-class F1 scores. A good enough result for this class project would be a model that shows meaningful learning and has explainable errors.

For a real community tool, I would want stronger performance before deployment. I would collect more data, use more careful annotation, and add more borderline examples.

## Expected Failure Modes

I expect the model to struggle with short comments because short text often lacks enough context.

Possible failure patterns:

1. Short review-like comments may be misclassified as `hype_reaction`.
2. Excited buying recommendations may be misclassified as `buying_advice` even when they give no real reason.
3. Comments that mix personal experience and advice may confuse `hands_on_review` and `buying_advice`.
4. Sarcastic comments may be difficult because the surface wording may not match the real meaning.
5. Product-specific words may influence the model even though the labels are supposed to describe discourse type, not product type.

## AI Tool Plan

### Label Stress-Testing

Before labeling the full dataset, I will use an AI tool to stress-test my label definitions. I will give it the three label definitions and ask it to generate borderline tech product comments that could sit between two labels.

The purpose is not to create my dataset. The purpose is to find weak label boundaries before annotation. If the generated borderline examples are hard to classify, I will revise my decision rules before collecting all examples.

### Annotation Assistance

I may use an AI tool to help pre-label some comments after I collect them from public sources. If I do this, I will still review every label myself and correct mistakes.

I will disclose this in the README. I will not use AI-generated comments as the dataset because the project requires real-world text.

### Failure Analysis

After running the fine-tuned model, I will use an AI tool to help identify patterns in the wrong predictions. I will give it the misclassified examples and ask what patterns might explain the errors.

I will verify the suggested patterns myself by rereading the examples. I will only include patterns in the README if they are supported by the actual wrong predictions.

## Current Project Checklist

* [x] Choose community
* [x] Define labels
* [x] Write planning document
* [x] Collect public comments
* [x] Label full dataset
* [x] Run Groq baseline
* [x] Fine-tune DistilBERT
* [x] Evaluate results
* [x] Write README
* [x] Record demo
