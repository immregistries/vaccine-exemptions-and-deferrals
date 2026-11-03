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
