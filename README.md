# StarterAI

An opinionated kit for starting ASP.NET Core website projects with an AI
coding agent. It provides operating instructions for the agent,
recommended technical and quality defaults, and a brief template that
the human makes project-specific.

The operating instructions are provider-neutral and work with any agent
that can read Markdown. The technical defaults are specific: ASP.NET Core
MVC, SQLite, ASP.NET Core Identity, Datastar, Serilog, xUnit v3,
Playwright, and Verify. They reflect the author's preferences, and a
completed brief can override any of them with a recorded approval.

## What is in this repository

| File | Purpose | Changes per project? |
|------|---------|----------------------|
| [1-director.md](./1-director.md) | How the agent works with you: discovery, delegation, verification, authority limits, communication, and definition of done. | No |
| [2-standards.md](./2-standards.md) | Recommended technical, security, privacy, quality, and deployment defaults. Apply relevant sections; exceptions require explicit human approval. | No |
| [3-brief.md](./3-brief.md) | Blank project brief template. A human must replace its prompts with a specific project brief before kickoff. | **Yes, every project** |
| [examples/example-brief.md](./examples/example-brief.md) | A completed sample brief for a fictional project, showing what a kickoff-ready `3-brief.md` looks like. | Reference only |
| [SECURITY.md](./SECURITY.md) | How to report security issues privately and what this policy covers. | No |
| [CONTRIBUTING.md](./CONTRIBUTING.md) | Ground rules for proposing changes to the kit. | No |
| [LICENSE](./LICENSE) | MIT License. | No |

## Precedence

When documents conflict: explicit instructions from the human in chat,
then the human-completed `3-brief.md`, then `2-standards.md`, then
`1-director.md`. A project may use an exception to a standard only when the
brief names the exception and records the human's approval. The agent must
report conflicts and ask before deviating.

## Starting a new project

1. Create or open the project's folder in your AI tool.
2. Copy `1-director.md`, `2-standards.md`, and `3-brief.md` into it.
3. **Before kickoff, a human must replace the prompts in `3-brief.md` with
   the project's goals, scope, decisions, and acceptance criteria.** Mark
   irrelevant items as not applicable; do not leave template prompts in the
   active brief. See `examples/example-brief.md` for a completed sample.
   The agent may help clarify decisions after kickoff, but must not begin
   project work until the human brief is project-specific and approved.
4. Give the agent all three files together, for example:

   > Read 1-director.md, 2-standards.md, and 3-brief.md. Follow
   > 1-director.md's first action. First check that 3-brief.md is a
   > completed, project-specific brief. Ask only questions that block the
   > agreed first delivery; do not scaffold until I approve scope and
   > material decisions.

5. The agent should inspect its environment, report relevant capability
   gaps, recommend a stack based on fit, and ask the necessary blocking
   questions with sensible defaults. Record decisions and deferred or
   non-applicable standards in `docs/decisions.md`.
6. After you approve the scope and material decisions, the agent can
   initialize git, create `AGENTS.md`, create the agent factory
   (`docs/agents/FACTORY.md` plus a fixed roster of role files: security,
   reliability, product analyst, architect, backend, frontend, designer,
   test, bug investigator, code reviewer, DevOps, privacy, and technical
   writer, tailored to the project), scaffold the
   approved stack, deliver a vertical slice, then complete the agreed MVP.

The three files go to the agent together.

## After setup: day-to-day work

Once the first delivery is reviewed, ask the director for ongoing work:
features, bug fixes, research, or reviews. Be specific about the outcome.

- **Feature:** "Add X so that Y. Done when Z."
- **Bug:** "Steps, expected result, actual result." The agent reproduces it
  and adds a regression test.
- **Research:** the agent returns findings, sources, uncertainties, and a
  recommendation.

The agent should classify each request, route it through the roles in
`docs/agents/roles/` (security and reliability review every change),
keep out-of-scope ideas in
`docs/backlog.md`, and return with verified results. It should ask before
deploying, spending money, deleting data, or materially changing auth or
architecture.

New sessions or new agents should start from `AGENTS.md`,
`docs/decisions.md`, and `docs/progress.md` in the project, not from this
starter folder.

## What the agent creates in each project

- `AGENTS.md`: the single, provider-neutral instruction file for the
  selected project stack.
- `docs/decisions.md`, `docs/progress.md`, `docs/backlog.md`, and
  `docs/testing.md` (test strategy; xUnit v3, .NET Playwright, and Verify
  for browser, accessibility, snapshot, and screenshot tests).
- The agent factory: `docs/agents/FACTORY.md` (how to create, update, and
  retire roles, with a role template) and `docs/agents/roles/*.md`
  (specialist briefs with owned paths, rules, checks, and limits). Later
  sessions use it to add roles as needs arise.
- `README.md` with run, configuration, and test instructions.
- Relevant SDK/runtime version pins and a secret-free example config.

## Recommended defaults

Use these when they fit the project; they are not universal requirements.
They are the author's preferences, chosen for small, secure, long-lived
sites maintained by one person. The agent must explain a material
mismatch, recommend an alternative, and wait for your approval before
adopting it. The brief can override any of them with a recorded approval.

- ASP.NET Core MVC with Razor views; recommend another rendering model
  only when the project needs it and the human approves the change.
- SQLite in WAL mode with `synchronous=FULL` for suitable single-node
  applications that need embedded relational storage.
- User accounts when the product needs them; passwordless six-digit email
  codes through ASP.NET Core Identity are the default authentication
  approach when suitable.
- Server-rendered, classless HTML and minimal client-side JavaScript. One
  minified, replaceable stylesheet (reset first, then a variable-driven
  theme) in the style of CSS Zen Garden: mobile first, accessible,
  responsive, dark futuristic default, monospace type with a large 23px
  base size for readability. Datastar is the
  preferred enhancement in the current stack; custom JavaScript, frontend
  frameworks, and third-party scripts require approval.
- Serilog for ASP.NET Core application logging.
- Tests: xUnit v3, .NET Playwright (`Microsoft.Playwright.Xunit.v3`), and
  Verify for snapshot and screenshot baselines.
- Security and reliability are top priority: a fixed role roster reviews
  every change, and high-severity findings block completion.

## Limits to know about

- This kit is provided as is under the MIT License (see
  [LICENSE](./LICENSE)). It is an opinionated starting point, not a
  guarantee of secure or correct software. Review what the agent builds.
- Package names and versions mentioned in the standards can change;
  the agent is told to verify them, and you should too.
- The documents guide the agent; they do not guarantee compliance. Review
  its first reply and confirm it respects the completed brief and approval
  boundaries.
- Agent capabilities differ. A platform may not support every suggested
  tool or workflow; the agent should disclose gaps rather than pretend.
- Standards marked conditional apply only when the project includes that
  feature. Record non-applicable or deferred requirements.
- Legal items are not legal advice. Have applicable documents reviewed
  before public launch.
- Nothing is deployed publicly or paid for without your explicit approval.

## Contributing and license

See [CONTRIBUTING.md](./CONTRIBUTING.md). Report security issues privately
as described in [SECURITY.md](./SECURITY.md). Released under the [MIT
License](./LICENSE).
