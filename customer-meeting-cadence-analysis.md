CUSTOMER MEETING CADENCE ANALYSIS — Version 2.0 (M365 Copilot)

Extract customer-related meeting cadences from my Microsoft 365 calendar data with maximum accuracy and minimum false positives. Use calendar metadata as the sole source of truth. Recurrence requires evidence, not intuition. Organizer behavior is treated conservatively. Email domains outweigh display names. Omission is preferred over misclassification.

EDITABLE PARAMETERS (CUSTOMIZE THESE):

CUSTOMERS = [
  {
    name: "Customer",
    aliases: ["", ""],
    external_domains: ["domain.com"]
  }
]

INCLUDE_INTERNAL_ONLY_MEETINGS = false

OPTIONAL_VALIDATION_PEOPLE = {
  internal_names: [],
  external_names: []
}

ANALYSIS WINDOW (FIXED):
- START: Exactly 12 months before today's date
- END: Last day of current calendar year (December 31, 2026)
- Include ONLY meetings whose start date falls within this window
- For recurring series spanning outside the window, include ONLY instances inside the window

Example: If run on January 28, 2026, include meetings from January 28, 2025 through December 31, 2026.

DATA SOURCES (STRICT HIERARCHY):
1. Calendar meetings/events (sole source of truth for organizer identity, recurrence status, cadence)
2. Teams chats (corroboration only)
3. Email (corroboration only)

Chats and emails MUST NOT override calendar metadata.

CUSTOMER MEETING IDENTIFICATION:

A meeting is customer-related ONLY if it satisfies these rules:

PRIMARY IDENTIFIERS (AUTHORITATIVE):
- The organizer OR any attendee email domain matches ANY value in external_domains, OR
- The meeting subject contains the canonical customer name (case-insensitive whole-word match, no substrings)

SECONDARY IDENTIFIERS (ALIASES — OPTIONAL):
- Apply alias matching ONLY if aliases are explicitly provided and non-empty
- Use case-insensitive whole-word matching only (no fuzzy matching, stemming, similarity scoring, or NLP inference)

WEAK ALIAS SAFEGUARD (AUTHORITATIVE):
- Any alias ≤ 3 characters is a WEAK ALIAS
- A WEAK ALIAS match by itself is NEVER sufficient
- Weak alias matches REQUIRE corroboration from at least one: (a) organizer/attendee email domain match, OR (b) canonical customer name in subject
- Without corroboration, OMIT the meeting

INTERNAL-ONLY CONTROL (AUTHORITATIVE OVERRIDE):
- If INCLUDE_INTERNAL_ONLY_MEETINGS = false: Include ONLY meetings with at least one organizer or attendee whose email domain matches external_domains. This OVERRIDES all subject-only or alias-only matches.
- If INCLUDE_INTERNAL_ONLY_MEETINGS = true: Meetings may be included by canonical name or alias even if all participants are internal.

MULTI-CUSTOMER COLLISION RULE:
- If a meeting matches multiple customers by name or alias AND no external domain match disambiguates, OMIT the meeting entirely.

SUBJECT NORMALIZATION:

Normalize meeting subjects as follows:

REMOVE: "RE:", "FW:", "FWD:", noise tokens such as "(Internal)", "(External)", "(Updated)", and embedded dates in any format

PRESERVE: Canonical customer name tokens, aliases exactly as written, and functional anchors such as Sync, Cadence, Touchbase, Review

GUARDRAILS: Do NOT collapse aliases into canonical names. If no calendar recurrence flag exists and language implies ad-hoc or working-session behavior, classify as ONE-OFF.

GROUPING LOGIC (CANONICAL ROWS):

Group meetings ONLY when BOTH match exactly:
- Normalized subject
- Organizer email address

RULES: Organizer email must match exactly. One-off meetings MUST NEVER be merged into recurring series. Repeated ad-hoc meetings do NOT imply recurrence.

RECURRENCE DETERMINATION (HARDENED):

Classify meetings as "recurring" or "one-off" using ONLY allowed evidence:
- Calendar recurrence flag (preferred), OR
- At least two evenly spaced instances within the analysis window with no contradictory one-off indicators

DISALLOWED EVIDENCE: Subject naming alone, alias presence alone, chat or email cadence, human intuition

ORGANIZER SAFEGUARD: If the organizer is an internal Cloud Solution Architect AND no calendar recurrence flag exists AND meetings were manually created, classify as ONE-OFF even if dates appear similar. If uncertain, APPLY this safeguard.

CADENCE ASSIGNMENT:

Assign cadence ONLY when:
- The meeting is classified as recurring
- At least two instances exist in the window
- Interval consistency is supported by calendar metadata

ALLOWED CADENCE VALUES: weekly, bi-weekly, monthly, quarterly

If cadence cannot be confidently assigned, use "—".

OUTPUT (REQUIRED):

Produce ONE table with EXACT columns:
| meeting | organizer | internal/external | recurring vs one-off | cadence |

RULES:
- organizer = calendar organizer display name
- internal/external = derived strictly from organizer email domain
- recurring vs one-off = literal values only
- cadence = allowed cadence or "—"

FACT-FINDING NARRATIVE (MANDATORY):

Provide one short paragraph explaining:
- How organizer identity was determined (calendar organizer field)
- How internal/external status was derived (email domain)
- Why meetings were recurring or one-off (allowed evidence only)
- Why cadence was or was not assigned

Do NOT describe meeting content.

GLOBAL NON-NEGOTIABLE RULES:
- Do NOT infer meeting purpose
- Do NOT infer cadence from a single instance
- Calendar metadata always wins
- Aliases widen inclusion, never classification
- Weak aliases require corroboration
- Prefer omission over misclassification

Execute this analysis now using my M365 calendar data within the specified analysis window.