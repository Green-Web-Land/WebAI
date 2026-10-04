# Connection setup for WebAI and webmail

Choose local-only HTTP or administrator-managed HTTPS. Ordinary users should
receive working URLs. Certificate trust is managed by the device owner, never
silently installed by WebAI.
WebAI remains self-hosted software; obtaining a certificate does not make it a
hosted service or require opening it to the public internet.

## Choose your installation method

| Method | Administrator prepares | What users need |
| --- | --- | --- |
| One computer, local-only HTTP | Loopback-only Docker port publication | Local WebAI URL; no browser certificate |
| Optional self-signed HTTPS | Two names, a unique certificate and private gateway routing | Manual certificate verification and trust |
| Your own HTTPS proxy | Two names, certificate coverage and private HTTP upstreams | Your configured WebAI URL |

The release includes an explicit new-container helper in [Installation](INSTALLATION.md).
These are deployment options, not an automatic setup wizard. Configure your own
ports and origins together; do not copy another installation's addresses or secrets.
This guide covers WebAI v0.9.0. Use the matching installer and image; do not mix release files.

## Local-only HTTP: no browser certificate

Use this mode only when the browser and Docker run on the same computer.
The names are `app.webai.localhost` and `mail.webai.localhost`; they keep the
application and mail cookies separate. Do not substitute a LAN address.

The local gateway listens on container port **8082**. Publish it only as
`127.0.0.1:18080:8082` (or another chosen host port), not `18080:8082` or
`0.0.0.0:18080:8082`. Do not publish the application, mail, database or bridge
backend ports. Do not use host networking. Your browser URLs are then:

- `http://app.webai.localhost:18080/workspace`
- `http://mail.webai.localhost:18080/start` (normally opened through **Open mail**)

The installation configuration must use `AI_BROWSER_MODE=local-http`, those exact
two origins with the same host port, and proxy peer `127.0.0.1`. The internal gateway
uses the installation's private proxy secret. Provision configuration before
starting the runtime; do not edit only one setting in an existing installation.

On **Windows**, run Docker with Linux containers and open the local URL on that
same Windows computer. On **macOS**, open it on the Mac running Docker. On
**Linux**, open it on the Linux computer running Docker. No hosts-file or trust-store
edit is part of this mode. If your browser or managed DNS policy does not resolve
these localhost names, stop and check that policy; do not change them to a LAN IP
or widen the Docker binding to work around it.

To inspect published ports, run `docker port <container-name>` in PowerShell
(Windows) or Terminal (macOS/Linux). For this mode, the only browser publication
should be `8082/tcp -> 127.0.0.1:18080` with your chosen port. A remote computer
must not be able to reach it. This is a configuration check, not automatic firewall
management. A compromised local computer remains outside this protection boundary.

Certificate-free refers to the **browser connection**. IMAP/SMTP still require
TLS and certificate validation. The internal mail connection uses a private,
authenticated local socket inside the container. It has no certificate to generate,
trust or renew and is not exposed on a network port.

## One gateway, two application names

Use separate names such as `webai.example.com` and `mail.example.com` (examples
only). One gateway can handle HTTPS for both. A single certificate may cover both
names, or the gateway may manage separate certificates. Both names must resolve
correctly from every client device, and each connection must present a valid
certificate for its requested name.

Keep application and mail upstream ports private. The gateway must preserve the
expected host and use the application's supported trusted-proxy configuration;
never accept arbitrary client-supplied HTTPS/proxy headers. Configure exact
application and mail origins together. Validate redirects, secure cookies, and
WebAI's live browser connection after setup.

The browser-facing certificate is separate from IMAP/SMTP certificates. Webmail
must also verify the mail server's identity and require the configured TLS mode.
Making the browser trust WebAI does not fix an untrusted external mail server.

## Network installation: your own HTTPS gateway

1. Ask the network administrator for two approved DNS names and private upstreams.
2. Install a certificate and full intermediate chain covering both names at the
   gateway. Keep private keys out of repositories, public downloads and logs.
3. Configure both application origins and supported proxy trust settings.
4. Verify browser access without warnings, then test sign-in and mail launch.
5. Assign responsibility for certificate renewal, monitoring and rollback.

No per-user hosts-file changes are needed when the network DNS resolves the names.

### Owned domain with automatic certificates

Use an ACME-capable gateway or certificate manager. For an internal-only server,
DNS-01 proves control of the domain through public DNS TXT records without exposing
the application to the internet. Configure client DNS to resolve the application
names to the intended private address. Certificate issuance and name resolution
are separate tasks.

Use narrowly scoped DNS credentials or delegated validation. Do not hand the
application unrestricted control over your domain. Automate renewal and gateway
reload, monitor expiry, and test renewal before relying on it. Public certificate
issuance may disclose hostnames through certificate transparency; do not put
confidential information in names.

See [Let's Encrypt DNS-01 guidance](https://letsencrypt.org/docs/challenge-types/#dns-01-challenge).
Public certificate authorities do not issue certificates for our private `.test`
preview names. Choose names under a domain you control for this option.

## Network installation: optional self-signed certificates

WebAI does not generate, install or renew certificates. If you choose a self-signed
certificate or private certificate authority, provision it using your own gateway
and your organization's tools. Configure coverage for both application and mail
names. Protect private keys outside the application and arrange renewal yourself.
A publicly trusted certificate is generally easier for users than manual trust.

Before trusting it, verify its fingerprint through a separate trusted channel,
its purpose, validity and ownership. Installing a CA grants trust to certificates
it signs, not only to one WebAI page. Obtain explicit device-owner or administrator
approval. Managed devices may receive trust through existing organizational policy.

- **Windows:** use the approved Windows certificate-management process for the
  intended user or machine scope. Do not silently install into machine-wide trust.
- **macOS:** use the approved Keychain or device-management process.
- **Linux:** use the distribution and browser's supported trust-store procedure.

Browser trust behavior varies: verify every supported browser after installation.
Provide removal instructions and a renewal/rotation plan. Docker cannot make a
certificate trusted on users' computers merely by installing it inside the image.

See [local certificate guidance](https://letsencrypt.org/docs/certificates-for-localhost/).

## Do users need to edit their hosts file?

**Normally, no.** Configure DNS once at the network level. A hosts-file entry is a
development fallback when DNS is unavailable; it affects only that device and
does not establish certificate trust. Containers' internal name mappings also do
not configure the user's browser.

Do not copy the preview's localhost mappings to another installation. Loopback
points to the computer running the browser, not automatically to the Docker server.

## Acceptance checklist

### Read-only configuration check

Run the supplied `check-browser-installation.py` helper against your container.
It reads Docker configuration, prints no passwords or proxy-secret values, and
does not modify the container, trust stores, hosts files, DNS or firewall.
Python 3 and the Docker command-line client must already be installed.

Windows PowerShell:

```powershell
py -3 .\check-browser-installation.py webai
```

macOS or Linux Terminal:

```sh
python3 ./check-browser-installation.py webai
```

Replace `webai` with your container name. An exit code of zero means the checked
configuration passed; it does **not** certify connectivity, certificate trust,
mail login or complete installation security. Fix reported errors before use.
Writable state at `/state` is required; it may live inside the container without
a mandatory external volume. Back up before replacing or deleting it. For an external HTTPS
gateway, verify that gateway and its private network separately.

### Functional checks

- Both names resolve to the intended installation from each supported client.
- Local HTTP: only the loopback gateway port is published, and both localhost URLs work.
- HTTPS: names and expiry are valid; trust is established manually or by your existing managed certificate policy.
- Gateway upstreams, database, model service and credential bridge are not public.
- IMAP/SMTP certificate verification remains enabled; STARTTLS is mandatory where configured.
- Open mail reaches the correct webmail service. The user performs the first real login.
- Authentication failure stops the test; do not repeatedly retry an uncertain password.
- No real email is sent as part of certificate checks.
- Sign-out/revocation behavior and renewal are verified, with recovery documented.

Never use “Continue unsafe” or disabled verification as an installation procedure.
Never ship a shared private key in a downloadable Docker image.

[Installation](INSTALLATION.md) · [Security boundaries](SECURITY.md)
