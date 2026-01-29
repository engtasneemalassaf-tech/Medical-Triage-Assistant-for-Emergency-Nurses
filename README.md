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
