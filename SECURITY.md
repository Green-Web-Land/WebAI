# Security boundaries

WebAI protections apply within the implemented application and container controls. A fully secure environment also requires protection of the host, Docker access, network, browser, secrets and backups. The demo is not a security certification.

## Application protections

Local accounts have distinct Owner, Admin and User authority. The first Owner identity cannot be replaced through the application. Admin cannot list or change that identity. Subsystem policy is enforced on service access, not just menu visibility. Hiding a menu does not revoke permissions.

Users' OpenAI keys are encrypted and write-only through the application. Owner and Admin have no API to retrieve another user's key. That protection does not survive hostile control of the host, runtime, process memory or database plus keyring. A protected Docker image is not a guarantee against inspection by its host administrator.

The Help Assistant has no data-operation authority. Imported translations and Help formatting must remain inert text, not scripts. Neither a Help instruction nor a generated answer grants permission to run an operation.

Data Search is separate from Help. Models interpret questions only; WebAI applies permissions, executes bounded read-only searches and formats results. Functional MCP endpoints do not accept arbitrary SQL or filesystem commands. Saved-result access and exports are rechecked; an old snapshot is not an authorization bypass. An external MCP client is responsible for how it uses returned data.

Personal mail credentials are private to each account through application interfaces. IMAP/SMTP require validated TLS; server destinations are checked to prevent access to private or reserved addresses. An authentication failure is not automatically retried. Administrative password recovery clears saved mail credentials. Support SMTP is separately administrator-managed and sends verification codes only through explicit account workflows; optional support IMAP does not automatically read mail.

Browser sign-ins persist across restarts using a cookie inaccessible to ordinary page scripts, rather than an authentication token in local storage. Sign-out, explicit revocation, suspension and password recovery still invalidate access. A stolen persistent session remains a risk until revoked, so protect your browser profile and sign out on shared devices. Browser retention policies and clearing site data can still end a saved sign-in.

## Operator responsibilities

WebAI v0.9.0 does not generate, install or renew deployment certificates, modify
trust stores or edit hosts files. Its internal mail bridge is an authenticated
owner-only Unix socket, not a network service. The local IPC key belongs in the
private state backup. This does not weaken external IMAP/SMTP or AI-provider TLS
validation, nor remove application authentication and authorization checks.

See [connection setup](HTTPS-SETUP.md) for certificate-free loopback-only access
and network HTTPS gateway, DNS and certificate choices.
Browser trust and the container's trust of external IMAP/SMTP servers are separate
requirements. Neither a hosts-file edit nor a certificate installed inside Docker
establishes browser trust. Never bundle a shared private key or instruct users to
bypass browser warnings. Private-CA trust installation requires explicit consent.

- Restrict access to the host and Docker daemon. Never expose or mount its socket into this application.
- Keep system software maintained and protect network access. Do not expose internal database or model ports.
- Use HTTPS for remote browser access. Unencrypted HTTP is not suitable for real provider keys or public exposure.
- Protect backups and the encryption keyring together. Store recovery copies with access controls appropriate to their contents.
- Expose only explicitly selected optional host folders, preferably read-only. A container is not a substitute for host filesystem permissions.

Internet-facing operation and reverse-proxy configurations have not been fully verified. Browser, accessibility and security testing is limited. Global request throttling can affect rapid or shared-address traffic. See [evaluation limitations](EVALUATION.md).

## Reporting a security concern

Do not place passwords, tokens, runtime backups, private documents or exploitable sensitive details in a public issue. Contact the publisher privately at [us@itisthebest.com](mailto:us@itisthebest.com) with a short description and request a suitable way to share sensitive evidence. Do not email credentials or complete backups.

[Return to overview](README.md)
