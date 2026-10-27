# Project brief: [project name]

**Human owner:** [name]  
**Last updated:** [YYYY-MM-DD]  
**Status:** Draft / approved

This is the human-completed brief. Replace every bracketed prompt before
kickoff and mark irrelevant items "Not applicable". The agent may help
clarify open decisions but must not draft this brief or start work while
it is a template.

Operating rules live in `1-director.md` and defaults in `2-standards.md`.
Name any requested exception to a standard and record my explicit
approval below or in `docs/decisions.md`; being in this brief is not
approval.

## Goal

- **What are we building?** [Describe the website or web application.]
- **Problem and desired outcome:** [What problem should it solve, and
  what should improve for users?]
- **Intended users:** [Who will use it?]
- **Launch context:** [Prototype, internal tool, public website, hosted
  product, or other. State intended launch date or constraints if known.]

## MVP

List the smallest useful first release as user-visible capabilities.
For each item, state who uses it and what successful completion looks
like.

- [Capability and measurable result.]

## Exclusions

List features, audiences, integrations, and behaviors that are
intentionally out of scope for the first release.

- [Explicitly excluded item.]

## Key workflows and content

- **Primary workflows:** [Describe the most important user journeys.]
- **Public and private areas:** [Which pages or information are public,
  private, or restricted?]
- **Content and branding:** [Existing copy, brand assets, tone, domain,
  or content owner. Mark unknown items.]
- **Integrations:** [Required external services, or "none".]

## Project decisions and constraints

Resolve known constraints before kickoff. The agent may ask about
unresolved decisions that block the first delivery; others are recorded as
deferred or not applicable in `docs/decisions.md`.

- **User accounts:** [Yes / no. Accounts are the starter default; if
  yes, describe who may register and any required roles.]
- **Data:** [What information is stored, how sensitive it is, retention
  and deletion expectations, export/import needs, and expected data
  volume.]
- **Payments or money:** [Whether payments or monetary values are
  involved; supported currencies and relevant rules.]
- **Languages, locale, and time zones:** [Supported values or defaults.]
- **Hosting and availability:** [Local, internal, or public; target
  hosting if known; expected traffic or uptime needs.]
- **Budget and paid services:** [Budget or zero-spend constraint.]
- **Technical constraints:** [Required platforms, tools, policies, or
  constraints from `2-standards.md`. List exceptions separately.]
- **Approved exceptions:** [For each exception, name the standard,
  chosen alternative, and your explicit approval/date; otherwise write
  "None".]
- **Open decisions:** [Decision, recommended default, and whether it
  blocks the first delivery.]

## Project-specific rules

List business rules, privacy constraints, design expectations, and
operational requirements that apply throughout the project.

- [Rule and how it should be enforced.]

## User experience and acceptance

- **Visual direction:** [Describe the desired look and feel, brand
  colors, and approved references, or write "default theme" to keep the
  dark futuristic default from `2-standards.md`. Say whether a light
  theme or theme switching is in scope.]
- **Responsive behavior:** [Important devices, viewport sizes, or
  workflows.]
- **Accessibility:** [Project-specific needs beyond the standards.]
- **Performance:** [Project-specific budget or constraints; the
  standards define a baseline.]
- **Critical acceptance tests:** [Observable pass/fail criteria for
  each MVP capability.]

## Initial delivery

After the human approves scope and material decisions:

- [Define the first useful vertical slice.]
- [List the evidence or checks required for the initial delivery.]
- [List any known operational or launch constraints.]

Do not deploy publicly, access production data, or provision paid
resources without explicit human approval.
