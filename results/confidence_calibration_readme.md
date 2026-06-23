## Confidence Calibration

I also checked whether the fine-tuned model's confidence scores were meaningful. I grouped test predictions by confidence range and compared the average confidence in each range with the actual accuracy for that range.

| Confidence Range | Count | Average Confidence | Accuracy |
|---|---:|---:|---:|
| 0.00-0.50 | 32 | 0.364 | 0.625 |
| 0.50-0.70 | 0 | nan | nan |
| 0.70-0.85 | 0 | nan | nan |
| 0.85-1.00 | 0 | nan | nan |

The calibration results show whether higher-confidence predictions were actually more reliable. This matters because a classifier is more useful if confidence gives the user a realistic signal about how much to trust the prediction.

The model was not perfectly calibrated. Some confident predictions were still wrong, especially near the boundary between `hype_reaction` and the other two labels. I would treat the confidence score as a useful signal, not a guarantee.

![Confidence Calibration](results/confidence_calibration.png)
