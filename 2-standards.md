# Project standards and recommended defaults

Recommended defaults, not a mandate to use every technology in every
project. Apply only relevant sections. A brief may depart from a default
only by naming it and recording my explicit approval. If a default
doesn't fit, explain why, recommend an alternative, and wait for approval.
Record decisions, non-applicable sections, deferred items, and approved
exceptions in `docs/decisions.md`.

Default stack: ASP.NET Core MVC with Razor views, SQLite, ASP.NET Core
Identity with passwordless email OTP (when accounts are needed), minimal
JavaScript with Datastar, Serilog, and xUnit v3 with .NET Playwright and
Verify for tests. Assess fit before scaffolding.

## AGENTS.md requirements

Root `AGENTS.md` must include this paragraph verbatim in a section near
the top titled "Communication and critique":

> Act as a rigorous, honest mentor. Do not default to agreement. Identify weaknesses, blind spots, and flawed assumptions. Challenge ideas when needed. Be direct and clear, not harsh. Prioritize helping me improve over being agreeable. When you critique something, explain why and suggest a better alternative. Keep the responses short and firm, do not shorten anything if we lose the response meaning but just keep to the facts so I don't have to read a book each time a response has been generated.

It must also include the role roster linking `docs/agents/FACTORY.md` and
each file in `docs/agents/roles/` (`1-director.md`, section 5). Role files
cite these standards by section and never copy them. The security role
covers hardening, authentication, and privacy sections that apply; the
test role covers "Code quality and testing" and "Universal acceptance
tests".

## Security and reliability come first

They outrank speed and scope. The gates in `1-director.md`, section 5,
enforce this: every change gets security, reliability, test, and
independent code review; unresolved high-severity findings block
completion unless I accept the risk in writing in `docs/decisions.md`; on
conflict, recommend the safe path and ask. Reliability review covers
error handling, failure modes, logging and monitoring, data integrity,
backup and restore, performance limits, health checks, and rollback. Bugs
start with the bug investigator, and the fix includes a regression test.

## Decisions required before implementation

Resolve only applicable decisions that block the first delivery; record
others as deferred:
- User accounts (default yes unless the brief opts out); if yes,
  registration policy: any verified email or approved users only.
- Authentication method and, with cookies, expiration (baseline: 14-day
  absolute; record trade-offs for any other).
- Administrator bootstrap and recovery, if admin roles exist.
- Production email provider, if the product sends email.
- Hosting target and data-storage location.
- Data access and migration approach, if relational data is stored.
- Stripe billing scope, currency, languages, and time zone handling, if
  applicable.
- Backup frequency, retention, and acceptable data loss (RPO/RTO), if data
  needs backup.
- Visual direction and blocking product decisions from the brief.

Recommend only technology that fills real gaps, flag compatibility,
security, and deployment concerns, and record agreed decisions with
testable acceptance criteria.

## Framework and rendering

- Use ASP.NET Core on the latest stable, supported .NET SDK at project
  start. No previews. Prefer LTS unless you explain otherwise. Pin the SDK
  in `global.json`.
- Use the MVC model: controllers and Razor views (`dotnet new mvc`); don't
  mix rendering models. If another model fits the brief better, explain
  the trade-off and get approval before scaffolding.
- Server-rendered HTML with layouts, partials, and view components as
  needed; strongly typed view models (no entities or `ViewBag`); thin
  controllers. No SPA, frontend framework, or web components without
  approval.
- Prefer ordinary forms and server-side navigation. Datastar is the
  preferred enhancement, only where a specific interaction needs it:
  - Pin the version and self-host it (no CDN) unless I approve.
  - It evaluates expressions at runtime and may need `unsafe-eval` in the
    CSP. Confirm a CSP-compatible approach first and disclose relaxations.
  - It uses server-sent events: check proxy buffering and timeouts, and
    CSRF protection.
- Core workflows work without Datastar.
- Custom JavaScript, service workers, PWA/offline, analytics, CAPTCHA, and
  third-party scripts need approval; flag the conflict. Test-only tooling
  (browser automation, accessibility scanners) is allowed; record it.
- Server-side validation and authorization are mandatory regardless of
  client behavior.
- Without approved JavaScript, don't rely on browser APIs such as time
  zone detection. Use explicit user settings or documented server defaults.

## Database and persistence (if data is stored)

- SQLite in WAL mode is the default for small single-node deployments. If
  traffic, hosting, or data needs differ, explain and get approval.
- Prioritize integrity over write speed: `synchronous=FULL` unless I
  approve a different trade-off.
- Short transactions, sensible indexes, bounded busy timeout, bounded
  retries where safe, and no blind retries of non-idempotent operations.
- Apply connection settings (foreign keys, busy timeout, synchronous) on
  every connection. Use database constraints, including unique emails for
  email accounts.
- Test concurrent writes and report contention limits. Don't claim WAL
  prevents corruption or allows unlimited writers.
- Record the data access approach (for example EF Core) and migration
  mechanism. Migrations are versioned, reviewable, and tested on a copy of
  realistic data.
- Store the database on durable local storage (not ephemeral or network
  filesystems), outside public directories.
- Back up with SQLite-aware methods that account for WAL; never copy the
  live file. Document backup, restore, migration, and recovery.
- Don't assume a shared SQLite file scales horizontally or run overlapping
  instances against it during deploys unless designed and tested. Flag
  requirements beyond this design.

## Authentication and identity (if accounts are needed)

- The brief may opt out of accounts; then mark this section not applicable.
- Default: ASP.NET Core Identity with passwordless six-digit email codes.
  If it doesn't fit, explain trade-offs and get approval. No passwords,
  social login, or other methods without approval.
- Unique normalized email enforced by the database. Existing users sign
  into their account; create an account only after email verification.
  Clarify open versus approved-user registration.
- Codes: cryptographically random, leading zeros kept, expiring,
  single-use, bounded attempts, atomic verification, stored securely (no
  plaintext or unkeyed hash of the small code space).
- Cooldowns and rate limits on issuing and checking codes, per account and
  per source. Limit overall email volume to prevent email bombing. Ask
  before using CAPTCHA (it conflicts with the JavaScript rules).
- Generic responses to limit account enumeration. Never log OTPs, auth
  cookies, or secrets.

**Account lifecycle** (implement or explicitly defer in the brief): logout
and sign-out of all sessions; view and revoke sessions or devices; change
email with verification; account deletion with what is kept and erased;
data export before deletion; admin disable or suspend; inactive accounts.

**Local email (if OTP):** SMTP on localhost port 25. Show the code in the
login page only in the local Development environment. In development, any
valid email may register. Never send real email from the local setup. In
production, never return the code in HTML, API responses, or logs, and
test that this can't be enabled.

**Production email (if email is sent):** record the provider, keep
credentials out of source, configure SPF, DKIM, and DMARC with a dedicated
from-address, and document the DNS records. On send failure, timeout, or
bounce: show a safe generic message, log without the code, and leave no
usable code. Messages are plain and transactional only; no marketing
without approval.

**Cookies (if cookie auth):**
- Production: 14-day absolute ticket, no sliding expiration. Any other
  policy needs recorded trade-offs and my approval.
- HTTPS, Secure, HttpOnly, and a SameSite policy compatible with the auth
  and Stripe flows. Protect and persist Data Protection keys so deploys
  don't invalidate cookies.
- Support logout and revocation, with security-stamp validation under
  Identity. Lifetime never bypasses these. CSRF protection on
  state-changing requests.

## Web security hardening

- Headers: HSTS (production), `X-Content-Type-Options`, restrictive
  `Referrer-Policy`, clickjacking protection, and a CSP as strict as
  dependencies allow. Document relaxations.
- Rate-limit all sensitive and expensive endpoints, with forwarded-header
  and proxy trust configured so limits see the real client address.
- Request size limits and sensible timeouts.
- Encode output by default; no raw HTML of user content; validate redirect
  targets.
- Custom 404 and error pages that reveal no internals.
- A `robots.txt` suited to the project (authenticated areas not indexed).

## Admin bootstrap and authorization (if admin roles exist)

- Never assume the first user is an admin. Use a controlled bootstrap with
  a configured email or explicit setup authorization in production,
  assigned atomically so concurrent signups can't create multiple admins.
- Everyone else is a non-admin until promoted through an authorized
  mechanism. Enforce authorization and data ownership on the server.
- Define recovery if admin access is lost.
- By default admins manage accounts (disable, view metadata) but can't read
  users' private records. Other access must be approved, justified, and
  audited.

## Stripe (if applicable)

- Use the official .NET SDK, test mode in development, and server-side
  secret keys outside source control. Clarify billing model and flows, and
  refunds, failed payments, cancellations, tax, and currency, before
  building.
- Verify webhook signatures on the unmodified body. The webhook is exempt
  from CSRF and cookie auth, never from signature verification. Process
  events idempotently, handling retries and out-of-order delivery.
- A browser success redirect is not proof of payment. Never handle raw card
  data.
- Prefer Stripe-hosted pages (Checkout, Customer Portal). Flag any
  Stripe.js or embedded element that conflicts with the JavaScript rules.

## Logging, errors, and monitoring

- Serilog for structured logging, with ASP.NET Core exception handling
  configured separately. Safe production errors with no stack traces and
  useful correlation IDs.
- Never log OTPs, credentials, cookies, payment secrets, or unnecessary
  personal data. Set log destinations, retention, and access controls.
- Health checks (liveness and, when relevant, data-store readiness) that
  expose nothing sensitive.
- Define uptime monitoring, error alerting, and how I am notified.

## Money and dates (if applicable)

- Money in integer minor units, never floating point. Confirm currency and
  precision; never total mixed currencies. Define whether entries are
  transactions or aggregates and which amounts are allowed (zero,
  negative).
- Distinguish when an event occurred from when it was recorded. Report
  boundaries use the user's timezone and the event date. Store audit
  timestamps in UTC. Define how timezone affects entry dates, filters,
  and totals.
- Use an injectable clock (`TimeProvider`) and test year, month, and
  daylight-saving boundaries.
- Prevent duplicates from resubmission. Summaries, totals, growth figures,
  and exports must match the underlying entries.

## Data privacy and isolation (if user or personal data is stored)

- Enforce ownership server-side on reads, writes, deletes, and exports.
  Never trust a browser-supplied user ID. Admin status doesn't grant
  access to private records.
- Collect only what features need. Keep personal data private to its owner
  and out of logs.
- Define export, deletion, and retention before public release. Exports
  give a complete, documented, re-importable copy where the brief calls
  for restore. Guard CSV exports against formula injection.
- Before public release, assess which privacy policy, terms, storage or
  cookie notice, support contact, and age disclosures apply, and which
  regimes (for example GDPR, CCPA) need review. This is not legal advice;
  have documents reviewed.
- Define breach response: who decides, how users are told, how evidence is
  preserved.

## User experience and accessibility

- Semantic HTML, accessible labels, keyboard navigation, visible focus,
  and understandable validation messages. Target WCAG 2.2 AA and disclose
  the limits of accessibility verification.
- Preserve valid input on failed validation. Provide clear empty, success,
  and failure states. Confirm destructive actions or provide reliable undo.
- Agree on a visual direction before substantial UI work; the default
  theme applies unless the brief says otherwise. The brief records whether
  a light theme or theme switching is in scope.
- Support the latest and previous major Chrome, Edge, Firefox, and Safari,
  including iOS Safari and Android Chrome. Test from 320px up.
- Key pages: LCP of 2.5s or less, CLS of 0.1 or less, and 500KB or less
  initial transfer, under a documented mobile/slow-network profile. Use
  repeatable tooling and disclose when targets can't be measured or met.
- Public pages: favicon, titles, meta descriptions, social-share metadata.
- Define language and number/date/currency culture explicitly, even for
  one locale.

### CSS and theming (default)

Classless HTML plus one replaceable stylesheet, in the spirit of CSS Zen
Garden: markup stays fixed, and swapping the CSS changes the look.

- **Classless.** Semantic elements styled by element, attribute, and
  structural selectors; no `class` for styling. `id`, `data-*`, `aria-*`,
  and `[type]`/`[role]` hooks only where semantics can't express the need,
  each listed in `docs/decisions.md`. Datastar attributes are not styling
  hooks.
- **One minified stylesheet.** Source in `css/site.css`; production ships
  `site.min.css`, with minification in the single build command. No CSS
  framework, preprocessor, or utility library without approval.
- **Order:** reset, theme variables (`:root` properties for color, font,
  size, spacing, radius), base elements, layout and components by element,
  media queries. Keep it short and commented so anyone can replace the file
  or just the variables.
- **Replaceable.** No inline styles or CSS in templates; templates don't
  depend on a specific stylesheet. Document in the README how to swap it.
- **Brief-driven.** Re-skin from `3-brief.md` (brand colors, tone, approved
  font) by changing only the variables. The director proposes values; I
  approve.
- **Default theme: futuristic, dark but not too dark.** Soft charcoal or
  slate background (not pure black), slightly lighter surfaces, off-white
  text, one cool accent (cyan or electric blue) and a restrained secondary.
  Subtle borders, gentle radii, minimal glow or gradient. No heavy effects;
  honor `prefers-reduced-motion`.
- **Type.** `Monaco, "SF Mono", Menlo, Consolas, "DejaVu Sans Mono",
  monospace`. Monaco isn't on every platform, so the fallbacks are
  required; no web fonts or font services without approval. Base size 23px
  set once (`--size: 23px`) and used via `rem`/`em`, for older readers and
  one-value scaling. Line height about 1.5 or more; measure 60-70
  characters.
- **Mobile first, responsive.** Base styles for the smallest screens, then
  `min-width` queries. From 320px up: no horizontal scroll, fluid layouts,
  touch targets of at least 44x44px, text resizable to 200%.
- **Accessible.** WCAG 2.2 AA contrast (4.5:1 body, 3:1 large text and UI)
  in the default theme and any re-skin, visible focus, no color-only
  meaning. Playwright accessibility and screenshot tests run on the default
  theme and after every theme change. A re-theme that fails contrast,
  mobile layout, or screenshot review isn't done; designer and test roles
  own those checks.

## Code quality and testing

- .NET: nullable reference types and analyzers on; warnings as errors where
  practical. Provide `.editorconfig`, format/lint checks, and static
  analysis.
- Layers: unit tests for meaningful logic, integration tests for database
  and HTTP boundaries, end-to-end tests for critical journeys. Also test
  authorization, data isolation, concurrency, idempotency, date
  boundaries, and production-configuration guards where they apply.
- **Stack:** xUnit v3 (`xunit.v3`) with `WebApplicationFactory`; .NET
  Playwright via `Microsoft.Playwright.Xunit.v3` (not the v2 package);
  `Verify` with `Verify.XunitV3` for snapshots of API responses, rendered
  HTML, and emails (scrub dates and IDs); `Verify.Playwright` with an image
  comparer such as `Verify.ImageMagick` for screenshots. `dotnet test` is
  the single entry point. Confirm exact package names and compatible
  versions when pinning, and record the stack in `docs/decisions.md`.
- **Browser tests:** Playwright for end-to-end journeys, accessibility scans
  (`Deque.AxeCore.Playwright`), screenshots at defined viewports, and
  traces on failure. .NET Playwright has no screenshot assertion, so Verify
  manages baselines and diffs. Keep baselines deterministic (fixed data,
  time, viewport), review `.verified` changes as deliberate diffs, and
  never accept received files just to pass. Run Playwright in CI when
  available.
- Document the strategy, mapping risks to test types, in `docs/testing.md`.
  Test data is safe and synthetic.
- One documented command to build, test, and lint, listed in `AGENTS.md`.
- CI, when available, runs build, tests, format/analyzers, and the
  dependency vulnerability audit on every pull request. No paid CI without
  approval.

## Universal acceptance tests

Show evidence that:
- Critical workflows meet the brief's acceptance criteria.
- Security headers and safe error responses are present.
- State-changing browser requests have CSRF protection.
- Core workflows work by keyboard alone and pass automated accessibility
  scans (Playwright with axe) plus agreed manual checks.
- Key pages and states have screenshot baselines at agreed viewports,
  reviewed as deliberate diffs.
- Repeated submissions create no unintended duplicates.
- Data survives restarts and a backup/restore drill succeeds, when data
  needs recovery.
- Every fixed bug has a regression test that failed before the fix.
- Security and reliability reviews are complete with no unresolved
  high-severity findings, or I have recorded an accepted risk.

Also test, when applicable: registration, login, lifecycle, and data
isolation; OTP expiry, single use, and rate limits, with production-only
safeguards; ownership and admin restrictions, and controlled admin
bootstrap; cookie security and lifetime; log redaction of secrets and
personal data; date, currency, and timezone boundaries; Stripe webhook
signatures and idempotency.

Add feature-specific tests from the brief. Run checks against the
integrated application and disclose anything unverified.

## Deployment and recovery

- Keep development, test, and production configuration separate. Provide a
  staging check where feasible or disclose its absence.
- Secrets stay outside source control. Document required configuration
  with a secret-free example, and validate it at startup, failing clearly.
- With SQLite, use a single application node with durable local storage.
  Document domain, DNS, and TLS setup and renewal.
- HTTPS in production with reverse-proxy trust configured correctly. Keep
  the SQLite database and Data Protection keys on durable storage.
- If data needs recovery: set backup frequency and retention from the
  acceptable loss, keep encrypted, access-controlled backups off the host,
  document who can restore, demonstrate a restore before launch, and repeat
  it periodically.
- Back up before production migrations. Define migration and start order
  and a rollback plan. Never delete or recreate a production database to
  fix a migration.
- Provide deployment and health-check instructions, plus recovery runbooks
  (restore, rollback, key and secret rotation) owned by reliability and
  DevOps.
- Public deployment and paid provisioning need my approval.

## Technical approach

Recommend a maintainable stack for the agreed scope and environment. The
stack here is a starting point; get approval before adopting a materially
different framework, data store, or authentication architecture. Prefer few,
well-supported dependencies and avoid unnecessary services. Don't collect
sensitive information the product doesn't need.
