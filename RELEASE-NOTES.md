# WebAI version notes

## WebAI v0.9.1

- HTTP-first installation uses a literal IPv4 address and separate WebAI/webmail ports.
- Choose local-only binding or a specific network address. No browser hostname,
  hosts-file edit or browser certificate is required.
- Webmail opens in a separate tab; WebAI stays open. Session cookies use distinct
  port-specific names, with backend cookie filtering and exact-origin checks.
- Browser certificate management is outside the application image. An optional
  external HTTPS gateway is operated by you.
- Outbound IMAP/SMTP and AI-provider TLS verification remains enabled.
- Footer: **WebAI v0.9.1**. About keeps detailed build information for support.

Use v0.9.1 helpers and the exact image ID from its matching manifest. Do not use
these instructions with the published v0.9.0 image. General conversion of an
existing hostname-based installation is not supplied as an automatic upgrade.
Back up and retain your existing installation before planning a replacement.

## WebAI v0.9.0

- Local-only browser access needs no certificate; network HTTPS belongs to your own proxy.
- WebAI does not generate, install or renew deployment certificates or change hosts files or trust stores.
- Internal mail authentication uses an owner-only local socket and key, with no internal certificate-expiry dependency.
- Outbound IMAP/SMTP and AI-provider TLS validation remains enabled.
- The footer displays **WebAI v0.9.0**; About retains technical build information for support.
- An explicit copied-backup migration supports v0.8.0 operator installations, preserving the original for rollback. See [Installation](INSTALLATION.md).

## WebAI v0.8.0 (archive tag: v0.8.0-next.1)

Build identifier: `0.8.0-next.1+20261004.deployment.1`. The footer and About page display the running build rather than a generic preview label.

### Find, save and revisit

- Read-only Data Search for Document Library, Write a Book, Files and Folders, and email.
- Local models interpret search questions; application code performs searches and formats results without sending retrieved content to the model.
- Optional private and administrator-managed OpenAI search connections for supported scopes, with explicit paid consent and no automatic paid fallback.
- Saved questions, dated result snapshots, Markdown/text exports and browser printing. Access is checked again when saved results are viewed or exported.
- Functional MCP search tools, not arbitrary SQL or shell execution.

### Email and accounts

- Multiple private IMAP/SMTP configurations per user; webmail opens in a separate tab.
- Email search within the selected account and folder, with documented coverage limits.
- Account email addresses, optional User-only signup, administrator approval and optional email verification codes.
- Administrator-managed support SMTP; optional support IMAP is reserved for future incoming-mail features.
- Local-only browser HTTP or administrator-managed HTTPS. WebAI does not edit hosts files or install certificate trust.

### A clearer workspace

- Alphabetized groups and ordinary submenus, with Help and AI Assistant afterward.
- Separate Documents and Office groups; Files and Folders under System Administration.
- **Search Document Library** combines search controls, Saved questions and Saved results.
- Books and Files keep saved-search panels on their Data Search pages; their redundant Saved Searches submenu links are removed.
- Switching Books/Files search pages and browser Back/Forward selects the correct subsystem and clears the previous page's transient search state.

### Scope and compatibility

The earlier 0.7.1 download does not contain these additions. Use an image and operator configuration matching your intended version. Do not delete an existing runtime container before verifying a recoverable backup and data transfer.

This is an evaluation build with developer testing, not independent production certification. See [evaluation coverage](EVALUATION.md), [search limits](AI-ASSISTANT.md), [webmail](WEBMAIL.md) and [security](SECURITY.md).

[Overview](README.md) · [User guide](USER-GUIDE.md)
