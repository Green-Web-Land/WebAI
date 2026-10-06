# Public Home and temporary demo accounts

Home at `/workspace` is public. Visitors can read the product introduction and any enabled hosting introduction, then sign in or register when signup is enabled. Viewing Home does not grant access to protected tools, dashboards, APIs, documents, files or mail.

Owner can open **System Configuration → Hosting introduction** to enable an optional hosting name, Markdown description and banner. It defaults off. Name is limited to 100 characters; description to 2,000. PNG/JPEG banners are limited to 512 KiB and 1600 × 400 pixels, displayed proportionally at up to 160px high. Uploads are decoded/re-encoded; metadata and unsupported content are rejected or stripped. Markdown is a safe subset with no active HTML, image embeds or links.

Preview does not publish. Save commits the complete selection; Cancel reloads saved content. Disabling retains the content privately and stops public banner delivery. Removing a banner takes effect on Save. Content is installation database data and survives normal repeatable startup; it is not part of the release image. Already downloaded content cannot be withdrawn from visitors. Admin and User accounts cannot edit hosting content.

Owner/Admin can enable **Temporary User registration** under account administration and choose 1–8760 hours (initial default 24). Signup itself and temporary registration default off. New public registrations receive the User role. The access period starts after required email verification and/or approval completes; visitors see the duration before registration.

Expiry suspends protected application and mail access without deleting data. Owner/Admin can extend access or choose **Make permanent and enable** for a suspended temporary User. Conversion preserves the User role and data; old sessions remain revoked and a fresh sign-in is required. Existing permanent accounts are not silently converted by changing registration defaults. The operator remains responsible for host resources, retention and public-demo operation.

Restricted Owner/Admin accounts use the separate [management connection](MANAGEMENT-ACCESS.md). WebAI Setup is a separate read-only prototype and does not deploy or update the demo.
