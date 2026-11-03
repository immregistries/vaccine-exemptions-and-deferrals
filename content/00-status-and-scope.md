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
