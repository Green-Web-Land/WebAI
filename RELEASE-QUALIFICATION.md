# WebAI v0.9.2-preview.1 qualification

Build: `0.9.2-preview.1+20261006.cleanup.1`.
Image: `sha256:accbfa7b52a9efc089347d6b5d8448e869da33c7a8ea032ca7f3525b55da1b44`.

The following are developer checks on this distribution image and matching helper files. The application, static assets, webmail and banner decoder came from the image; they were not replaced by development overlays.

| Area | Passed checks | Scope |
|---|---:|---|
| Account lifetime, shared access, management and hosting services | 189 | Synthetic migration, defaults, role boundaries, expiry, revocation, recovery, image validation and restart persistence |
| Chromium and Firefox workspace flows | 254 | Hosting editor/public Home, management policy, signup/expiry/permanent conversion, narrow layouts, footer and About identity |
| Optional hosting fallback | 3 | Empty, disabled and unavailable optional content |
| Chromium and Firefox synthetic webmail | 22 | Public/private handoff, revocation, ordinary User access and management-session replay rejection |
| Packaged operator helpers | 25 | Synthetic configuration, input bounds, explicit management mapping and incomplete-backup refusal |

The image contains one flattened filesystem layer, empty installation state and no C# development sources or PDBs in its application directory. Its 232 installed Debian package entries match the supplied notice inventory. Six model files match retained identities and read-only permissions. The PHP/GD banner decoder is present in the distribution image and was exercised with valid and rejected images. Nine managed dependency entries and 149 Debian source identities are covered by the accompanying materials; 339 referenced archive file hashes were verified.

All three export parts and the complete compressed stream were checked. The OCI manifest, linked configuration and layer blobs were checked by hash, then the actual export was loaded successfully. A fresh isolated start passed readiness, packaged-model listing, first-Owner creation/login and browser-rendered public Home/version checks. The worker stopped cleanly with no OOM or host-published ports.

## Boundaries

These checks are not independent QA or complete user acceptance. Earlier bounded independent QA, including Sent-copy evidence, targets predecessor artifacts and does not qualify these new bytes. The current mail checks use synthetic local TLS services and perform no real-mail send. Full Sent-copy, real-provider interoperability, complete Data Search, model inference and backup/restore suites were not repeated for this combined release. Local-model file integrity and listing are separate from inference quality.

The retained predecessor-to-candidate synthetic state migration and repeated schema installation are not a general in-place upgrade guarantee. Automated management-aware backup/restore is unavailable. Windows/macOS Docker-host installation, ARM, arbitrary external HTTPS/proxy arrangements, complete accessibility/translation coverage, clean-daemon loading and independent reproducible rebuilds remain unqualified.

Checksums provide integrity records, not publisher signatures. WebAI Setup has separate artifacts, runtime requirements and read-only prototype qualification in its accompanying README.
