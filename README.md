# Vaccine Exemptions and Deferrals

> **Status:** Working draft for community discussion. This is not a published
> standard, an HL7 ballot, or a final decision.

Immunization information systems (IIS) increasingly hold formal school and
childcare vaccine exemptions, and schools, providers, and other
systems need to exchange them through HL7 v2 interfaces. National guidance does
not yet say how, or separate an exemption from a refusal or a clinical
contraindication. This Topic Reference sets out a technical template for
communicating exemptions, parts of which are implemented in at least one IIS,
and frames the remaining work: unsolicited updates (VXU), deferrals, and a path to
formal standards.

This repository is the Topic Reference for
[Vaccine Exemptions and Deferrals, InteropHub topic 36](https://informatics.immregistries.org/hub/es/topic/36). It records where
the topic currently stands: its definitions, principles, diagrams, working
decisions, and open questions.

## Start here

1. Read the [Topic Reference](https://immregistries.github.io/vaccine-exemptions-and-deferrals/latest/), where you can also leave
   comments, or the [GitHub reading copy](CoreDocument.md), or browse the
   [canonical source sections](content/README.md).
2. See the [working decisions and open questions](content/90-working-decisions.md).
3. Read [how to contribute](CONTRIBUTING.md).

## What belongs here, and what does not

See [Status and Scope](content/00-status-and-scope.md).

## Community coordination

The community works on this topic through InteropHub:
[Vaccine Exemptions and Deferrals, topic 36](https://informatics.immregistries.org/hub/es/topic/36). Meetings, agendas, and
discussion notes are tracked there.

## Build

Initialize the shared templates after cloning:

```powershell
git submodule update --init
```

Then build the reading copy, Word and HTML review copies, and the web guide in
`output/`:

```powershell
powershell.exe -NoProfile -ExecutionPolicy Bypass -File .\build.ps1
```

Validate source and generated files:

```powershell
powershell.exe -NoProfile -ExecutionPolicy Bypass -File .\validate.ps1
```

Pushing to `main` publishes the guide at https://immregistries.github.io/vaccine-exemptions-and-deferrals/main/. Pushing a release
tag such as `v0.1.0` publishes it at that version and at
https://immregistries.github.io/vaccine-exemptions-and-deferrals/latest/.

## Repository structure

```text
content/            canonical Markdown for the core document
products/           derived products, including the web guide
review/             review inputs awaiting adjudication
sources/            list of source material and how it is used
process/            submodule: shared public templates and tools
output/             generated files; not committed
VaccineExemptionsAndDeferrals.qmd    assembly authority for section order
CoreDocument.md     generated single-file reading copy
guide.json          web guide settings
```
