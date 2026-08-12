# Prompt 10: Weekly summary

## Workflow
Creates a brief weekly operations summary from raw JSON data to give insights about the weekly performances and anomalies.

## Problem being solved
Staffs spend significant chunk of time creating weekly summaries from raw data. It can become a big workload for the staff to manually summarize the weeks performance in a brief and understandable way. This task helps to summarize the weekly operations from raw data in a professional and human-readable way , significantly cutting manual human workload and time consumption.

## Prompt:
```
role: You are an operations analyst preparing a concise weekly performance summary for the clinic manager, based on the aggregated statistics below. Highlight notable trends, any anomalies flagged, and areas that may need managerial attention

input:
{
  "week_of": "Aug 4-10",
  "total_tickets": 87,
  "category_breakdown": {
    "appointment_request": 45,
    "complaint": 12,
    "insurance_claim": 20,
    "technical_issue": 10
  },
  "urgency_breakdown": {
    "urgent": 8,
    "routine": 60,
    "non-urgent": 19
  },
  "anomalies_flagged": 3,
  "anomaly_types": {"under-triage": 2, "over-triage": 1},
  "avg_time_to_routing_hours": 4.2
}

rules:
- The paragraph should be well structured and human readable
- Do not make up information not provided in the input
- Give proper insights about what the weekly summary represents
```

**output:**
```
For the week of Aug 4–10, the clinic processed 87 tickets. Appointment requests represented the largest share with 45 tickets (51.7%), followed by insurance claims with 20 (23.0%), complaints with 12 (13.8%), and technical issues with 10 (11.5%). Most cases were classified as routine, accounting for 60 tickets (69.0%), while 19 (21.8%) were non-urgent and 8 (9.2%) were urgent. This indicates that the majority of the week’s workload consisted of standard, non-time-critical cases, with appointment-related demand forming the largest component.

Three anomalies were flagged during the week: two under-triage cases and one over-triage case. The under-triage cases warrant particular managerial attention because they involved cases assessed at a lower urgency than appropriate. The average time to routing was 4.2 hours; however, without a defined target or previous-week comparison, it is not possible to determine whether this represents satisfactory performance or a delay. Overall, management attention should focus on reviewing the triage anomalies, particularly under-triage, while continuing to monitor appointment demand and routing time against appropriate operational benchmarks.
```

## Automation potential
This task has a very high potential because of the summarization capabilites of the model . It is highly demanding for the staff to calculate specific percentages and present it in a brief and human readable manner. The automation can do this fairly quickly without needing much human oversight.

## Risk and limitations
**Fabricated information:** The model could make up numbers that does not clearly represent the input. mitigated by mentioning rules preven*ting any information fabrication
**Over professional:**The model might try to be overly professional with clinical words and details which can kill the original purpose of a brief weekly summary. 
