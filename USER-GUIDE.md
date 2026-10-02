# WebAI user guide

This guide describes the implemented evaluation workspace. Use synthetic data while evaluating it. Some controls are available only to the Owner or Admin, and an unavailable subsystem cannot be enabled merely by displaying it in your menu.

## Sign in and manage your account

The first installation creates one permanent Owner identity. The operator supplies the initial setup token privately; it stops being a setup mechanism after the first Owner is created. There is no public signup or email recovery dependency.

Sign in using your local username and password. Your sign-in is remembered across browser restarts. Use **Sign out** when finished on a shared device. Sessions can still be revoked, and clearing browser data or browser retention limits can require signing in again. My account lets you update your display name and biography and change your password using the current password. A successful password change revokes other sessions.

The Owner creates Admin and User accounts. Admin manages ordinary User accounts only and cannot list or alter the Owner account. Additional Owners cannot be created. Account administration does not grant access to another user's private content or saved OpenAI key. Forgotten-Owner recovery is an operator matter, not an Admin bypass; its complete recovery procedure remains a release limitation.

## Find your workspace

**Menu** opens or closes navigation. Expand a module, then a subsystem, to choose a page. Modules include Personal / Home, Documents / Office, Data / MIS, System Administration and Workflow / Automation. Empty groups are placeholders for future work.

**Configure workspace** changes your personal menu choices. It does not install features or change permissions. The Owner's subsystem access policy is separate and applies to service requests as well as the UI.

Panels normally start collapsed. Select their headings, or use keyboard Enter or Space, to expand them. Collapsing a panel does not discard form values. Confirmation panels can open automatically; important errors remain visible. Action styles distinguish primary, secondary, destructive and disabled buttons. Hover or focus help explains controls; field question buttons also support touch. Menu and Help topic selectors intentionally have no balloons.

Home is the introductory landing page. Choose **Dashboard** in the main menu, or **Open dashboard** on Home, for overall availability and authorized subsystem summaries. Subsystem dashboards provide their own summaries and navigation. Refresh to obtain a new snapshot; the footer is a brief availability summary, not continuous health monitoring.

## Document Library

Use Dashboard, Documents, Analytics or Help under Document Library. Search and select a document, or choose **New document**. Save a revision to record changes without overwriting history. Save or discard a draft before switching documents.

Uploads preserve original bytes unless you explicitly edit text. Binary content is not offered as editable text. Downloads contain saved content, not unsaved changes. History, comparison, archive and confirmed restore operate on saved documents; restore creates a new revision. Sharing is subject to the available controls and authorization.

Analytics counts authorized documents, including archived items, independently of the current editor search. It does not grant access to private documents. Refresh after permission changes; displayed summaries are snapshots.

## Files and Folders

Operations contains browsing and file actions. Initialize private folders once, select a source and browse relative paths. Initialization does not mount a host folder. Review the source, destination and confirmation before a change. Existing destinations are not silently overwritten.

Batch input uses one source-relative path per line, with at most 64 expanded entries. Copy and Move target an existing folder; ZIP targets a new archive name. Review an archive before extraction. A changed source invalidates the review.

After an interrupted request, inspect **Current operation** before retrying. Undo can retain items for recovery instead of deleting them; restore retained items to a new destination. **Keep current state** gives up undo and is not proof that an earlier operation succeeded.

Owner-only Settings manages access to existing subfolders of an optional operator-provided mount. Read-only is the default. The application does not mount arbitrary host paths. External folder contents are not included in the internal runtime backup.

## Write a Book

Use Dashboard, Manuscripts and Help. Create a book, add or reorder chapters, then save a revision. Removing a chapter affects the draft after confirmation; saved history remains. The title filter applies to the current page of up to 50 books.

History does not replace your draft. A confirmed restore creates a new revision. Text and Markdown exports include saved title and chapters, not private notes or change notes. Book projects are private to their account; Owner has no content override.

## Help and language

Find Help in the main menu or a subsystem menu. Select multiple topics to keep them on one page. Selected buttons are highlighted; select one again to remove that topic. The first topic opens automatically, and additional topics can be expanded independently. Leaving or fully refreshing the Help page resets this transient selection. Help supports safe Markdown-style headings, emphasis, lists, code and notes, not executable HTML.

My account provides an English JSON translation template and one saved custom language per account. Keep its keys and numbered placeholders unchanged; translate the values, set language name/code and left-to-right or right-to-left direction, preview and apply. Missing wording falls back to English. Importing a new file replaces only your custom language. Save other edits before switching language.

Translation files are limited to 512 KiB, 2,000 entries and 4,000 characters per translation. Invalid or unknown keys are rejected. This changes interface wording, not your documents or filenames. No paid translation service is used. Browser translation is optional and separate; review your browser provider's privacy behavior.

## Connection interruptions

Preserve unsaved text before refreshing. An uncertain request must not be blindly repeated: inspect the saved revision or operation result first. Reopening a page is not confirmation that the previous change succeeded.

[Return to overview](README.md)
