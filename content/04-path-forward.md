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
