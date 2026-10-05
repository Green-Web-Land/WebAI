# Evaluation status and contributions

WebAI's demo is intended for testing, usability feedback, enhancement proposals and translation cooperation. Use synthetic data rather than critical or production records. Check [WebAI releases](https://github.com/Green-Web-Land/WebAI/releases) for download availability.

## Developer verification

The versioned v0.9.1 candidate passed 68 targeted checks: 26 application
origin/proxy, 16 installer/configuration, 6 PHP policy, 2 real nginx cookie-map,
7 packaged installation/recovery, 8 Chromium checks and 3 image/dependency checks. Browser checks covered
IP-only sign-in, separate-tab Roundcube login with a synthetic mailbox, reload
and parent sign-out revocation. The user subsequently confirmed real LAN IP-only
mailbox access on the earlier functional candidate. The versioned image also
passed its exact footer check. Download-part export and publication are separate
steps; these tests do not establish that unpublished downloads are available.

Windows/macOS Docker-host runtime, a fresh SMTP send, and the complete Data Search
suite were not repeated for this change. This is developer verification, not
independent QA or a guarantee for all environments.

### Earlier v0.9.0 verification

For WebAI v0.9.0, the isolated database-backed recovery, mail-session and enrollment
suite passed 231 checks, with 37 additional entry/browser-policy/endpoint checks.
The exact candidate image passed 12 fresh-install/backup/restore checks, five
browser smoke checks and seven copied-v0.8-backup upgrade checks. A dedicated
internal-connection proof passed 20 checks plus its PHP client checks. The existing
outbound-mail test image passed 11 protocol checks, including wrong-certificate
hostname rejection. No real mailbox or paid AI provider was contacted.
The complete historical Data Search browser suite was not rerun for v0.9.0.

### Earlier feature verification

For the 0.8.0-next.1 search/navigation build, 179 synthetic service checks and 15 navigation source checks passed. Its footer-only successor, `0.8.0-next.1+20261004.deployment.1`, passed 32 packaged browser checks, including the exact version display. Browser coverage included mail, Books and Files search, saved questions/results, exports, per-account isolation, session revocation, menu ordering and Books/Files route switching. The service checks were not rerun for the footer-only change. These are developer checks, not independent QA or a guarantee for every environment.

Mail search and sending/receiving were also confirmed by the user on their selected provider. That does not establish compatibility with every mail service. No live paid OpenAI request was made in this qualification.

The combined source-built image includes the application, relational database and local model. A bounded recovery batch passed 29 checks, covering restart and clean transfer of accounts, sessions, manuscripts, folders and encrypted-key material. An offline local-model check succeeded without network access.

The provider implementation had 40 component, 44 API and 12 role-specific browser checks. OpenAI behavior used a mock provider, not a real paid call. These checks occurred across development increments; they are not one complete regression of every feature against the final image. Copying encrypted-key material does not itself demonstrate a live provider request after restore.

**This demo has developer testing but has not undergone independent QA.** It is not certified or warranted for production use.

## Evaluation limitations

- The 0.8 installer and recovery helpers passed 11 isolated Linux checks against the exact release image, including Owner setup, login, mail configuration preservation, cold backup, restore and restart. Windows/Mac installation and broader host coverage remain unverified.
- WebAI v0.9.0 uses an authenticated private local socket for its internal mail bridge; there are no internal certificates to renew. Operator-managed browser HTTPS and outbound mail-server certificate validation remain separate responsibilities.
- Forgotten-Owner recovery does not yet have a fully verified procedure.
- Rapid or shared-address browser traffic can encounter request throttling.
- Accessibility, translation and browser coverage is limited.
- The tested private HTTPS gateway and local-only synthetic profile do not qualify every reverse proxy, certificate setup or network environment.

Read-only Data Search is available in the version described here. Arbitrary SQL, AI-driven source changes, inter-Hub features and general autonomous operations are not included.

## Report a defect

Use this repository's Issues. Include the page or feature, expected behavior, actual behavior, reproducible steps, application version and operating environment. A small synthetic example and redacted screenshot are helpful. Never attach credentials, private files or complete runtime backups.

Reports and proposals do not guarantee a response time or acceptance of every enhancement. For separately arranged professional assistance, see [About and support](ABOUT.md).

## Suggest translations

Start from the application's current English JSON template. Translate values rather than keys; preserve numbered placeholders and test long wording and text direction. Report the template/application version and a small example of any rendering problem. Interface translation does not translate user documents. No paid translation service is required.

## Licensing

Read the software licence and third-party notices supplied with a download before redistributing it. Third-party components retain their own terms; a model's licence does not license the application. The separate educational-content licence does not apply to WebAI software. See [About](ABOUT.md).

[Return to overview](README.md)
