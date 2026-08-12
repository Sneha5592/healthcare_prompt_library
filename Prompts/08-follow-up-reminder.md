# Prompt 8: Follow up reminder

## Workflow
Creates a patient facing reminder for doctor follow ups and presribed medicines and their dosages.

## Problem being solved
Staff often have to manually write the diagnosis result and prescribed medicines and their dosage to the patients . This can be time consuming to write for every case . This automation task drafts a small patient facing follow up reminder which includes diagnosis, prescribed medicines, their dosages and short notes(if any) to be sent directly to the patient.

## Version 1
```
role: you are a patient communication assistant at a multi-specialty diagnostics clinic. writing a warm, clear, and reassuring message to a patient about their prescribed medication and follow-up care.

task: Write a brief and clear message to the patient informing them about the upcoming follow up appointments(if any) and the prescribed medicines, their dosage and potential side effects from the given input.

input:
{
  "patient_name": "Sarah Jones",
  "diagnosis_summary": "Persistent stomachache requiring gastrointestinal evaluation. Patient advised to monitor symptoms, follow dietary recommendations, and attend the scheduled Gastroenterology appointment for further assessment.",
  "medication": [
    {
      "name": "Omeprazole",
      "dosage": "20 mg once daily before a meal"
    }
  ],
  "follow_up_date": "2026-08-18"
}

rules:
1. The tone should be professional and genuine. 
2. Do not fabricate information. Fall back to "N/A" if not available.
3. The reminder should mimic how real world reminders are actually structured.
```

**output:**
```
Dear Sarah Jones,

This is a reminder regarding your prescribed medication and follow-up care.

Medication:Omeprazole
Dosage: 20 mg once daily
potential side effects: N/A

Follow-up appointment: September 12, 2026

Please take your medication as prescribed and continue monitoring your blood pressure as advised. Please also continue following the lifestyle recommendations discussed with your care team.

We look forward to seeing you at your follow-up appointment.
```

**issues:** There is no rule mentioning side effect which can cause the model to make up compelling information. No notes on what to do if any side effects occur and preventing the model from making decisions for the patients. 

## Version 2 
```
role: you are a patient communication assistant at a multi-specialty diagnostics clinic. writing a warm, clear, and reassuring message to a patient about their prescribed medication and follow-up care.

task: Write a brief and clear message to the patient informing them about the upcoming follow up appointments(if any) and the prescribed medicines, their dosage and potential side effects from the given input.

input:
{
  "patient_name": "Sarah Jones",
  "diagnosis_summary": "Persistent stomachache requiring gastrointestinal evaluation. Patient advised to monitor symptoms, follow dietary recommendations, and attend the scheduled Gastroenterology appointment for further assessment.",
  "medication": [
    {
      "name": "Omeprazole",
      "dosage": "20 mg once daily before a meal"
    }
  ],
  "follow_up_date": "2026-08-18",
  "potential_side_effects": "headache, nausea, stomach pain, diarrhea, constipation, and gas"
}

rules:
1. The tone should be professional and genuine. 
2. Do not fabricate information. Fall back to "N/A" if not available.
3. The reminder should mimic how real world reminders are actually structured.
4. List the side effects clearly while providing a note to the patient to seek immediate medical care if any side effects escalate.
```

**output:**
```
Dear Sarah Jones,

This is a friendly reminder about your prescribed medication and upcoming follow-up care.

**Medication:** Omeprazole
**Dosage:** 20 mg once daily before a meal

**Potential side effects:**

* Headache
* Nausea
* Stomach pain
* Diarrhea
* Constipation
* Gas

Please take your medication as prescribed, continue monitoring your symptoms, and follow the dietary recommendations provided by your care team.

**Gastroenterology follow-up:** August 18, 2026

If any of the listed side effects become severe or significantly worsen, please seek immediate medical care.

Warm regards,
Patient Care Team

```

**changes:** Added explicit rules around side effects , added side effect in the input to test how the model generates and structures the output

## Automation Potential
This task has a medium to high automation potential cutting upto 80% of manual routine reminder task. The reminder itself is highly automation friendly . But the nature of the task which consists medical details like potential side effects, a manual reviewer has to double check the reminders before sending it to the patients. This prevents the ai from making up information which can be medically risky.

## Risks and limitations

- **Fabricated information:** The ai could make up information sorrounding medications and potential side effects and the note. This is risky if the patient is informed about unrelated medicines or dangerous side effects. mitigated by strict rule to stop the model from making up information
- **Upstream dependency:** If the input recieved by the model already has errors like date mismatch and diagnosis errors. The model could draft false reminders because of its context and output being tied to the input recieved.

