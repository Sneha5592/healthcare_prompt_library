# Prompt 2: information extraction 

## Workflow:
Extracts important and relevant information from the given ticket in a JSON format 

## Problem being solved:
staffs currently have to extract relevant information from mostly unstructured raw text from the inquiries. This can cause them to miss crucial information . This automation ensures that every inquiries is scanned thoroughly to look for relevant information to extract from 

## version 1 :

```
Extract the following fields from the ticket below:
Name:
Age:
Primary issue:
Secondary issue:
preffered date and time:
referred by:

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

rules:
- return the the asked extraction categories in a JSON format
```

**output:**
```
{
"Name": "sarah jones",
"Age": 39,
"Primary issue": "Persistent stomachache",
"Secondary issue": "Possible referral for further diagnostic tests or a specialist",
"preffered date and time": "Aug 11 or Wednesday, Aug 12; Morning / Afternoon",
"referred by": null
}
```

**Issues found:** The rules around what should be treated as a primary or secondary issue not clear, limited fields to cover a significant amount of information.

## Version 2:

```
Extract the following fields from the ticket below. If a field is not 
mentioned in the ticket, set its value to null. Do not add or remove fields.

Name:
Contact Number:
Email:
Date of Birth:
Primary issue:
Secondary issue:
Preferred date and time:
Referred by:
Insurance provider:
Policy/Member ID:

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


rules:
- return the the asked extraction categories in a JSON format
- Treat the medical issue as the primary issue in every case, issues like complaints and claims should be included in the secondary issues field. 

```

**output:**
```
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

```

**changes:** added rules around primary and secondary issues, changed the output fields requirement to cover wide range of fields, if a given information is not present, the field is set to none. 

## Automation Potential
This task has a high automation potential because it allows the ai to scan for every inquiry thoroughly to pull relevant information . The information extracted can be much more structured and time saving compared to manual human extraction. It reduces the risk of missing information. While it has high automation potential in extracting information, it can struggle with contexts and understanding when to add or skip information requiring manual human oversight.

## Risks and limitations

**Missing contexts:** A ticket combining multiple issues without a clear context can cause every information even if irrelevant to be extracted . This causes the extracted data to be filled with a lot of irrelevant information which increases the risk of reviewers missing useful information.Mitigated by manual human oversight over extracted information
**Hallucination:**: The model can hallucinate and make up information causing the extracted data to be filled with fake information. This can be highly risky since a lot of future pipelines might depend on the extracted info provided by this task causing the entire chain to be bloated by fake information. 
**Field limitations:** Since there are multipled fields that can be created to fill niche informations, it could bloat the data structure causing important informations to compete with less important ones . Mitigated by simply directing them to secondary issues to be overlooked by the human reviewer.


