# Security policy

StarterAI is a documentation and agent-workflow kit. It does not itself
run a web service, store user data, or provide security guarantees for
projects created with it. Reports about an application built using this kit
should be sent to that application's maintainers instead.

## Supported versions

Only the current `main` branch receives fixes. There are no versioned
releases.

## Reporting a vulnerability

Please do not open a public issue or pull request for a suspected
vulnerability.

Use the repository's private vulnerability reporting feature from the
**Security** tab on GitHub. If that is unavailable, contact the maintainer
through the contact method listed on their GitHub profile. Do not include
secrets, credentials, or unnecessary personal data in a report.

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

## Response

This is a small project maintained by one person in their spare time.
Reports are acknowledged as soon as practical, usually within a couple of
weeks, and the reporter is kept informed about remediation when disclosure
is appropriate. There are no guaranteed response times.

## Disclosure

Please allow reasonable time for investigation and remediation before
sharing vulnerability details publicly. We will coordinate public
disclosure with the reporter when practical and may credit reporters who
want attribution.

## Scope and exclusions

This policy covers the files tracked in this repository.

Do not test systems or services that you do not own or have explicit
permission to test. Ordinary feature requests, content disagreements, and
issues in third-party services without a demonstrated impact on this
repository are outside this policy.
