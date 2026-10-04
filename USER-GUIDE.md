# WebAI user guide

This guide describes the implemented evaluation workspace. Use synthetic data while evaluating it. Some controls are available only to the Owner or Admin, and an unavailable subsystem cannot be enabled merely by displaying it in your menu.

## Sign in and manage your account

The first installation creates one permanent Owner identity. The operator supplies the initial setup token privately; it stops being a setup mechanism after the first Owner is created. Optional signup creates User accounts only and is disabled by default. Email verification is not password recovery.

Sign in using your local username and password. Your sign-in is remembered across browser restarts. Use **Sign out** when finished on a shared device. Sessions can still be revoked, and clearing browser data or browser retention limits can require signing in again. My account lets you update your display name and biography and change your password using the current password. A successful password change revokes other sessions.

The Owner creates Admin and User accounts. Admin manages ordinary User accounts only and cannot list or alter the Owner account. Additional Owners cannot be created. Account administration does not grant access to another user's private content or saved OpenAI key. Forgotten-Owner recovery is an operator matter, not an Admin bypass; its complete recovery procedure remains a release limitation.

## Find your workspace

**Menu** opens or closes navigation. Groups are alphabetized: Data warehouse / MIS, Documents, Office, Personal / Home, System Administration, System Configuration, and Workflow / Automation. Inside each group, ordinary pages are alphabetized, followed by Help and AI Assistant where available. Home, Dashboard, Help, About and AI Assistant keep their fixed main-menu positions. Groups start collapsed. Empty planned groups are not installed features.

Documents contains Document Library and Write a Book. Office contains mail settings and email search. System Administration contains Files and Folders. System Configuration contains account and workspace settings. Your permissions and personal visibility preferences determine which entries appear.

**Configure workspace** changes your personal menu choices. It does not install features or change permissions. The Owner's subsystem access policy is separate and applies to service requests as well as the UI.

Panels normally start collapsed. Select their headings, or use keyboard Enter or Space, to expand them. Collapsing a panel does not discard form values. Confirmation panels can open automatically; important errors remain visible. Action styles distinguish primary, secondary, destructive and disabled buttons. Hover or focus help explains controls; field question buttons also support touch. Menu and Help topic selectors intentionally have no balloons.

Home is the introductory landing page. Choose **Dashboard** in the main menu, or **Open dashboard** on Home, for overall availability and authorized subsystem summaries. Subsystem dashboards provide their own summaries and navigation. The footer shows **WebAI**, the running application version and a brief availability summary; it is not continuous health monitoring. About shows the same application version and additional component versions.

## Document Library

Use Analytics, Dashboard, Data Search, Documents or Help under Document Library. In Documents, select a document or choose **New document**. Save a revision to record changes without overwriting history. Save or discard a draft before switching documents.

**Data Search** opens **Search Document Library**. Its collapsible panels include Search question, Saved questions and Saved results on one page. Save a question for reuse, edit it, or choose **Rerun and save** to create a dated result snapshot. Earlier results remain unchanged when you edit the question. There is no separate Library Saved Searches menu. [Search coverage and exports](AI-ASSISTANT.md#search-across-your-working-subsystems).

Uploads preserve original bytes unless you explicitly edit text. Binary content is not offered as editable text. Downloads contain saved content, not unsaved changes. History, comparison, archive and confirmed restore operate on saved documents; restore creates a new revision. Sharing is subject to the available controls and authorization.

Analytics counts authorized documents, including archived items, independently of the current editor search. It does not grant access to private documents. Refresh after permission changes; displayed summaries are snapshots.

## Files and Folders

Under **System Administration → Files and Folders**, Operations contains browsing and file actions. Initialize private folders once, select a source and browse relative paths. Initialization does not mount a host folder. Review the source, destination and confirmation before a change. Existing destinations are not silently overwritten. **Data Search** searches a selected authorized folder and includes its Saved Searches panel; no separate Saved Searches submenu is needed.

Batch input uses one source-relative path per line, with at most 64 expanded entries. Copy and Move target an existing folder; ZIP targets a new archive name. Review an archive before extraction. A changed source invalidates the review.

After an interrupted request, inspect **Current operation** before retrying. Undo can retain items for recovery instead of deleting them; restore retained items to a new destination. **Keep current state** gives up undo and is not proof that an earlier operation succeeded.

Owner-only Settings manages access to existing subfolders of an optional operator-provided mount. Read-only is the default. The application does not mount arbitrary host paths. External folder contents are not included in the internal runtime backup.

## Write a Book

Use Dashboard, Data Search, Manuscripts and Help. Create a book, add or reorder chapters, then save a revision. Removing a chapter affects the draft after confirmation; saved history remains. The title filter applies to the current page of up to 50 books. Data Search finds current saved manuscripts and includes its Saved Searches panel. Switching between Books and Files Data Search opens the selected subsystem, not the previous page's filters or results.

History does not replace your draft. A confirmed restore creates a new revision. Text and Markdown exports include saved title and chapters, not private notes or change notes. Book projects are private to their account; Owner has no content override.

## Help and language

Find Help in the main menu or a subsystem menu. Select multiple topics to keep them on one page. Selected buttons are highlighted; select one again to remove that topic. The first topic opens automatically, and additional topics can be expanded independently. Leaving or fully refreshing the Help page resets this transient selection. Help supports safe Markdown-style headings, emphasis, lists, code and notes, not executable HTML.

My account provides an English JSON translation template and one saved custom language per account. Keep its keys and numbered placeholders unchanged; translate the values, set language name/code and left-to-right or right-to-left direction, preview and apply. Missing wording falls back to English. Importing a new file replaces only your custom language. Save other edits before switching language.

Translation files are limited to 512 KiB, 2,000 entries and 4,000 characters per translation. Invalid or unknown keys are rejected. This changes interface wording, not your documents or filenames. No paid translation service is used. Browser translation is optional and separate; review your browser provider's privacy behavior.

## Account email and optional signup

Administrators configure signup and the support email under **System Configuration → Accounts**. Signup is disabled by default. When enabled, it creates **User** accounts only, with either automatic activation or administrator approval. If email verification is also required, both verification and approval must be completed before sign-in.

Verification uses an **eight-digit email code**, not a link. Enter it in the WebAI browser where you requested it. Codes expire after ten minutes, can be used once, and allow five attempts. Wait at least one minute before requesting a replacement; a replacement invalidates the earlier code. No externally reachable WebAI address is needed. The installation still needs outbound SMTP connectivity.

In **My account**, save your email address and view its verification status. Changing the address clears verification. Email verification does not provide password recovery.

The support mailbox is administrator-managed and separate from personal mailboxes. SMTP requires verified TLS: port 587 with STARTTLS or port 465 with implicit TLS. Password fields are write-only; saving settings does not connect or send mail. Optional IMAP settings are reserved for future releases; WebAI does not automatically read incoming support mail in this release. The current endpoint validator permits public IPv4 destinations only.

## Email

Use **Office → Mailbox settings** for up to 20 private named mail accounts. **Open mail** launches the selected mailbox in a separate tab, leaving WebAI available. **Office → Search email** searches the selected account and folder; **Office → Saved Searches** keeps saved email questions and snapshots. See [Webmail](WEBMAIL.md) for connection requirements and search limits.

## Connection interruptions

Preserve unsaved text before refreshing. An uncertain request must not be blindly repeated: inspect the saved revision or operation result first. Reopening a page is not confirmation that the previous change succeeded.

[Return to overview](README.md)
