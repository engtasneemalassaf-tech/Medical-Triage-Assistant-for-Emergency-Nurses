## Clinical Symptom Screening Assistant (for Nurses) ##
## Scenario & Target Users (Detailed)

Healthcare professionals—especially nurses—often work in high-pressure environments where patient volume is high and time per patient is limited. In these settings, early symptom screening and triage are frequently repetitive, yet critical. Repeating the same symptom questions across many patients increases cognitive load and can raise the risk of overlooking important details.

This project introduces a Generative AI Screening Assistant designed to support nurses and clinical assistants during the early-stage patient screening process. The assistant helps structure symptom information quickly and consistently before medical consultation.

The system accepts symptom descriptions in natural language (e.g., “fever, cough, fatigue”), then provides:
- Possible condition suggestions as **non-diagnostic hypotheses** rather than definitive diagnoses
- Basic precautions and safe follow-up guidance (e.g., what to monitor, when to seek urgent help)
- Confidence-aware responses using thresholds (e.g., escalation when confidence < 0.70)
- Safety escalation when severe or high-risk symptoms are detected (e.g., chest pain, difficulty breathing)

The assistant is explicitly designed as a **decision-support tool**, not a medical authority. It always provides a clear disclaimer and encourages professional medical consultation, especially in uncertain or high-risk cases.


## Escalation Triggers & Paths (with Severity Levels + Feedback)

**3 Severity Levels:**
- **Low (Yellow)**: Low conf or few symptoms → Clarify & monitor
- **Medium (Orange)**: Ambiguity or moderate concern → Physician review
- **High (Red)**: Emergency or mental health risk → Immediate ER/crisis

**Feedback Buttons:**
- "Helpful" → Logs positive case
- "Escalate now?" → Triggers Level 3 + logs

**Bias Considerations:**
- Dataset limited to 41 diseases (possible under-representation)
- Mitigation: stratified split, high fuzzy threshold, strong disclaimer, feedback loop for future debiasing
  
- ## Model A vs Model B

| Aspect       | Model A (Logistic Regression)          | Model B (Flan-T5-base + RAG)             |
|--------------|----------------------------------------|------------------------------------------|
| Purpose      | Precise top-5 disease ranking          | Actionable nurse guidance                |
| Speed        | Very fast                              | Moderate (CPU)                           |
| Safety       | No hallucination                       | Grounded by RAG + strict prompt          |
| Output       | Ranked list + confidence               | 4-line actionable advice                 |
| Best For     | Ranking possibilities                  | Practical nurse steps under pressure     |

Hybrid use: Model A ranks, Model B advises safely.

## Evaluation

**15 test cases** were used:
- 5 common respiratory
- 4 red-flag emergencies
- 3 few-symptom cases
- 3 ambiguous cases

**Key Results**
- Model A accuracy: 98.2%
- Red-flag escalation: 100% correct
- Model B actionability: High (4-line nurse-focused responses)

**Failure Cases & Mitigations**
1. Very short input → Escalation Level 1 + prompt to clarify
2. Rare disease → Low confidence + strong disclaimer
3. Repetition in Model B → Fixed by repetition_penalty + fallback template

Feedback loop logs are saved for future improvement.



## Prompt Engineering (3 Strategies)

1. **Role + Basic** → Vague & long responses
2. **Few-shot Structured** → Better structure, still repetitive
3. **Chain-of-Thought + RAG + Guardrails (Final)** → 4-line actionable format, grounded in dataset, safe, concise

Iteration evidence: Reduced response length from 8–12 lines to 4 lines, eliminated disease name hallucination, improved actionability.
## Optimization Step

**What was optimized:** Model B prompt

**Before:** Long (9+ lines), repetitive, occasional hallucination of disease names

**After (Chain-of-Thought + RAG grounding + strict guardrails):**
- Reduced to exactly 4 lines
- No repetition
- No disease name hallucination
- Always includes actionable nurse steps

Result: Much clearer and safer responses under time pressure.
## Ethics & Safety Considerations

- Strong disclaimer on every screen: "NOT a medical diagnosis"
- Tiered escalation suppresses Model B in high-risk cases
- Dataset bias mitigated by stratified split + escalation for low confidence
- No personal data collected; logs are anonymized
- Strict prompt guardrails prevent hallucination and unsafe advice
- Designed for educational/supervised use only

Always consult a licensed clinician. This is a prototype.
# Medical Screening Assistant – Nurse Triage Chatbot

**Educational prototype** for CO7201 Individual Project – University of Leicester

**Author:** tasneem  
**Date:** January 31, 2026

## Overview
Nurse-friendly symptom screening tool using:
- Model A: Logistic Regression
- Model B: Flan-T5-base + RAG (grounded in dataset)

**Important:** Educational prototype only. NOT a medical diagnosis.

## How to Run (Google Colab – 3 steps)

1. Open a new Google Colab notebook
2. Copy all cells from the notebook and run them in order
3. The Gradio UI will launch automatically

## Requirements
```bash
pip install -r requirements.txt
gradio==4.42.0
pandas
scikit-learn
fuzzywuzzy
python-Levenshtein
kagglehub
transformers
torch
