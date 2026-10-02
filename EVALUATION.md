# Evaluation status and contributions

WebAI's demo is intended for testing, usability feedback, enhancement proposals and translation cooperation. Use synthetic data rather than critical or production records. Check [WebAI releases](https://github.com/Green-Web-Land/WebAI/releases) for download availability.

## Developer verification

The combined source-built image includes the application, relational database and local model. A bounded recovery batch passed 29 checks, covering restart and clean transfer of accounts, sessions, manuscripts, folders and encrypted-key material. An offline local-model check succeeded without network access.

The provider implementation had 40 component, 44 API and 12 role-specific browser checks. OpenAI behavior used a mock provider, not a real paid call. These checks occurred across development increments; they are not one complete regression of every feature against the final image. Copying encrypted-key material does not itself demonstrate a live provider request after restore.

**This demo has developer testing but has not undergone independent QA.** It is not certified or warranted for production use.

## Evaluation limitations

- Fresh-host installation and final startup instructions are not yet fully verified.
- Forgotten-Owner recovery does not yet have a fully verified procedure.
- Rapid or shared-address browser traffic can encounter request throttling.
- Accessibility, translation and browser coverage is limited.
- HTTPS and reverse-proxy configurations are not yet fully verified.

AI database actions, inter-Hub features and other future expansions are not demo features.

## Report a defect

Use this repository's Issues. Include the page or feature, expected behavior, actual behavior, reproducible steps, application version and operating environment. A small synthetic example and redacted screenshot are helpful. Never attach credentials, private files or complete runtime backups.

Reports and proposals do not guarantee a response time or acceptance of every enhancement. For separately arranged professional assistance, see [About and support](ABOUT.md).

## Suggest translations

Start from the application's current English JSON template. Translate values rather than keys; preserve numbered placeholders and test long wording and text direction. Report the template/application version and a small example of any rendering problem. Interface translation does not translate user documents. No paid translation service is required.

## Licensing

Read the software licence and third-party notices supplied with a download before redistributing it. Third-party components retain their own terms; a model's licence does not license the application. The separate educational-content licence does not apply to WebAI software. See [About](ABOUT.md).

[Return to overview](README.md)
