# Concepts and Use Cases

## Concepts

An IIS can hold several kinds of statement about a vaccine that was not given.
In HL7 v2 they share the non-administration form, but they mean different
things and must not be used to carry one another.

| Concept | Working definition | Affects evaluation and forecast | Developed here |
|---|---|---|---|
| Formal exemption | An official, administrative status that excuses a person from a vaccination requirement under jurisdictional law or policy. It applies to one or more vaccine categories, has a type (such as medical, religious, or philosophical), and has a filing date. | No | Yes |
| Refusal | A patient's or parent's decision to decline a vaccine, usually at a point of care. Not an official status; a person may refuse without holding an exemption. | Not by this topic | No; existing guidance applies |
| Contraindication or precaution | A clinical assessment that a vaccine is inadvisable or calls for caution. | May | No; existing guidance applies |
| Evidence of immunity | A history of disease or laboratory result showing a vaccine is not needed. | May | No; existing guidance applies |
| Deferral | Not yet defined. See [Deferrals](#deferrals-frame). | Not yet defined | Questions only |

## Terminology cautions

- A **medical exemption** is a formal exemption with a medical basis. It is
  still an administrative status, separate from any clinical contraindication
  record, even when the same condition underlies both. Its LOINC answer is
  labeled "Medical contraindication"; prose and displays should say "medical
  exemption."
- For personal-belief exemptions, "Philosophical objection" is preferred over
  "Parent/patient refusal" so the exemption is not read as a refusal.
- Jurisdictions use different names for the same exemption types, so display
  names should be agreed separately from codes.

## Deferrals {#deferrals-frame}

"Deferrals" are in this topic's name, but no one has yet said what a deferral
is or which use cases need one exchanged. This document does not fit deferrals
into the exemption structure until these are answered (OQ-1):

- Is a deferral a temporary clinical delay, an administrative delay (such as a
  grace period for school entry), a forecast recommendation, or something else?
- Does it apply to a vaccine category, a dose, a series, or a period of time?
- What creates, updates, ends, or supersedes one?
- How does it differ from a contraindication, precaution, refusal, or forecast
  status?
- Which use cases need an IIS to receive a deferral, return one, or both?

## Actors

| Actor | Role in the exchange |
|---|---|
| IIS | Holds the current exemption state and returns or receives it. |
| School or childcare system | Checks whether a student lacking required vaccines holds an exemption. |
| Provider system (EHR or pharmacy) | May query for exemptions or submit them. |
| Jurisdiction exemption program | Issues or records exemptions through its own process, outside the exchange. |
| Other systems | Any other system that exchanges exemptions, such as a state education agency. |

Parents and guardians claim exemptions but do not take part in the HL7
exchange.

## Use cases

| ID | Use case | Exchange | Status |
|---|---|---|---|
| UC-1 | A school asks the IIS only for a student's exemptions. | QBP/RSP | Implemented in at least one IIS |
| UC-2 | A school asks for immunization history and exemptions together. | QBP/RSP | Implemented in at least one IIS |
| UC-3 | A school asks for evaluated history and forecast with exemptions. | QBP/RSP | Implemented in at least one IIS |
| UC-4 | A provider asks for exemptions. | QBP/RSP | Same pattern |
| UC-5 | A system submits a new or changed exemption to the IIS. | VXU | Not yet designed; see [Unsolicited updates](#unsolicited-updates) |
| UC-6 | A system exchanges a deferral. | Not determined | Not defined (OQ-1) |

The first use cases are kindergarten through grade 12 school requirements,
where a school reviewing compliance needs to know whether a
student holds an exemption, its type, and the vaccine categories it covers.
