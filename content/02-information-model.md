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
