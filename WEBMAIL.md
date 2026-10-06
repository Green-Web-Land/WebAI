# Webmail

Webmail uses your existing mail service; WebAI does not create a mail server or
mailbox for you. This guide covers WebAI v0.9.2-preview.1. The older v0.7.1 download
does not include integrated webmail.

## Open your mailbox

1. Open **Office → Mailbox settings**. Under **Mail accounts**,
   select a named account or use **Add mail account**, then expand
   **Connection and credentials**. Existing settings remain your first account.
2. Enter that account's mail username and IMAP/SMTP hostnames. IMAP uses port
   993 with SSL/TLS. Choose SMTP 465 with SSL/TLS or 587 with required STARTTLS.
3. Enter both mail passwords privately and select **Save settings**. Saving does
   not test the password, connect to the mailbox, or send email.
4. Select **Open mail** to launch webmail in a separate browser tab. WebAI stays open.

The client handles reading and composing mail. Saved credentials alone do not
prove authentication works: open the mailbox to confirm the connection.
Do not repeatedly retry an uncertain password after a sign-in failure. Verify it
privately with your mail administrator or existing trusted mail client first.

## Secure access

Choose the appropriate [connection setup](HTTPS-SETUP.md) for WebAI and webmail:
HTTP using one IPv4 address and separate ports, either loopback-only or on your
chosen LAN address. HTTP does not encrypt browser traffic. Optional HTTPS belongs
to an external gateway managed by you, not the application image. No browser
hostnames or certificates are required for IP-only HTTP. WebAI does not change
your trust stores or hosts files. Do not ignore unexpected certificate warnings.
Mail-server certificate verification remains required independently of browser
trust. SMTP STARTTLS must complete before credentials are sent when that mode is
configured; it must not fall back to plaintext.

Only your WebAI account manages its saved mail credentials through the application.
Host/runtime administrators remain outside that application privacy boundary.
Passwords are never displayed again. Changing the destination or login identity
requires fresh passwords rather than silently reusing them for another account.
Administrative password recovery removes saved mail credentials; re-enter them
after recovery. See [Security boundaries](SECURITY.md).

## Connection and account limits

Each user can save up to 20 private named configurations.
Open mail selects one account, not a unified inbox.
Per-user server names must resolve to public IPv4 destinations. Private,
loopback, reserved and metadata-service addresses are rejected; IPv6-only
mail servers and internal mail servers are not supported by this mode yet.
Each connection is pinned to a validated address while the TLS certificate is
checked against the entered hostname. Plaintext and self-signed mail-server
certificates are not accepted. Saving does not verify the password.
A successful preview on one server does not qualify all providers or authentication methods.
Contacts and calendar integration with WebAI is separate from mail search.

## Search email

Open **Office → Search email**, select your private account, and load its folders.
Enter filters directly, or use **Interpret question** to turn a question into
editable filters using the local model. Review them, then select **Search**.
Interpretation receives only your question and fixed instructions—not messages,
headers, results, credentials or account identifiers. No paid fallback occurs.

Search is read-only. Results appear as an escaped table with subject, sender,
recipient, date and unread status. Subject links open messages in a new webmail
tab; opening a message there may mark it read, but searching does not.

The initial implementation searches one folder's latest 5,000 UID values, with
25 results per page. It does not claim complete historical coverage or semantic
analysis. Sender, recipient, subject and text use literal matching; **Since** is
inclusive and **Before** exclusive. Up to 200 folders are listed. Pages are fresh
queries, so changes in the mailbox can change the result order between pages.

A separate read-only MCP endpoint, `/mcp/mail-search`, exposes `mail_folders`
and `mail_search`. It requires the caller's current WebAI bearer session and
mailbox ownership. It provides no raw IMAP commands, SQL, writes, administrator
override, or message bodies. External MCP clients receive the returned headers;
their subsequent use of those headers is outside WebAI's question-only flow.

An unsuccessful connection/login stops search attempts for that saved-settings
revision. Verify the password and connection settings privately, then save the
corrected settings. Search never retries the login automatically.

[User guide](USER-GUIDE.md) · [Installation](INSTALLATION.md)
