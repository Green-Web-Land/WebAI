# Evaluation status and contributions

WebAI's demo is intended for testing, usability feedback, enhancement proposals and translation cooperation. Use synthetic data rather than critical or production records. Check [WebAI releases](https://github.com/Green-Web-Land/WebAI/releases) for download availability.

## Developer verification

For the 0.8.0-next.1 search/navigation build, 179 synthetic service checks and 15 navigation source checks passed. Its footer-only successor, `0.8.0-next.1+20261004.deployment.1`, passed 32 packaged browser checks, including the exact version display. Browser coverage included mail, Books and Files search, saved questions/results, exports, per-account isolation, session revocation, menu ordering and Books/Files route switching. The service checks were not rerun for the footer-only change. These are developer checks, not independent QA or a guarantee for every environment.

Mail search and sending/receiving were also confirmed by the user on their selected provider. That does not establish compatibility with every mail service. No live paid OpenAI request was made in this qualification.

The combined source-built image includes the application, relational database and local model. A bounded recovery batch passed 29 checks, covering restart and clean transfer of accounts, sessions, manuscripts, folders and encrypted-key material. An offline local-model check succeeded without network access.

The provider implementation had 40 component, 44 API and 12 role-specific browser checks. OpenAI behavior used a mock provider, not a real paid call. These checks occurred across development increments; they are not one complete regression of every feature against the final image. Copying encrypted-key material does not itself demonstrate a live provider request after restore.

**This demo has developer testing but has not undergone independent QA.** It is not certified or warranted for production use.

## Evaluation limitations

- The 0.8 installer and recovery helpers passed 11 isolated Linux checks against the exact release image, including Owner setup, login, mail configuration preservation, cold backup, restore and restart. Windows/Mac installation and broader host coverage remain unverified.
- Internal mail-bridge identities created by the installer expire after 365 days. Operator-controlled renewal is required; automatic renewal is not included.
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
