# Prompt 4: Routing 

## Workflow
routes the patient into specific department based on the extracted information and the classified urgency

## Problems being solved
Patients needs to be routed into specific departments to get the relevant services. Medical departments are more critical than non-medical departments. Manual human review of every case can slow down cases which needs quick routing to medically essential departments. This automation can speed up the routing process significantly using the outputs of extraction and urgency classification step.

## Version 1

```
route the following input case into one of the mentioned departments:
1. Internal Medicine
2. Pediatrics
3. Obstetrics & Gynecology
4. General Surgery
5. Orthopedics
6. Cardiology
7. Dermatology
8. ENT
9. Neurology
10. Psychiatry

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
  "Insurance provider": null,
  "Policy/Member ID": null,
  "urgency": "routine",
  "confidence": "high",
  "reasoning": "The input reports a persistent stomachache requiring medical evaluation, but provides no symptoms suggesting an emergency or rapid deterioration. Therefore, based strictly on the provided information, this is standard care needed but not time-critical."
}

return:
speciality:
reasoning: (should be short, 2 to 3 sentence max )

rules:
- Return the output in a JSON format 
- Do not make up information if not provided. Simply fill the missing information with null 
- If multiple medical departments are involved, list them in order of severity mentioned 
```

**output:** 
```
{
"speciality": "Internal Medicine",
"reasoning": "The primary issue is a persistent stomachache in an adult patient. Based on the departments provided and without a specified surgical or other specialty-specific condition, Internal Medicine is the appropriate initial department."
}
```

**issues found:** There is no mention in the prompt on how to handle non medical departments which can lead  to complaints and claims piling up because they are simply routed nowhere. There is no mention of care pathway(opd or emergency) which can limit the ability to route patients efficiently .

## Version 2

```
route the following input case into one of the mentioned departments:
Medical Departments:
1. Internal Medicine
2. Pediatrics
3. Obstetrics & Gynecology
4. General Surgery
5. Orthopedics
6. Cardiology
7. Dermatology
8. ENT
9. Neurology
10. Psychiatry

Non medical Departments:
1. Administration
2. Human Resources
3. Finance & Accounting
4. Information Technology (IT)
5. Billing & Insurance

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
  "Insurance provider": null,
  "Policy/Member ID": null,
  "urgency": "routine",
  "confidence": "high",
  "reasoning": "The input reports a persistent stomachache requiring medical evaluation, but provides no symptoms suggesting an emergency or rapid deterioration. Therefore, based strictly on the provided information, this is standard care needed but not time-critical."
}

return:
speciality:
Non-medical department:
care-pathway:
reasoning: (should be short, 2 to 3 sentence max )

rules:
- categorize the care pathway into 2 class:
1. emergency: if urgency==urgent and confidence==high or confidence==medium. for low confidence but urgent cases . keep it in OPD but give a reason on why it could be urgent 
2. OPD: if urgency==routine or non urgent 
- Return the output in a JSON format 
- Do not make up information if not provided. Simply fill the missing information with null 
- If multiple medical departments are involved, list them in order of severity mentioned 
```

**output:**
```
{
"speciality": "Internal Medicine",
"Non-medical department": null,
"care-pathway": "OPD",
"reasoning": "The persistent stomachache is appropriately routed to Internal Medicine based on the listed specialties, with no non-medical issue provided. The urgency is routine with high confidence, so the care pathway is OPD."
}
```

**changes:** added care pathway so the cases can be routed to correct departments . Added non medical departments so that general issues dont get stuck in a deadlock.

## Automation Potential
This task has medium to high automation potential. Automating the process can significantly speed up the routing process even in a high volume scenario cutting up upto 80% manual work of categorizing and routing to correct departments. This helps manual reviewers focus more on urgent and critical cases while most routine cases get automated and routed through the pipeline. Human oversight is still crucial to not let false positives slip through and drain the center's resources and time . 

## Risks and limitation

- **False positives and false negatives:** non urgent cases could get routed into emergency slots delaying genuine urgent care while urgent cases could get slotted into a  opd slot .This could happen if the context and structure of the information provided is not handled carefully . Mitigated by manual human oversight over urgent cases by preventing false positives to pass through. 
- **Scale of the industry:** Medicine is a very broad field with a significant number of specialities and sub specialities. This could lead the ai to misclassify specialities if the provided info lacks proper medical details and symptoms or if the information is too complex and overlaps multiple specialities. 




