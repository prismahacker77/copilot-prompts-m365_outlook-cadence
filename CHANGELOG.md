## [2.1.0] - 2026-01-28

### Fixed
- **Critical Accuracy Fix:** Internal/external classification now evaluates ALL meeting participants (organizer AND attendees), not just the organizer. Prevents false "internal" classification when external customer attendees are present on invitation list.
- Added explicit attendee domain evaluation logic to INTERNAL vs EXTERNAL CLASSIFICATION section
- Enhanced fact-finding narrative requirements to document attendee domain evaluation

### Changed
- Updated OUTPUT section to clarify that internal/external classification examines organizer AND all attendee email domains against external_domains list
- Added inline example demonstrating organizer-attendee domain mismatch scenario (e.g., internal CSA organizing meeting with OneMain Financial attendees)
- Appended "Internal/external classification examines ALL participants, not just organizer" to GLOBAL NON-NEGOTIABLE RULES

### Example
- A meeting titled "OneMain Financial: Weekly Azure EDE Call Series" organized by an internal Cloud Solution Architect with OMF attendees (e.g., @onemainfinancial.com) is now correctly classified as **external** instead of internal.

---

## [2.0.0] - 2026-01-28

### Changed
- Converted multi-section markdown document into single, linear Copilot-executable prompt
- Removed all markdown formatting delimiters and section headers
- Consolidated 11 separate rule blocks into 5 integrated instruction sequences
- Reduced overall token count by ~35% while preserving all functional logic
- Moved editable parameters to prominent top position for immediate visibility
- Simplified parameter structure with clearer inline documentation
- Consolidated "weak alias," "internal-only," and "multi-customer collision" rules into unified customer identification logic
- Streamlined recurrence determination language to reduce ambiguity
- Made analysis window calculation more explicit with execution-date references

### Added
- Version header (Version 2.0) at top of prompt
- Explicit INTERNAL vs EXTERNAL CLASSIFICATION section
- Enhanced fact-finding narrative scope documentation

---

## [1.6.0] - 2026-01-27

### Initial Release
- Enterprise-grade customer meeting cadence analysis prompt
- Calendar metadata-driven classification
- Conservative recurrence determination logic
- Weak alias safeguards
- Organizer bias protection
- Multi-customer collision handling