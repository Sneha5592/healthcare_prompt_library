# Prompt 5 : Pre authorization drafting

## workflow
Drafts a formal pre-authorization request letter to a patient's insurance provider, justifying medical necessity for a referred specialist visit or diagnostic test, before the appointment takes place.

## Problems being solved
staff often have to draft insurance pre authorization letters manually for each referral which is very time consuming and prone to inconsistent formatting and missing important details. This risks delaying insurer's response and in turn, delayed patient care.

## Version 1 

```
You are a medical billing officer at a multi specialty diagnostics clinic, drafting a formal pre-authorization request to a patient's insurance provider. Draft a pre authorization request including the details mentioned below:

include: patient's name, policy id,requested service and a detailed medical justification based on the provided clinical details.

input:
{
  "Name": "sarah jones",
  "Contact Number": "5590995",
  "Email": null,
  "Date of Birth": "05/01/1987",
  "Primary issue": "Persistent stomachache",
  "Secondary issue": null,
  "Preferred date and time": "Aug 11 or Wednesday, Aug 12; Morning / Afternoon",
  "Referred by": null,
  "Insurance provider": insurance company limited,
  "Policy/Member ID": 559-009-087-12,
  "speciality": "Internal Medicine",
  "Non-medical department": null,
  "care-pathway": "OPD",
  "reasoning": "The persistent stomachache is appropriately routed to Internal Medicine based on the listed specialties, with no non-medical issue provided. The urgency is routine with high confidence, so the care pathway is OPD."
}

rules:
- do not invent policy numbers or details if not provided in the input. if any info is missing, state "N/A" instead of creating your own details.
```

**output:** 
```
PRE-AUTHORIZATION REQUEST

To: Insurance Company Limited
Subject: Pre-Authorization Request – Internal Medicine OPD Evaluation

Dear Pre-Authorization Team,

We are submitting this request for pre-authorization for the following patient and requested medical service.

Patient Information
Patient Name: Sarah Jones
Date of Birth: 05/01/1987
Contact Number: 5590995
Email: N/A
Policy/Member ID: 559-009-087-12
Insurance Provider: Insurance Company Limited

Requested Service
Specialty: Internal Medicine
Care Pathway: OPD (Outpatient Department)
Requested Service: Internal Medicine outpatient evaluation for persistent stomachache
Preferred Appointment Date/Time: Aug 11 or Wednesday, Aug 12; Morning / Afternoon
Referred By: N/A

Medical Justification
The patient reports a persistent stomachache requiring medical evaluation. Based on the clinical information provided, the patient's condition has been categorized as routine and is appropriately directed to Internal Medicine through the outpatient care pathway.

An Internal Medicine evaluation is requested to assess the patient's persistent stomachache and determine the appropriate clinical management. No secondary issue or additional clinical symptoms have been provided. Accordingly, this pre-authorization request is limited to the Internal Medicine OPD evaluation described above.

We kindly request authorization for the requested service under the patient's applicable insurance coverage.

Please review this request and advise regarding authorization.

Sincerely,
Medical Billing Officer
Multi-Specialty Diagnostics Clinic
```

**issues found:** no mention of tone and prevention of unnecessary details being added in the drafted letter. no mention of the draft actually requiring a human review and clinician sign off before submission.

## Version 2
```
You are a medical billing officer at a multi specialty diagnostics clinic, drafting a formal pre-authorization request to a patient's insurance provider. Draft a pre authorization request including the details mentioned below:

include: patient's name, policy id,requested service and a detailed medical justification based on the provided clinical details.

input:
{
  "Name": "sarah jones",
  "Contact Number": "5590995",
  "Email": null,
  "Date of Birth": "05/01/1987",
  "Primary issue": "Persistent stomachache",
  "Secondary issue": null,
  "Preferred date and time": "Aug 11 or Wednesday, Aug 12; Morning / Afternoon",
  "Referred by": null,
  "Insurance provider": insurance company limited,
  "Policy/Member ID": 559-009-087-12,
  "speciality": "Internal Medicine",
  "Non-medical department": null,
  "care-pathway": "OPD",
  "reasoning": "The persistent stomachache is appropriately routed to Internal Medicine based on the listed specialties, with no non-medical issue provided. The urgency is routine with high confidence, so the care pathway is OPD."
}

rules:
- do not invent policy numbers or details if not provided in the input. if any info is missing, state "N/A" instead of creating your own details.
- keep tone formal and professional, do not add unnecessary details not relevant to the insurance provider
- the draft requires human review and clinical sign off before submission
```

**output:**
```
**PRE-AUTHORIZATION REQUEST**

**Date:** 11 August 2026

**To:** Insurance Company Limited
**Subject:** Request for Pre-Authorization – Outpatient Internal Medicine Consultation

Dear Sir/Madam,

We are writing to request pre-authorization for an outpatient consultation for the following patient:

**Patient Name:** Sarah Jones
**Date of Birth:** 05/01/1987
**Policy/Member ID:** 559-009-087-12
**Contact Number:** 5590995

**Requested Service:**
Outpatient consultation with the Internal Medicine department.

**Medical Justification:**
The patient presents with a persistent stomachache requiring medical evaluation. Based on the clinical information provided, the patient has been appropriately referred to Internal Medicine for assessment. The case has been classified as routine, with no documented symptoms indicating an emergency or rapid deterioration.

We kindly request authorization for the above outpatient Internal Medicine consultation to facilitate appropriate clinical evaluation of the patient's presenting complaint.

Please review and provide the necessary pre-authorization for the requested service.

Sincerely,

**Medical Billing Officer**
**Multi-Specialty Diagnostics Clinic**
```

**changes:** the new rules explicitly state the clinical sign off rules and the formal and professional tone required to draft the pre authorization letter. 

## Automation potential
The drafting part itself is a highly automation friendly task. it reduces significant workload by drafting routine bills automatically without needing much manual intervention.However, the letter still needs to be reviewed and signed off by a clinician before submission to reduce any risk of misinformation or unnecessary details being presented to the insurance provider.

## Risks and limitations

- **hallucinated justification:** The AI could invent or exaggerate medical details not actually supported by the extracted symptoms to sound more compelling. This could count as insurance fraud if sent as it is, since the letter is making a formal claim to the payer.
- **fabricated patient/policy details:** if insurance info is missing in the input, the ai could make up plausible sounding details instead of flagging the gap .mitigated by explicit rules stated in the prompt itself to not make up information and flag gaps with null instead.
