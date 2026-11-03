# Sources

Source material informs the canonical content but is not itself authoritative.
Where sources disagree, the disagreement is recorded as an open question in
[the working decisions appendix](../content/90-working-decisions.md) rather than
resolved silently.

Source documents are not copied into this repository. Content drawn from them
is rewritten, reviewed, and placed in `content/`. Nothing private is committed;
`sources/private/` is ignored by Git as a safeguard.

## Public sources

| Source | Date | Use |
|---|---|---|
| [InteropHub topic 36: Vaccine Exemptions and Deferrals](https://informatics.immregistries.org/hub/es/topic/36) | Current | Community coordination, meetings, and discussion notes |
| [HL7 Version 2.5.1 Implementation Guide for Immunization Messaging, Release 1.5, and addendum](https://www.cdc.gov/iis/technical-guidance/hl7.html) | Current | Message structures and existing query profiles |
| [LOINC answer list LL4353-0](https://loinc.org/LL4353-0) | Checked 2026-11-03 | Exemption type values |
| [LOINC 100282-3](https://loinc.org/100282-3) | Checked 2026-11-03 | Exemption certificate observation |
| [LOINC 30956-7](https://loinc.org/30956-7) | Checked 2026-11-03 | Vaccine type observation |
| [CDC CVX codes](https://www2.cdc.gov/vaccines/iis/iisstandards/vaccines.asp?rpt=cvx) | Current | Exempted vaccine category |
| [CDC CVX vaccine groups](https://www2.cdc.gov/vaccines/iis/iisstandards/vaccines.asp?rpt=vg) | Current | Vaccine-group codes for exempted categories |

## Working documents

Private implementation notes informed the query design. They are not listed
by title or published; their technical content is rewritten in `content/`.

| Document | Date | Use |
|---|---|---|
| Private implementation notes | Not published | Informed the query design |
