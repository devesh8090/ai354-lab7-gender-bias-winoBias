# AI354 Lab Assignment 7: Probing Gender Bias in Language Models - A WinoBias Mask Prediction Study

## Devesh Singh Chauhan
### I23MA002

**Assigned on:** 19/02/26

---

## Dataset
- **WinoBias Dataset**: [Link](https://huggingface.co/datasets/uclanlp/wino_bias)
- A dataset designed to test for gender bias in coreference resolution systems. It contains sentences with pro-stereotypical and anti-stereotypical gender assignments.
- **Key Feature:** Sentences are structured with two participant roles (e.g., "developer" and "designer"), where one role is typically stereotyped for a particular gender. A pronoun ("she"/"he"/"her"/"him"/etc.) is used, and the task is to correctly link it to its antecedent.

---

## Assignment Objective
Use the WinoBias dataset for a **mask prediction task** by masking its sentences at the pronoun position. The pronoun itself is the target label. Evaluate and compare the performance of two different model architectures for this task.

---

## Models to Compare
a) **Encoder-only model (e.g., BERT)** - A masked language model.
b) **Decoder-only model (e.g., GPT, LLama)** - An autoregressive language model.

---

## Evaluation Metrics
You will record and compare the following metrics for both models:

1.  **Accuracy**: The overall percentage of correctly predicted masked pronouns.
2.  **Gender Accuracy Gap**: Calculated as `accuracy(male pronouns) - accuracy(female pronouns)`. A value close to zero indicates less gender-based performance disparity.
3.  **Stereotype Preference Score**: Calculated as `P(male pronoun | female-stereotyped role)`. This measures the model's tendency to predict a male pronoun even when the correct antecedent is in a female-stereotyped role (e.g., 'receptionist', 'nurse'). A high score indicates a strong stereotype bias.

---

## Assignment Tasks

### Q1. Model Evaluation and Comparison
Consider and study the WinoBias dataset. Use it for a mask prediction task by masking its sentences at the pronoun position. The pronoun is your target label.

**Steps:**
1.  **Data Preparation**:
    - Load the WinoBias dataset.
    - Parse the sentences and identify the position of the pronoun (target).
    - Create a version of the sentence where the pronoun is replaced with a `[MASK]` token (for BERT) or prepared for next-token prediction (for decoder models).
2.  **Model Setup**:
    - Load a pre-trained BERT model (e.g., `bert-base-uncased`).
    - Load a pre-trained decoder model (e.g., `gpt2`).
3.  **Experimentation**:
    - For each model, run inference on the masked sentences to predict the masked pronoun.
    - Record the model's prediction.
4.  **Analysis**:
    - Calculate the three evaluation metrics for both models.
    - Compare the results.
    - Analyze and discuss which model shows less bias and why.

---

## Suggested Experiment Nomenclature
Based on the pattern in your previous assignments, a suitable name for this experiment could be:

**ai354-lab5-gender-bias-probing-winoBias**

---

## Implementation Details

### Data Loading and Preprocessing
- Use the `datasets` library to load the WinoBias dataset.
- Extract the `tokens` field. The pronoun is the target at a specific index (you will need to identify its position, possibly using the `coreference_clusters` field which links pronouns to their antecedents).
- Create two sets of inputs:
    - **For BERT**: Tokenize the sentence with the pronoun masked.
    - **For GPT**: Tokenize the sentence up to the word before the pronoun; the target is the pronoun token itself.

### Mask Prediction
- **BERT**: Use the `fill-mask` pipeline or manually get logits for the `[MASK]` position and compare probabilities for male/female pronoun tokens.
- **GPT**: Use the model to generate the next token probabilities at the pronoun position and compare probabilities for male/female pronoun tokens.

### Metric Calculation
- Compute accuracy separately for male-pronoun and female-pronoun samples to calculate the **Gender Accuracy Gap**.
- For the **Stereotype Preference Score**, identify sentences where the correct antecedent is in a female-stereotyped profession. Calculate the frequency with which the model predicts a male pronoun for these instances.

---

## Deliverables
- Well-documented Python code (Jupyter Notebook is acceptable).
- A table clearly showing the **Accuracy**, **Gender Accuracy Gap**, and **Stereotype Preference Score** for both models.
- A comparative analysis discussing:
    - Which model performed better overall?
    - Which model exhibited a larger gender bias?
    - How do the architectural differences (encoder-only vs. decoder-only) potentially contribute to the observed biases?
    - Limitations of this evaluation approach.

---

## Submission Guidelines
- Submit the complete code with comments.
- Include a comprehensive report (PDF or Markdown) detailing the methodology, results, and your analysis.
- Ensure all results are reproducible.
