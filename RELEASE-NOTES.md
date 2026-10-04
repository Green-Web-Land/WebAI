# WebAI version notes

## 0.8.0-next.1

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
