# Prompt 1: intake categorization

## Workflow:
sorts incoming clinic requests into 4 different categories before routing

## Problem being solved:
staff currently manually read and sort every incoming ticket, causing delays. It also tries to reduce risk of missing information which can be caused by human errors like lack of focus or high volume of requests. 

## version 1

```
You are an administrative intake co-ordinator at a multispeciality diagnostic clinic. You will categorize the following ticket into one of the 4 categories given:
1. Referral to a doctor
2. Insurance claims
3. General complaint
4. Technical issues

input:
Subject: Appointment Request – General Practitioner (Stomachache Consultation & Referral)

Reason for Visit: I have been experiencing persistent stomachache for the past [e.g., 3 days / week] and would like to schedule a consultation with a General Practitioner.

Goal: I am seeking a GP evaluation to help diagnose the issue and, if necessary, get a referral for further diagnostic tests or a specialist within the center.

Preferred Appointment Window:
    Preferred Date(s): [ Aug 11 or Wednesday, Aug 12]
    Preferred Time: [e.g., Morning / Afternoon]

Patient Details:
    Name: sarah jones
    Phone: 5590995
    Date of Birth: 05/01/1987

Please let me know the available time slots. Thank you!

return:

Category:
inquirer's name:
inquirer's id: (if provided)
inquirer's number:

rules:
- return the following in a JSON format
```

**output:** 
```
{
"Category": "Referral to a doctor",
"inquirer's name": "sarah jones",
"inquirer's number: "5590995"

}
```


**Issues found:** missing category in output, no rules for requests that doesn't fall in the mentioned categories,Trying to extract info that is actually a separate step in the workflow

## version 2

```
You are an administrative intake co-ordinator at a multispeciality diagnostic clinic. You will categorize the following ticket into one of the 4 categories given:
1. Referral to a doctor
2. Insurance claims
3. General complaint
4. Technical issues

input:
Subject: Stomachache Consultation & Insurance Inquiry

Reason:
I have been experiencing a stomachache and would like to consult with an appropriate doctor or specialist. I would also like to understand whether diagnostic tests may be necessary.

Goal:
I would like assistance with a doctor referral and scheduling a consultation. I also want to confirm whether my insurance is accepted and understand the claim process and required documents.

Detail:
Name: michelle jones
Contact Number: 591223400
Email: mj@gmail.com
Insurance Provider: insurance limited
Policy/Member ID: 56-2232-00
Preferred Appointment Date: 01/04/2026

return:

Category:

rules:
- Return the following in a JSON format
- If the request does not fall into one of those categories, categorize it as 'uncategorized' and a small reason for it 
```

**output(v2):** 
```
{
"Category": "Referral to a doctor"
}
```

**changes:** added a rule where requests that are out of the mentioned categories gets categorized into 'uncategorized'

**Issues found:** Tries to output only one category even if more than one category is given.

## version 3

```
You are an administrative intake co-ordinator at a multispeciality diagnostic clinic. You will categorize the following ticket into one of the 4 categories given:
1. Referral to a doctor
2. Insurance claims
3. General complaint
4. Technical issues

input:
Subject: Stomachache Consultation & Insurance Inquiry

Reason:
I have been experiencing a stomachache and would like to consult with an appropriate doctor or specialist. I would also like to understand whether diagnostic tests may be necessary.

Goal:
I would like assistance with a doctor referral and scheduling a consultation. I also want to confirm whether my insurance is accepted and understand the claim process and required documents.

Detail:
Name: michelle jones
Contact Number: 591223400
Email: mj@gmail.com
Insurance Provider: insurance limited
Policy/Member ID: 56-2232-00
Preferred Appointment Date: 01/04/2026

return:

Category:

rules:
- Return the following in a JSON format
- If the request does not fall into one of those categories, categorize it as 'uncategorized' and a small reason for it 
- If more than one category is given and all of it falls under the mentioned categories, mention them all.
- If one falls into the mentioned category while others dont, simply categorize into the available category only while providing a small reason for the uncategorization 
```

**output:** 
```
{
"Category": [
"Referral to a doctor",
"Insurance claims"
]
}
```
**changes:** added rules where multiple categories can efficiently get categorized 

## Automation Potential
This task has  high automation potential because it is simply categorizing the incoming tickets into known  common categories . Most of the categorization task can be run without a human. While most of the categories are common and categorizable , tickets that combine multiple categories, vague wordings or confused inquiries will still need humans to manually check and categorize it.It eliminates the need for front desk staff to manually sort every incoming ticket,a task currently repeated dozens of times daily.

## Risks and Limitations

- **Misclassification of ambiguous requests** : A ticket combining multiple categories could be forced into a single category or be categorized as uncategorized, losing information. This requires a manual human review to categorize them correctly.
- **Over-reliance risk**: Staffs may stop double checking outputs over time. Letting errors and misclassifications slip through .
















