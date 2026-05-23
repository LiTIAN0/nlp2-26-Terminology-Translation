# Terminology Translation with Lightweight LLMs

This repository contains the code, data, and translation outputs for our project investigating terminology constraint adherence in lightweight language models (Qwen2.5-1.5B-Instruct). This work was conducted for the WMT25 Terminology Shared Task.

## Overview

Enforcing terminology constraints in machine translation is critical for specialized domains. While massive models excel at this, lightweight models often struggle. We explore four targeted interventions:
* **RQ1 (Inference):** Terminology-injection prompting.
* **RQ2 (Data):** Synthesizing 5000 terminology-rich training examples from the News Commentary corpus.
* **RQ3 (Training):** Parameter-Efficient Fine-Tuning (LoRA) to adapt the model to dictionary constraints.
* **RQ4 (Advanced):** A multi-step self-refinement pipeline that decouples fluency generation from terminology insertion.

Our findings reveal an **alignment tax**: fine-tuning improves general translation fluency but degrades the ability of the model to follow explicit dictionary rules. Conversely, multi-step reasoning improves terminology accuracy on the base model.

## Repository Structure

* `nlp2.ipynb`: The complete Jupyter Notebook containing the translation pipeline, data augmentation, LoRA fine-tuning, and multi-step inference code. This Notebook is developed in Google Colab. GitHub may not render it correctly due to widget metadata compatibility issues, but the notebook itself is fully functional.
* `translations/`: Contains the 12 generated translation files (JSONL format) across the three evaluation modes (Proper, Random, NoTerm) for all tested systems.
* `track1_score_dict.json`: The final evaluation dictionary containing ChrF++ and Terminology Success Rates used for our report visualizations.
* `track1_score_dict_full.json`: The raw evaluation output including the consistency metrics.
* `plots/`: The visualization of evaluation result.

## Evaluation Notes and Limitations

We utilized the official automated evaluation scripts from the [WMT25 Terminology Shared Task repository](https://github.com/Sethjsa/nlp2-26/tree/main/wmt25-terminology). 

**Important Note on Consistency Metrics:** The `track1_score_dict_full.json` file contains metrics for `consistency_frequent` and `consistency_predefined`. We identified a critical bug in the official evaluation script where the external 0.5B alignment model systematically fails to extract terms. This triggers a fallback mechanism resulting in degenerate scores (exactly 1.0 and 0.0) across all systems. Consequently, these consistency metrics are mathematically broken and were excluded from our final analysis. The `track1_score_dict.json` contains the cleaned data used for our report.

## Reproduction Steps

1. Clone this repository.
2. Open `nlp2.ipynb` in Google Colab or a local Jupyter environment with GPU support.
3. Install the required dependencies.
4. Run the cells sequentially to execute data synthesis, fine-tuning, and generation.
5. Execute the post-processing cleaning code on the output JSONL files to extract German translation.
6. Evaluate the cleaned files using the official WMT25 metrics script.
