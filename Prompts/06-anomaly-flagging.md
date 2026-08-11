# Prompt 6: Anomaly flagging

## Workflow:
Compares classified urgency with the routing details to flag false positives and false negatives that has slipped through the system 

## Problem being solved:
Staff often have to manually dig through records to check any false positives that might have slipped through. This takes up a lot of time and resources and is much less efficient. This automation automatically compares the urgency and routing of cases to flag anomalies that have passed through the system

## Version 1 

```
Compare the urgency classified with the routing details to flag any false positives or false negatives that has passed through the system. Also detect anomalies like speciality not matching the reasoning.

input:

{
  "case_1": {
    "urgency": "routine",
    "confidence": "high",
    "speciality": "Internal Medicine",
    "Non-medical department": null,
    "care-pathway": "OPD",
    "reasoning": "The input reports a persistent stomachache requiring medical evaluation, but provides no symptoms suggesting rapid deterioration. Internal Medicine is appropriate, and routine OPD evaluation is suitable."
  },

  "case_2": {
    "urgency": "routine",
    "confidence": "high",
    "speciality": "Internal Medicine",
    "Non-medical department": null,
    "care-pathway": "Emergency",
    "reasoning": "The input reports a persistent stomachache without symptoms suggesting rapid deterioration. Internal Medicine is appropriate and the urgency is routine."
  },

  "case_3": {
    "urgency": "routine",
    "confidence": "high",
    "speciality": "Cardiology",
    "Non-medical department": null,
    "care-pathway": "OPD",
    "reasoning": "The input reports a persistent stomachache requiring routine medical evaluation. There are no symptoms suggesting an urgent presentation, and OPD evaluation is appropriate."
  },

  "case_4": {
    "urgency": "urgent",
    "confidence": "high",
    "speciality": "Internal Medicine",
    "Non-medical department": null,
    "care-pathway": "OPD",
    "reasoning": "The input reports a persistent stomachache without symptoms suggesting rapid deterioration. Internal Medicine and OPD are appropriate for routine evaluation."
  }
}

return:
Total flagged anomalies:

rules: 
Return the following in a JSON format
Flag anomalies based on given rules only
```
**output:**
```
{
"Total flagged anomalies": 3
}
```

**issues:** Only outputting the number of flagged anomalies doesnt give much insights, No reasoning field was provided for a detailed review about the anomaly.


## Version 2

```
Compare the urgency classified with the routing details to flag any false positives or false negatives that has passed through the system. Also detect anomalies like speciality not matching the reasoning.

input:

{
  "case_1": {
    "urgency": "routine",
    "confidence": "high",
    "speciality": "Internal Medicine",
    "Non-medical department": null,
    "care-pathway": "OPD",
    "reasoning": "The input reports a persistent stomachache requiring medical evaluation, but provides no symptoms suggesting rapid deterioration. Internal Medicine is appropriate, and routine OPD evaluation is suitable."
  },

  "case_2": {
    "urgency": "routine",
    "confidence": "high",
    "speciality": "Internal Medicine",
    "Non-medical department": null,
    "care-pathway": "Emergency",
    "reasoning": "The input reports a persistent stomachache without symptoms suggesting rapid deterioration. Internal Medicine is appropriate and the urgency is routine."
  },

  "case_3": {
    "urgency": "routine",
    "confidence": "high",
    "speciality": "Cardiology",
    "Non-medical department": null,
    "care-pathway": "OPD",
    "reasoning": "The input reports a persistent stomachache requiring routine medical evaluation. There are no symptoms suggesting an urgent presentation, and OPD evaluation is appropriate."
  },

  "case_4": {
    "urgency": "urgent",
    "confidence": "high",
    "speciality": "Internal Medicine",
    "Non-medical department": null,
    "care-pathway": "OPD",
    "reasoning": "The input reports a persistent stomachache without symptoms suggesting rapid deterioration. Internal Medicine and OPD are appropriate for routine evaluation."
  }
}

return:
Total flagged anomalies:
Critical anomalies flagged:
Reasoning:( 2 to 3 sentences max)

rules: 
Return the following in a JSON format
Flag anomalies based on given rules only
```

**output:** 
```
{
"Total flagged anomalies": 3,
"Critical anomalies flagged": [
"case_2",
"case_3",
"case_4"
],
"Reasoning": "Case 2 has a routine urgency but an Emergency care pathway. Case 3 routes a persistent stomachache to Cardiology without reasoning supporting that speciality, while Case 4 classifies the case as urgent despite reasoning supporting routine OPD evaluation."
}
```

**changes:** the model now outputs critical anomalies and gives a brief reasoning on why they were critical.

**issues:** there is no explicit rule for flagging critical anomalies, causing the model to generate its own internal rule which can be inconsistent across broad cases

## Version 3

```
Compare the urgency classified with the routing details to flag any false positives or false negatives that has passed through the system. Also detect anomalies like speciality not matching the reasoning.

input:

{
  "case_1": {
    "urgency": "routine",
    "confidence": "high",
    "speciality": "Internal Medicine",
    "Non-medical department": null,
    "care-pathway": "OPD",
    "reasoning": "The input reports a persistent stomachache requiring medical evaluation, but provides no symptoms suggesting rapid deterioration. Internal Medicine is appropriate, and routine OPD evaluation is suitable."
  },

  "case_2": {
    "urgency": "routine",
    "confidence": "high",
    "speciality": "Internal Medicine",
    "Non-medical department": null,
    "care-pathway": "Emergency",
    "reasoning": "The input reports a persistent stomachache without symptoms suggesting rapid deterioration. Internal Medicine is appropriate and the urgency is routine."
  },

  "case_3": {
    "urgency": "routine",
    "confidence": "high",
    "speciality": "Cardiology",
    "Non-medical department": null,
    "care-pathway": "OPD",
    "reasoning": "The input reports a persistent stomachache requiring routine medical evaluation. There are no symptoms suggesting an urgent presentation, and OPD evaluation is appropriate."
  },

  "case_4": {
    "urgency": "urgent",
    "confidence": "high",
    "speciality": "Internal Medicine",
    "Non-medical department": null,
    "care-pathway": "OPD",
    "reasoning": "The input reports a persistent stomachache without symptoms suggesting rapid deterioration. Internal Medicine and OPD are appropriate for routine evaluation."
  }
}

return:
Total flagged anomalies:
Critical anomalies flagged:
speciality anomaly:
Reasoning:( 2 to 3 sentences max)

rules: 
Return the following in a JSON format
Flag anomalies based on given rules only:
1. If urgency==urgent but care_pathway!=emergency, flag it as critical anomaly
2. If urgency==routine or non urgent but care_pathway==emergency, flag it as critical anomaly
3. if specialty does not match the reasoning, flag it as speciality anomaly
4. if multiple anomaly overlap, flag it in both scenarios 
```

**output:**
```
{
"Total flagged anomalies": 3,
"Critical anomalies flagged": [
"case_2",
"case_4"
],
"speciality anomaly": [
"case_3"
],
"Reasoning": "Case 2 is critical because routine urgency is paired with an Emergency pathway, while case 4 is critical because urgent urgency is paired with OPD. Case 3 is a speciality anomaly because Cardiology is not supported by the reasoning, which only describes a persistent stomachache and routine OPD evaluation."
}
```

**changes:** The rules around anomaly flagging were made explicit. this helps the model to be more consistent across broad cases. The routed specialities not matching the reason is given a separate anomaly class

## Automation Potential
This task has a very high automation potential because anomaly flagging can be hard to perform manually without dedicated systems. Automating the anomaly flagging pipeline helps reviewers identify critical anomalies quickly which would have been much slower manually otherwise.The critically flagged anomalies can be quickly fixed or reported which helps to reduce the strain on the clinic's resources . This task however still needs a human reviewer to actually review the critical anomalies and escalate it further if required.

## Risks and limitations
- **False negatives:** This is a high risk scenario if a false negative slips through and much dangerous if staffs have no idea how to double check it . This is riskier than a false positive slipping through the system. THis can cause the staff to develop fake confidence that "if it wasn't flagged, it is fine"
- **false positives:** Less dangerous than a false negative but still risky if staffs start ignoring flags altogether
- **only catches defined mismatch:** the anomaly detection only catches 2 mismatch types for now. This could limit the detection specially if the anomaly if outside of those explicitly defined rules. Precision was chosen over broad coverage for this task
**Upstream dependency:** Since the anomaly detection depends on urgency and routing outputs, any error in those data itself will cause the anomaly detection to fail too causing false positives or false negatives to pass through.

