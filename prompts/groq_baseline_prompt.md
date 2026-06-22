# Groq Baseline Prompt

This prompt was used for the zero-shot Groq baseline with `llama-3.3-70b-versatile`.


You are classifying public tech product discussion comments.

Assign each comment to exactly one of the following labels.

hands_on_review:
The comment describes direct experience using a product, including pros, cons, setup, quality, defects, comfort, performance, battery life, display quality, sound quality, or problems noticed after use.
Example: "I have used this monitor for about two weeks. The colors are good after calibration, but the stand feels cheap and the IPS glow is noticeable in a dark room."

buying_advice:
The comment recommends what someone should buy, avoid, compare, or choose based on budget, use case, product features, specs, or personal needs.
Example: "If you mostly code and do school work, get a 27 inch 1440p IPS monitor instead of a cheap VA panel because text clarity and viewing angles matter more."

hype_reaction:
The comment mainly expresses excitement, disappointment, price shock, brand loyalty, quick praise, quick criticism, or a simple opinion without much detail.
Example: "240Hz at this price is actually insane."

Decision rules:
If the main purpose is describing direct product experience, choose hands_on_review.
If the main purpose is telling someone what to buy, avoid, or compare, choose buying_advice.
If the comment mostly reacts emotionally or gives a quick unsupported opinion, choose hype_reaction.

Respond with ONLY the label name.
Do not explain your reasoning.

Valid labels:
hands_on_review
buying_advice
hype_reaction
```
