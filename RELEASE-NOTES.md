# WebAI v0.9.2-preview.1

Build identity: `0.9.2-preview.1+20261006.cleanup.1`.

This pre-release combines the workspace layout corrections, configurable temporary User registration, optional independent Owner/Admin management-login restrictions, and a public Home page with an optional Owner-managed hosting introduction.

Account screens use the shared workspace form layout. Temporary registrations can receive a configured lifetime in hours; expired accounts lose access, and an administrator can convert them to permanent access. Hosting text supports a bounded Markdown subset and an optional validated image. Guest Home access does not grant access to private workspace data or settings.

The application remains HTTP-first. Browser HTTPS and the authenticated private management gateway are operator responsibilities. The gateway helper is included with the matching operator package; enabling restrictions requires the documented management path. Do not infer a trusted management connection from a forwarded IP header alone.

Use the matching image manifest, operator helpers and documentation. WebAI Setup has its own version and is a read-only Linux x64 prototype; it does not install or update WebAI.

See [demo and hosting](DEMO-AND-HOSTING.md), [management access](MANAGEMENT-ACCESS.md), [installation](INSTALLATION.md) and [qualification](RELEASE-QUALIFICATION.md) for the exact developer checks and their limits. Use the accompanying manifest and checksums to identify the matching downloads.
