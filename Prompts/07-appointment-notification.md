# Prompt 7: Appointment notification

## Workflow
combines pre authorization state, routing and extracted info to create a detailed appointment schedule to be sent to the patient

## Problem being solved
Staff manually has to check insurance authorization status, extracted info from the patient details and routed specialities and draft a schedule combining all those fields. Automating the process lets the model handle the cases while drafting a professional schedule notification to be presented to the patient in a professional way. 

## Version 1 
```
You are a patient communication assistant at a multi-specialty diagnostics clinic writing a clear appointment  confirmation message directly to a patient. Draft an appointment message from the given input provided, After the formal appointment message, write a clear message explaining them what it means in plain human understandable language without making them more anxious:

input:
{
  "specialty": "Gastroenterology",
  "care_pathway": "OPD",
  "preauth_status": "approved",
  "appointment_datetime": "Aug 18, 10:00 AM"
}
{
"Name": "sarah jones",
"Contact Number": "5590995",
"Email": null,
"Date of Birth": "05/01/1987",
"Primary issue": "Persistent stomachache",
"Secondary issue": null,
"Preferred date and time": "Aug 11 or Wednesday, Aug 12; Morning / Afternoon",
"Referred by": null,
"Insurance provider": null,
"Policy/Member ID": null
}

rules: 
- Keep the tone very warm and professional 
- Do not make up information to sound compelling, Only refer to the input provided
- Only add date if explicitly provided in the input, if no date is given, fall back to "Waiting to be scheduled"
```

**output:**
```
Dear Sarah Jones,

Your appointment with the Gastroenterology specialty has been confirmed for August 18 at 10:00 AM.

Appointment details:
Specialty: Gastroenterology
Care pathway: OPD
Appointment: August 18, 10:00 AM
Pre-authorization status: Approved
Reason for visit: Persistent stomachache

We look forward to assisting you with your care.

What this means in simple terms:

Your appointment is confirmed with the Gastroenterology team, which focuses on concerns involving the digestive system. OPD means this is an outpatient appointment, so you are scheduled for a regular clinic visit.

Your pre-authorization has been approved, meaning the required authorization for this appointment has been approved.

The main thing to know is that your appointment is confirmed for August 18 at 10:00 AM.
```

**issues found:** The simple terms also keeps a very professional tone which could make patients still anxious. It should be made more genuine and warm


## Version 2
```
You are a patient communication assistant at a multi-specialty diagnostics clinic writing a clear appointment  confirmation message directly to a patient. Draft an appointment message from the given input provided, After the formal appointment message, write a clear message explaining them what it means in plain human understandable language without making them more anxious:

input:
{
  "specialty": "Gastroenterology",
  "care_pathway": "OPD",
  "preauth_status": "approved",
  "appointment_datetime": "Aug 18, 10:00 AM"
}
{
"Name": "sarah jones",
"Contact Number": "5590995",
"Email": null,
"Date of Birth": "05/01/1987",
"Primary issue": "Persistent stomachache",
"Secondary issue": null,
"Preferred date and time": "Aug 11 or Wednesday, Aug 12; Morning / Afternoon",
"Referred by": null,
"Insurance provider": null,
"Policy/Member ID": null
}

rules: 
- Keep the tone very warm and professional. The simpler terms should have a more genuine and warm tone trying to make the patient less anxious without making up details or undermining the actual cause
- Do not make up information to sound compelling, Only refer to the input provided
- Only add date if explicitly provided in the input, if no date is given, fall back to "Waiting to be scheduled"
```

**output:**
```
Dear Sarah Jones,

Your appointment with the Gastroenterology specialty has been confirmed for August 18 at 10:00 AM.

Appointment details:
Specialty: Gastroenterology
Care pathway: OPD
Appointment date and time: August 18, 10:00 AM
Pre-authorization status: Approved
Reason for visit: Persistent stomachache

We look forward to assisting you with your care.

What this means for you:

Your appointment is confirmed with the Gastroenterology team for your persistent stomachache. OPD simply means that this is a regular outpatient clinic visit.

Your pre-authorization status is approved, which means the required authorization for this appointment has been approved.

We understand that dealing with an ongoing stomachache and seeing medical terms in an appointment message can feel concerning. These details are simply explaining where your appointment has been arranged and its current status. Your appointment is confirmed for August 18 at 10:00 AM, and the Gastroenterology team will see you regarding your stomachache.
```

**changes:** Keeps the explanation more genuine and warm without trying to sound compelling or undermining the issue

## Automation potential
This task has a medium to high automation potential. While writing the appointment scheduling letter and message has high automation-potential, manual human oversight is still required to fix any misconfigured dates, made up information or missed information regarding their appointment.

## Risks and limitation

- **fabricated information:** While explicitly stating the model to be warm and genuine can make the patient feel less anxious, it can cause the model to undermine or ignore the primary issue to sound more compelling. This can be dangerous if patients starts expecting false things without proper diagnosis
- **Filling gaps:** The model could fill critical gaps like care_pathway or pre authorization status which can lead to both medical and legal issue if not reviewed in time manually. mitigated by explicitly telling the model to not fill gaps and fall back to necessary guardrails.