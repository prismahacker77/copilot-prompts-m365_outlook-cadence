# Customer Meeting Cadence Analysis Prompt #PromptOfTheWeek

## Overview
This repository contains an enterprise‑grade prompt designed to analyze Microsoft 365 calendar data and accurately identify customer‑related meetings and their true cadences. The prompt treats calendar metadata as the sole source of truth and applies strict, rule‑based logic to determine whether a meeting is customer‑related, recurring, or one‑off, prioritizing accuracy and auditability over completeness.

## What This Prompt Does
The prompt extracts customer meetings using authoritative signals such as external email domains and explicit subject matches to canonical customer names, with optional alias support guarded by strict safeguards. It conservatively groups meetings by exact organizer and normalized subject, classifying recurrence only when supported by calendar flags or evenly spaced instances within a fixed analysis window. Cadence (weekly, bi‑weekly, monthly, quarterly) is assigned only when recurrence and interval consistency are clearly evidenced by calendar metadata.

## Design Principles
- Calendar metadata always wins over chat or email signals  
- External email domains outweigh display names or intuition  
- Weak aliases never qualify meetings without corroboration  
- Internal‑only meetings can be explicitly excluded  
- Repetition does not imply recurrence without evidence  
- Omission is preferred over misclassification  

## Output
The prompt produces a single, auditable table listing each qualifying meeting, its organizer, internal vs. external status, recurrence classification, and assigned cadence. A mandatory fact‑finding narrative accompanies the table, explaining—without referencing meeting content—why each meeting was included, how recurrence was determined, and why a cadence was or was not assigned.

## Intended Use
This prompt is designed for use with large language models analyzing Microsoft 365 calendar exports or tenant‑scoped calendar data. It is suitable for customer planning, account reviews, delivery cadence validation, and governance scenarios where false positives are more costly than omissions.

## Customer Configuration
Customers are defined explicitly using a structured configuration. Each customer **must** include a canonical name and one or more external email domains. Aliases are optional and are only applied when explicitly provided.

### Example Customer Configuration (rows 7-13 in customer-meeting-cadence-analysis.md prompt file)
CUSTOMERS = [
  {
    name: "Contoso",
    aliases: ["CSO"],
    external_domains: ["contoso.com", "contoso.io"]
  },
  {
    name: "Fabrikam Industries",
    aliases: [],
    external_domains: ["fabrikam.com"]
  }
]