# Label Stress Test Prompt

This prompt was used before full annotation to test whether the label definitions were clear.

I am building a text classifier for public tech product discussion comments.

My labels are:

hands_on_review:
A comment that describes direct experience using a product. It may mention pros, cons, setup, quality, defects, comfort, performance, battery life, display quality, sound quality, or problems noticed after use.

buying_advice:
A comment that recommends what someone should buy, avoid, compare, or prioritize based on budget, use case, product features, specs, or personal needs.

hype_reaction:
A comment that mainly expresses excitement, disappointment, price shock, brand loyalty, quick praise, quick criticism, or a simple opinion without much detail.

Generate 10 borderline tech product comments that could sit between two labels.

For each example, include:
1. the comment
2. the two labels that could be confused
3. the final label you would choose
4. a short reason why

Focus on difficult cases such as:
- comments that mention ownership but give no details
- comments that say "buy it" without explaining why
- comments that mix personal experience with a recommendation
- comments that react strongly to price or specs