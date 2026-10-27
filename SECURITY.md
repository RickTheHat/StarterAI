# Security policy

StarterAI is a documentation and agent-workflow kit. It does not itself
run a web service, store user data, or provide security guarantees for
projects created with it. Reports about an application built using this kit
should be sent to that application's maintainers instead.

## Supported versions

The following versions receive security fixes:

| Version | Supported |
| --- | --- |
| `main` | Yes |
| Latest tagged release | Yes |
| Older releases | Best effort only |

## Reporting a vulnerability

Please do not open a public issue or pull request for a suspected
vulnerability.

If this repository is hosted on GitHub, use the repository's private
vulnerability reporting feature from the **Security** tab. If private
reporting is not available, contact the maintainer through the private
contact method listed in the repository profile. Do not include secrets,
credentials, private briefs, or unnecessary personal data in a report.

Include enough detail to reproduce the issue safely:

- The affected file, release, or commit.
- The security impact and who could be affected.
- Minimal reproduction steps or a proof of concept that does not access
  other people's data or disrupt services.
- Any relevant logs or screenshots after removing secrets and personal data.
- Whether the issue affects the kit itself or a generated project.

For prompt-injection or unsafe-agent-behavior reports, include the
untrusted input and the resulting behavior, but redact credentials,
proprietary content, and private project details.

## Response targets

We aim to:

1. Acknowledge a report within five business days.
2. Triage its severity and scope within ten business days.
3. Keep the reporter informed about remediation or mitigation when
   disclosure is appropriate.

These are targets rather than guaranteed response times. Timing may depend
on the availability of a maintainer and the complexity of the report.

## Disclosure

Please allow reasonable time for investigation and remediation before
sharing vulnerability details publicly. We will coordinate public
disclosure with the reporter when practical and may credit reporters who
want attribution.

## Scope and exclusions

This policy covers files and release artifacts intentionally tracked in
this repository. The local `examples/` directory is intentionally ignored
and is not part of the public release.

Do not test systems or services that you do not own or have explicit
permission to test. Reports based only on unsupported versions, ordinary
feature requests, content disagreements, or third-party services without
a demonstrated impact on this repository are outside this policy.
