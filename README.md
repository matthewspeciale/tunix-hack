# Google Tunix Hackathon (Kaggle) — Reasoning-Tuned Gemma 2 2B

This repository contains my team’s submission to **Google’s Tunix Hackathon** (Nov 11, 2025 → Jan 12, 2026).  
The goal was to fine-tune **Gemma 2 2B** to produce **structured reasoning** before final answers.

## 🔎 What’s in this repo

- **Training notebook:** [`hackathon-final.ipynb`](hackathon-final.ipynb)  
  Runs end-to-end on a **Kaggle TPU v5e-8** and produces the fine-tuned model.
- **Write-up:** [`tunix-write-up.pdf`](tunix-write-up.pdf)  
  Details data creation, training strategy (SFT → DPO → GRPO), and evaluation.
- **Overview video:** https://www.youtube.com/watch?v=AJ_LHMeszds  
- **Final Kaggle model:** https://www.kaggle.com/models/sandeeplleb/tunix-gemma2-2b-grpo-final

## 🧠 Approach (high level)

Training was done in three stages:
1. **SFT** to teach the target format and baseline reasoning behavior
2. **DPO** to improve response preference/quality (chosen vs rejected pairs)
3. **GRPO** with programmatic rewards to improve accuracy + formatting consistency

## 🏷️ Output Format (Reasoning Tags)

We trained the model to consistently separate reasoning from the final response using explicit tags:

```xml
<reasoning>
...model reasoning...
</reasoning>
<answer>
...final answer...
</answer>
```

During training and evaluation, outputs were also scored for tag compliance (presence + correct ordering) in addition to answer quality.

## 📈 Results (held-out evaluation)

On a held-out test set, we observed:
- **Format compliance:** ~51% → ~95%
- **Reasoning quality:** ~5.6 → ~6.7 (1–10 scale)
- **Answer quality:** ~5.4 → ~6.4 (1–10 scale)

See the write-up for full tables and ablations.

## 🚀 Running the notebook

1. Open the notebook on Kaggle and select **TPU v5e-8**.
2. Run [`hackathon-final.ipynb`](hackathon-final.ipynb) top-to-bottom.
3. The notebook exports the fine-tuned checkpoint and logs intermediate outputs.

> **Note:** This project was built to run in Kaggle’s TPU environment; local execution may require additional setup.

## 📄 Write-up

For details on datasets, reward design, mesh configuration, and troubleshooting, see:
- [`tunix-write-up.pdf`](tunix-write-up.pdf)

## 🙌 Acknowledgements

- Google’s **Tunix** library + Kaggle TPU environment
- The **Gemma** model family
- Team member: **vennasandeepreddy**
