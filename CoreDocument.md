<!--
GENERATED FILE. DO NOT EDIT DIRECTLY.

The authoritative source is in content/. Regenerate this reading copy with:
    python process/tools/build_core_markdown.py --assembly VaccineExemptionsAndDeferrals.qmd
-->

# Vaccine Exemptions and Deferrals

*A Topic Reference for community discussion*

> This is a generated reading copy for GitHub. For review and editing, use the
> [canonical section files](content/README.md).

---

# Status and Scope

> **Status:** Working draft for community discussion. Nothing in this document
> is a published standard, an HL7 ballot, or a final decision. Working
> decisions are recorded so the community can see, question, and improve them,
> and any of them may change.

## In brief

Many jurisdictions let a parent or guardian claim a formal exemption from
vaccines that a school or childcare program requires. Immunization information
systems (IIS) increasingly hold those exemptions, and schools, providers, and other systems want
to exchange them through the HL7 v2 interfaces they already use. National HL7 v2
immunization guidance does not say how, and does not distinguish an exemption
from a refusal or a clinical contraindication.

This document sets out a technical template for communicating exemptions,
parts of which are implemented in at least one IIS, and frames the rest of the
topic: submitting exemptions in unsolicited updates (VXU), defining deferrals,
and moving toward formal standards.

## What this project covers

- Definitions that separate exemptions, deferrals, refusals, contraindications,
  and other reasons a vaccine is not given.
- The use cases and actors that need exemption or deferral information
  exchanged.
- A transport-independent information model for exemption state.
- A query and response pattern for exemptions.
- The questions a VXU design must answer, and a path to formal standards.

## What this project does not cover

This document defines how exemption information is communicated. It does not
define:

- who qualifies for an exemption, which exemption types a jurisdiction
  recognizes, or which vaccines a school requires;
- how an exemption is collected, validated, deduplicated, expired, or revoked;
- who may send or receive exemption information, or how exchanged information
  is used, disclosed, or protected, which are matters of IIS and jurisdictional
  policy; or
- any effect on immunization evaluation or forecasting, which belongs to a
  separate clinical decision support proposal.

A jurisdiction's exemption process can therefore change without changing the
message design, as long as its exemption types map to the shared vocabulary.
This document is not an HL7 implementation guide or conformance profile.

## Related work

| Work | Relationship to this work |
|---|---|
| [HL7 Version 2.5.1 Implementation Guide for Immunization Messaging, Release 1.5, and addendum](https://www.cdc.gov/iis/technical-guidance/hl7.html) | Defines VXU, QBP, RSP, and the existing query profiles (`Z34`, `Z44`, `Z32`, `Z42`) the exemption pattern extends. |
| LOINC and CDC CVX codes | Supply the exemption vocabulary. See [Vocabulary](#vocabulary). |
| Clinical decision support for immunizations (CDSi) | Evaluation and forecasting, which exemptions do not change. |

---

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

---

# Information Model

This chapter describes exemption information independently of any message.
[Query and Response](#query-and-response) shows how it is carried in HL7 v2;
a VXU or FHIR design should carry the same information.

## Structure

A person has zero or more **exemption items**. Each item states that one
vaccine category is exempted, with a type and a filing date. One exemption form
usually produces several items. The type may differ from item to item, even
where a jurisdiction's own process applies one type to every vaccine selected.

A response may instead state that **no exemptions are documented** as of a
date. That statement is itself exemption information.

## Data elements

| Element | Meaning | Status |
|---|---|---|
| Exemption type | The basis of the exemption | Required |
| Exempted vaccine category | The vaccine or vaccine-preventable disease covered | Required |
| Date filed | When the exemption was filed with the jurisdiction | Required |
| As-of date | When a "no exemptions documented" statement is true | Required for that statement |
| Who completed or issued the exemption | The person or organization behind it | Not required; optional if a need is shown (OQ-2) |
| Status, expiration, revocation, school year | Whether and when the exemption is in force | Not required for a snapshot (OQ-2) |
| Exemption identifier | Stable identifier for updating or revoking an item | Not needed for a snapshot; open for VXU (OQ-12) |
| Source and provenance | Which system or program asserted it | Open for VXU (OQ-14) |

Every element should have an operational reason to exist. Elements were left
out of the query design because no consumer needed them, not because they were
judged unimportant.

## Current-state semantics

A query response is a **snapshot** of the IIS's current exemption state, with
no history or lifecycle events. An exemption that has ended is simply absent
from later responses, and consumers query again to see changes. The IIS manages
expiration and revocation in its own process, for example through grade-based
rules or later vaccination. Unsolicited updates need more; see
[Unsolicited updates](#unsolicited-updates).

## Vocabulary

**Exemption type** uses answers from [LL4353-0](https://loinc.org/LL4353-0),
the example answer list for [LOINC 100282-3](https://loinc.org/100282-3). The
list covers reasons a vaccine was not given generally; only these are used:

| Code | LOINC display | Use |
|---|---|---|
| `LA27164-5` | Medical contraindication | Medical exemption |
| `LA27171-0` | Religious exemption | Religious exemption |
| `LA27170-2` | Philosophical objection | Philosophical or personal-belief exemption |
| `LA137-2` | None | No exemptions documented; not in `LL4353-0` |

`LA27167-8` (Parent/Patient refusal) is in the list but is not used for
exemptions. Whether these LOINC codes are the best fit is open (OQ-4).

**Exempted vaccine category** uses [CVX](https://www2.cdc.gov/vaccines/iis/iisstandards/vaccines.asp?rpt=cvx),
preferring "unspecified formulation" codes or the code that represents a
[CDC vaccine group](https://www2.cdc.gov/vaccines/iis/iisstandards/vaccines.asp?rpt=vg),
because an exemption covers a category, not a product. CVX is not a
disease-group vocabulary: MMR and varicella, for example, have no "unspecified"
code. A disease-group vocabulary could replace this later without changing the
rest of the model (OQ-3).

---

# Query and Response

This chapter sets out the QBP/RSP pattern for exemptions. Parts of it are
implemented in at least one IIS. Profile identifiers shown here are local
choices, not national assignments.

## Requested explicitly

Exemption content is returned only when the query explicitly asks for it.
Whether the IIS returns it to a particular requester is IIS policy, outside
this document; the exchange only signals that exemptions were requested and
whether they are included (OQ-8).

Existing querying systems do not expect exemptions, and most people have none,
so adding exemption content (or "none" statements) to every response would add
size and processing for no benefit. Returning exemptions based only on the
requester's settings in the IIS was considered and set aside, so that a
querying system always knows exemptions may be present.

## Requesting exemptions

The request adds an exemption profile in repeating `MSH-21`, alone or with an
existing query profile:

| `MSH-21` in the query | Meaning | Valid |
|---|---|---|
| `Z34` | Immunization history | Yes, existing |
| `Z44` | Evaluated history and forecast | Yes, existing |
| Exemption profile alone | Exemptions only | Yes |
| `Z34` and exemption profile | History plus exemptions | Yes |
| `Z44` and exemption profile | Evaluated history and forecast plus exemptions | Yes |
| `Z34` and `Z44` | Conflicting intents | No; return an error with `ERR` |

Order of the repeated values carries no meaning. Only listed combinations are
valid (OQ-7). `Z34` and `Z44` keep their current meanings.

At least one implementation uses `Z99^CDCPHINVS`, assigned locally, as its
request profile.
A complete design needs a pair: a query profile meaning "exemption content is
requested" and a response profile meaning "exemption content is included," so
the response's `MSH-21` repeats as well (for example `Z42` with the exemption
response profile). No national pair has been agreed (OQ-5). Whether `QPD-1`
also carries the combined intent is open (OQ-6).

Example: evaluated history and forecast plus exemptions, with `QPD-1` carrying
the primary intent only. Identifiers and demographics are illustrative.

```hl7
MSH|^~\&|SCHOOLSYS|SCHOOL01|IIS|IIS01|20260115093000-0700||QBP^Q11^QBP_Q11|Q0001|T|2.5.1|||ER|AL|||||Z44^CDCPHINVS~Z99^CDCPHINVS
QPD|Z44^Request Evaluated History and Forecast^CDCPHINVS|Q0001|STU1001^^^SCHOOL01^MR|Example^Pat^^^^^L||20190601|F
RCP|I|1^RD&records&HL70126
```

## Response order group

Exemptions go in one dedicated `ORC/RXA[/RXR]/OBX...` order group, however
many vaccine categories are exempted, as long as all items share the same
administrative context. In an evaluated-history response it sits with the
other non-administration groups, after immunization history and before
forecast.

The `ORC` and `RXA` follow the pattern already used for contraindications and
history of disease:

| Field | Value | Note |
|---|---|---|
| `ORC-3` | `9999^IIS` | Established placeholder |
| `RXA-3` | Date the message was built | Required by structure; no exemption meaning |
| `RXA-5` | `998^No Vaccination Administered^CVX` | |
| `RXA-20` | `NA` | Not administered |

`RXR` may appear where the structure calls for it and adds no meaning. `NK1`
may be returned under the underlying query profile to help confirm a match.
`NTE` is optional; omit it without a defined need.

## Paired observations

Each exemption item is two `OBX` segments sharing an `OBX-4` sub-ID:

| Field | Exemption type observation | Vaccine category observation |
|---|---|---|
| `OBX-2` | `CE` | `CE` |
| `OBX-3` | `100282-3^Vaccine exemption certificate^LN` ([LOINC](https://loinc.org/100282-3)) | `30956-7^Vaccine type^LN` ([LOINC](https://loinc.org/30956-7)) |
| `OBX-4` | Pair identifier | Same pair identifier |
| `OBX-5` | Exemption type ([vocabulary](#vocabulary)) | Vaccine category ([vocabulary](#vocabulary)) |
| `OBX-11` | `F` | `F` |
| `OBX-14` | Date filed | Not used |

Example: two religious exemption items, for hepatitis A and DTaP. In a complete
response, `OBX-1` continues the numbering of earlier groups.

```hl7
ORC|RE||9999^IIS
RXA|0|1|20260115||998^No Vaccination Administered^CVX|999||||||||||||||NA
OBX|1|CE|100282-3^Vaccine exemption certificate^LN|1|LA27171-0^Religious exemption^LN||||||F|||20250901
OBX|2|CE|30956-7^Vaccine type^LN|1|85^HepA Unspecified^CVX||||||F
OBX|3|CE|100282-3^Vaccine exemption certificate^LN|2|LA27171-0^Religious exemption^LN||||||F|||20250901
OBX|4|CE|30956-7^Vaccine type^LN|2|107^DTaP Unspecified^CVX||||||F
```

## No exemptions documented

When exemptions were requested and are being returned but none are on file,
the IIS
may send the order group with a single exemption-type observation whose value
is `LA137-2^None^LN` and whose `OBX-14` is the response date. The `ORC` and
`RXA` are still required. When exemption content is not being returned, send no
exemption group at all.

```hl7
OBX|1|CE|100282-3^Vaccine exemption certificate^LN|1|LA137-2^None^LN||||||F|||20260115
```

---

# Path Forward

## Unsolicited updates

Exemptions could also be submitted to an IIS in VXU messages. A VXU should
carry the same payload as a response: the
[order group](#response-order-group) and [paired observations](#paired-observations).

The query mechanism does not transfer. A response is a snapshot the IIS builds
from its own data; a VXU tells a receiver about something it may already hold.
A VXU design must therefore decide:

- whether a message reports one event or the sender's complete current set
  (OQ-11);
- what identifies an exemption so it can be replaced, revoked, or deleted
  (OQ-12);
- whether a sender may assert "none," which could erase exemptions from another
  source (OQ-13); and
- provenance and acknowledgement behavior (OQ-14).

## Candidate artifacts

This Topic Reference is the shared starting point. Formal artifacts will be
separate and will cite it.

| Artifact | Purpose | Resolves |
|---|---|---|
| Exemption query and response profile proposal | Assign and define the profile pair and response behavior | OQ-3 to OQ-10 |
| Exemption submission (VXU) proposal | Define unsolicited exemption updates | OQ-11 to OQ-14 |
| Deferral concept paper | Define deferrals and their use cases | OQ-1 |
| Conformance test cases | Let implementers test queries, responses, and errors | Follows the profile proposal |

The sponsoring body and publication path are open (OQ-15).

## Validation

- Experience from existing implementations is the first test of the query
  design and is recorded as review input.
- Other IIS should review the design before profiles are assigned, so the
  identifiers do not collide with local profiles.
- Message examples should be validated against national conformance tools once
  profiles exist.

## Steps

1. Review definitions, scope, and use cases with the community.
2. Confirm the explicit request and payload as the common design, or record
   objections.
3. Gather requirements from IIS and submitters interested in VXU.
4. Agree on a definition of deferral, or narrow the topic.
5. Choose a sponsoring body and prepare the first formal proposal.

---

# Appendix: Working Decisions and Open Questions

This appendix consolidates the working decisions on the topic and the
questions still open. A working decision is the current position for
discussion, not a final or approved decision. Each records where it came from so
it can be revisited. Details are in the linked sections.

## Working decisions

| ID | Working decision | Basis |
|---|---|---|
| WD-1 | A formal exemption is distinct from a refusal and a clinical contraindication ([Concepts](#concepts)). | Implementation experience; national guidance does not separate them |
| WD-2 | Exemptions do not change evaluation or forecasting ([scope](#what-this-project-does-not-cover)). | Implementation experience |
| WD-3 | The information model is transport-independent; QBP/RSP and VXU carry the same payload ([Information Model](#information-model)). | Implementation experience; VXU use case |
| WD-4 | This document defines how exemption information is communicated, not intake, lifecycle, eligibility, or who may send, receive, or use it ([scope](#what-this-project-does-not-cover)). | Implementation experience |
| WD-5 | Exemption content is returned only when explicitly requested; whether a requester receives it is IIS policy ([requested explicitly](#requested-explicitly)). | Implementation experience |
| WD-6 | The paired-observation order group is the leading worked example for exemption payloads ([paired observations](#paired-observations)). | Implementation experience |
| WD-7 | A query response is a current-state snapshot ([semantics](#current-state-semantics)). | Implementation experience |
| WD-8 | "No exemptions documented" is sent only when exemptions were requested and are being returned ([no exemptions](#no-exemptions-documented)). | Implementation experience |
| WD-9 | Exemption requests are post-coordinated in repeating `MSH-21` ([requesting](#requesting-exemptions)). | Implementation experience |
| WD-10 | Locally assigned identifiers such as `Z99` are implementation experience, not national assignments ([requesting](#requesting-exemptions)). | Implementation experience |
| WD-11 | The topic covers QBP/RSP and VXU without assuming they share an exchange contract ([unsolicited updates](#unsolicited-updates)). | Project plan |
| WD-12 | The wire representation is jurisdiction-neutral and permits a different type per vaccine category ([structure](#structure)). | Implementation experience |
| WD-13 | Deferrals stay in scope but are not fitted into the exemption structure until defined ([Deferrals](#deferrals-frame)). | Project plan |

## Open questions

Each question needs discussion before the affected text is settled. Questions
that belong to a related topic are referred there rather than answered here.

| ID | Question | Sources in tension | Status |
|---|---|---|---|
| OQ-1 | What is a deferral in this topic, and which use cases need it exchanged? | Topic name includes deferrals; no use case described | Open |
| OQ-2 | Should issuer, status, expiration, or school year be optional elements? | Query design found no need; VXU and other jurisdictions may | Open |
| OQ-3 | What vocabulary identifies exempted categories where CVX is not enough? | CVX is product-oriented; requirements are disease-oriented | Open; CVX used for now |
| OQ-4 | Are these LOINC codes the best fit? `100282-3` is a document code whose description says the exemption reason is typically carried by `85714-4`; `LL4353-0` is an example list of general non-vaccination reasons; and `LA137-2` (None) is not in that list. | Implemented use of `100282-3`; LOINC term descriptions | Open |
| OQ-5 | What national request and response profiles should be assigned, and should the first be labeled experimental to leave room for immunities or deferrals? | `Z99` used locally; a `Z70` to `Z73` range discussed; local collisions | Open |
| OQ-6 | Does combined intent appear only in `MSH-21`, or also in `QPD-1`? | Design discussion kept `QPD-1` to the primary intent; implementation test queries repeat it | Open |
| OQ-7 | What is the complete table of valid combinations, and which `ERR` codes apply? | Mechanism agreed; codes not defined | Open |
| OQ-8 | How does a response distinguish "no exemptions on file" from "exemptions not returned" or "not supported"? | "None" covers only the first case | Open |
| OQ-9 | Do legacy senders and receivers tolerate repeating `MSH-21`, and should an IIS ignore or reject an unrecognized profile? | Backward compatibility not tested broadly | Open |
| OQ-10 | With several or replaced exemptions for one category, does a response return only the active one? | Snapshot semantics; intake rules vary | Open |
| OQ-11 | Does a VXU report exemption events or a complete snapshot? | Snapshot design; update needs | Open |
| OQ-12 | What identifiers and matching rules support replacement, revocation, and deletion in VXU? | Not needed for a snapshot | Open |
| OQ-13 | Is a "none" assertion safe or meaningful in a VXU? | Safe in an IIS-built response; may erase other sources' data in a submission | Open |
| OQ-14 | What provenance and acknowledgement rules apply to VXU? | No VXU requirements gathered yet | Open |
| OQ-15 | What sponsoring body and publication path should take formal artifacts forward? | Not decided publicly | Open |
