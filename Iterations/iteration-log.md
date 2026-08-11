# Iteration Log

## Prompt 01 – Intake Categorization
- v1 → v2: added missing category field to output, added rule that categorizes an outside field as 'uncategorized'
- v2 → v3: added rules for inputs containing multiple categories
- [See full detail →](https://github.com/Sneha5592/healthcare_prompt_library/blob/main/Prompts/01-intake-categorization.md)

## Prompt 02- Information Extraction 
- v1 → v2: added rules around primary and secondary issues, changed the output fields requirement to cover wide range of fields, if a given information is not present, the field is set to none.
- [See full detail →](https://github.com/Sneha5592/healthcare_prompt_library/blob/main/Prompts/02-extraction.md)

## Prompt 03- urgency classification
- v1 → v2: added explicit rule to prevent the model from diagnosing the patient or confidently giving potentially harmful medical information.
- [See full detail →](https://github.com/Sneha5592/healthcare_prompt_library/blob/main/Prompts/03-urgency-classification.md)

## Prompt 04- routing 
- v1 → v2: added care pathway so the cases can be routed to correct departments . Added non medical departments so that general issues dont get stuck in a deadlock.
- [See full detail →](https://github.com/Sneha5592/healthcare_prompt_library/blob/main/Prompts/04-routing.md)

## Prompt 05- Pre authorization 
- v1 → v2: the new rules explicitly state the clinical sign off rules and the formal and professional tone required to draft the pre authorization letter. 
- [See full detail →](https://github.com/Sneha5592/healthcare_prompt_library/blob/main/Prompts/05-pre-authorization.md)

## Prompt 06 – Anomaly detection
- v1 → v2: the model now outputs critical anomalies and gives a brief reasoning on why they were critical.
- v2 → v3: The rules around anomaly flagging were made explicit. this helps the model to be more consistent across broad cases. The routed specialities not matching the reason is given a separate anomaly class
- [See full detail →](https://github.com/Sneha5592/healthcare_prompt_library/blob/main/Prompts/06-anomaly-flagging.md)

## Prompt 07- appointment notification 
- v1 → v2:  Keeps the explanation more genuine and warm without trying to sound compelling or undermining the issue
- [See full detail →](https://github.com/Sneha5592/healthcare_prompt_library/blob/main/Prompts/07-appointment-notification.md)
