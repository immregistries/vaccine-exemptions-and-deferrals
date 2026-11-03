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
