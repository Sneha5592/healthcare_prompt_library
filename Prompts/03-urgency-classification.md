# Prompt 3: urgency classification

## Workflow:
categorizes the given patient issue into urgent,routine or non-urgent category

## Problems being solved 
Urgent cases needs to be classified as soon as possible to be routed into relevant departments. Urgency classification helps to quickly classify urgent cases even in a high volume scenario. This reduces the need to manually classify each case which could potentially delay urgent cases. This automation helps urgent cases to be classified quickly and move through routing and necessary departments as soon as possible.

## Version 1

```
Classify the given input into one of the classes:
- urgent 
- routine
- non-urgent

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
"Policy/Member ID": null
}

return:
urgency:
confidence: (high/medium/low)
reasoning:

rules: 
- Return the output in a JSON format
- Do not make up information, strictly adhere to the input provided
- Classify urgency into exactly one of:
1. urgent: symptoms suggesting potential emergency or rapid deterioration
2. routine: standard care needed but not time-critical
3. non-urgent: general/administrative, no immediate medical concern
```

**output:**
```
{
"urgency": "routine",
"confidence": "high",
"reasoning": "The input reports a persistent stomachache requiring medical evaluation, but provides no symptoms suggesting an emergency or rapid deterioration. Therefore, based strictly on the provided information, this is standard care needed but not time-critical."
}
```

**issues found:** There is no rule to stop the model from trying to diagnose the patient.The model might attempt to diagnose the patient's condition or sound very confident while giving potentially harmful information.

## Version 2

```
Classify the given input into one of the classes:
- urgent 
- routine
- non-urgent

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
"Policy/Member ID": null
}

return:
urgency:
confidence: (high/medium/low)
reasoning:

rules: 
- Return the output in a JSON format
- Do not make up information, strictly adhere to the input provided
- Classify urgency into exactly one of:
1. urgent: symptoms suggesting potential emergency or rapid deterioration
2. routine: standard care needed but not time-critical
3. non-urgent: general/administrative, no immediate medical concern
- Do not attempt to diagnose the patient's condition. classify urgency based on described 
symptoms and stated severity only.
```

**changes:** added an explicit rule to prevent the ai from trying to diagnose the patients or give potentially harmful information confidently.

## Automation Potential:
This task has a medium to high automation potential. While the classification can be automated to a very high level, the nature of the classification itself requires human oversight to some extent. The oversight prevents misclassified cases to waste resources and potentially delay actually urgent cases. The automation output with a manual human oversight on urgent flagged cases can speed up routing of urgent cases helping save many lives.

## Risks and limitations

- **Misclassification:** The model can misclassify urgent cases as non urgent or non urgent cases as urgent if context and data are not properly structured. This can cause genuinely urgent cases to be delayed and slotted much later while routine cases get high priority. Mitigated by manual human oversight and anomaly detection .
- **Risk to resources**: Non urgent cases that were flagged as urgent can potentially put strain on hospital resources and staff putting urgent cases in a deadlock.





