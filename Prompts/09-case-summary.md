# Prompt 9: Case summary

## Workflow
Creates a complete, detailed summary about the patient starting from intake categorization,urgency,routing all the way to diagnosis report for internal record keeping.

## Problem being solved
Staff often have to manually draft a full case summary for every case . This can put huge workload on the staffs which can reduce efficiency as the number of cases increase. This automation helps to automatically create clear and detailed summaries about the patient case, where they were routed and the outcomes

## Version 1
```
Role: You are a clinical record keeping assistant at a multi speciality diagnostics center  responsible for compiling summaries of each patient case from inquiry to outcomes. 

task: compile a complete,factual case summary from the given input for internal record keeping. Focus more on factual data and professional structure over genuineness or compelling information.

input:{
  "Category": "Referral to a doctor",
  "Name": "Sarah Jones",
  "Contact Number": "5590995",
  "Email": null,
  "Date of Birth": "05/01/1987",
  "Primary issue": "Persistent stomachache",
  "Secondary issue": null,
  "Preferred date and time": "Aug 11 or Wednesday, Aug 12; Morning / Afternoon",
  "Referred by": null,
  "Insurance provider": null,
  "Policy/Member ID": null,
  "urgency": "routine",
  "confidence": "high",
  "urgency_reasoning": "Sarah Jones reports a persistent stomachache requiring gastrointestinal evaluation. No symptoms suggesting an emergency or rapid deterioration have been provided. Based on the available information, the case is classified as routine and appropriate for outpatient care.",
  "specialty": "Gastroenterology",
  "Non-medical department": null,
  "care_pathway": "OPD",
  "routing_reasoning": "Sarah Jones's persistent stomachache is appropriately routed to Gastroenterology for further evaluation. No non-medical issue has been identified. Given the routine urgency classification and absence of reported emergency symptoms, the appropriate care pathway is OPD.",
  "preauth_status": "approved",
  "appointment_datetime": "2026-08-18 10:00 AM",
  "input": {
    "patient_name": "Sarah Jones",
    "diagnosis_summary": "Persistent stomachache requiring gastrointestinal evaluation. Patient advised to monitor symptoms, follow dietary recommendations, and attend the scheduled Gastroenterology appointment for further assessment.",
    "medication": [
      {
        "name": "Omeprazole",
        "dosage": "20 mg once daily before a meal",
        "potential_side_effects": [
          "headache",
          "nausea",
          "stomach pain",
          "diarrhea",
          "constipation",
          "gas"
        ]
      }
    ],
    "follow_up_date": "2026-08-18"
  }
}
rules:
- Do not make up information not present in the input.
- Keep the tone very professional
- Only mention categories actually required to draft the summary. Do not force every information in the summary
```

**output**
```
Internal Patient Case Summary

Patient: Sarah Jones
Date of Birth: 05/01/1987
Contact Number: 5590995
Case Category: Referral to a doctor

Presenting Issue:
Sarah Jones reported a persistent stomachache requiring gastrointestinal evaluation. No secondary issue or emergency symptoms were documented.

Triage and Routing:
The case was classified as routine urgency with high confidence, as the available information did not indicate an emergency or rapid deterioration.

The patient was routed to Gastroenterology under the OPD care pathway for further evaluation.

Appointment and Authorization:
Preauthorization Status: Approved
Gastroenterology Appointment: August 18, 2026 at 10:00 AM
Follow-up Date: August 18, 2026

Clinical Management:
The documented assessment describes a persistent stomachache requiring gastrointestinal evaluation. The patient was advised to monitor symptoms, follow dietary recommendations, and attend the scheduled Gastroenterology appointment for further assessment.

Medication:
Omeprazole, 20 mg once daily before a meal.

Documented potential side effects:
Headache, nausea, stomach pain, diarrhea, constipation, and gas.
```

**issues:** Since no consistent structure was mentioned in the prompt, it could create different structure for each case which is not favourable because record keeping needs consistent structures across different cases.

## Version 2
```

Role: You are a clinical record keeping assistant at a multi speciality diagnostics center  responsible for compiling summaries of each patient case from inquiry to outcomes. 

task: compile a complete,factual case summary from the given input for internal record keeping. Focus more on factual data and professional structure over genuineness or compelling information.

Structure the case summary using exactly these sections, in this order. If information for a section is unavailable,write "Not recorded". do not skip the section or invent information.

1. Patient Identifier: [name, policy/member ID if available]
2. Initial Request Category: [from intake categorization]
3. Presenting Issue: [from extraction — primary/secondary issue]
4. Urgency Assessment: [urgency level + brief reasoning]
5. Routing Decision: [specialty, care pathway]
6. Pre-Authorization Status: [approved/pending/denied]
7. Visit Outcome: [diagnosis/findings from post-visit data]
8. Prescribed Treatment: [medication, dosage]
9. Follow-Up Plan: [next steps, timeline]

note: After recording all this. Write a brief professional paragraph about the whole case from beginning to follow up. Make it 4 to 5 lines maximum.

input:{
  "Category": "Referral to a doctor",
  "Name": "Sarah Jones",
  "Contact Number": "5590995",
  "Email": null,
  "Date of Birth": "05/01/1987",
  "Primary issue": "Persistent stomachache",
  "Secondary issue": null,
  "Preferred date and time": "Aug 11 or Wednesday, Aug 12; Morning / Afternoon",
  "Referred by": null,
  "Insurance provider": null,
  "Policy/Member ID": null,
  "urgency": "routine",
  "confidence": "high",
  "urgency_reasoning": "Sarah Jones reports a persistent stomachache requiring gastrointestinal evaluation. No symptoms suggesting an emergency or rapid deterioration have been provided. Based on the available information, the case is classified as routine and appropriate for outpatient care.",
  "specialty": "Gastroenterology",
  "Non-medical department": null,
  "care_pathway": "OPD",
  "routing_reasoning": "Sarah Jones's persistent stomachache is appropriately routed to Gastroenterology for further evaluation. No non-medical issue has been identified. Given the routine urgency classification and absence of reported emergency symptoms, the appropriate care pathway is OPD.",
  "preauth_status": "approved",
  "appointment_datetime": "2026-08-18 10:00 AM",
  "input": {
    "patient_name": "Sarah Jones",
    "diagnosis_summary": "Persistent stomachache requiring gastrointestinal evaluation. Patient advised to monitor symptoms, follow dietary recommendations, and attend the scheduled Gastroenterology appointment for further assessment.",
    "medication": [
      {
        "name": "Omeprazole",
        "dosage": "20 mg once daily before a meal",
        "potential_side_effects": [
          "headache",
          "nausea",
          "stomach pain",
          "diarrhea",
          "constipation",
          "gas"
        ]
      }
    ],
    "follow_up_date": "2026-08-18"
  }
}
rules:
- Do not make up information not present in the input.
- Keep the tone very professional
- Only mention categories actually required to draft the summary. Do not force every information in the summary
- The output should be plain structured text summary
```

**output:**
```
1. Patient Identifier: Sarah Jones; Policy/Member ID: Not recorded

2. Initial Request Category: Referral to a doctor

3. Presenting Issue: Primary issue: Persistent stomachache. Secondary issue: Not recorded

4. Urgency Assessment: Routine. The patient reported a persistent stomachache requiring gastrointestinal evaluation, with no symptoms provided suggesting an emergency or rapid deterioration.

5. Routing Decision: Gastroenterology; OPD care pathway

6. Pre-Authorization Status: Approved

7. Visit Outcome: Persistent stomachache requiring gastrointestinal evaluation.

8. Prescribed Treatment: Omeprazole, 20 mg once daily before a meal

9. Follow-Up Plan: Monitor symptoms, follow dietary recommendations, and attend the scheduled Gastroenterology appointment for further assessment on August 18, 2026 at 10:00 AM.

Case Summary:
Sarah Jones presented with a persistent stomachache and was classified as a routine case based on the absence of reported emergency symptoms. The case was routed to Gastroenterology through the OPD pathway, with pre-authorization approved. Omeprazole 20 mg once daily before a meal was prescribed. The patient was advised to monitor symptoms, follow dietary recommendations, and attend the scheduled Gastroenterology follow-up on August 18, 2026.
```

**changes:** explicitly mentioned structure for consistent record keeping. Added a paragraph about the case in the end to make it more detailed.

## Automation potential
This task has a very high automation potential . Automating the  summarizing process helps cut manual workload significantly . This can help staff focus more on double checking the errors in the summary instead of drafting the whole summary manually and still having to double check it.

## Risks and limitations
- **Addition of irrelevant information:** Since the input to the prompt consists of significant information related or unrelated to the case. The ai could try to fit in every information causing the summary to lose its actual value. Mitigated by strict mentioned structure for the model to follow 
- **fabricated information:** The model could try to fill in the required fields with fabricated information if not provided in the input. Mitigated by fallback mechanism built into its rules






