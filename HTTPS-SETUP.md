# Connection setup for WebAI and webmail

WebAI v0.9.2-preview.1 defaults to HTTP with one IPv4 address and separate ports.
Browser hostnames and certificates are not required. HTTPS is an optional,
operator-managed layer outside the application image.

| Access | WebAI | Webmail |
| --- | --- | --- |
| Local-only default | `http://127.0.0.1:18080/workspace` | `http://127.0.0.1:18081/start` |
| LAN example | `http://192.168.1.20:18080/workspace` | `http://192.168.1.20:18081/start` |
| Optional external HTTPS | Your configured application origin | Your configured mail origin |

Choose installation arguments from [Installation](INSTALLATION.md).
Use a specific host IPv4 address, not wildcard publication. WebAI does not
configure your firewall, tunnels, forwarding, DNS, hosts files or trust stores.

## Local and LAN HTTP

Use **Open mail** to open webmail in a separate tab. The application gateway
publishes container port 8082 and the mail gateway publishes 8083. The installer
maps these to your chosen host ports. Internal application/mail backends,
database, models and credential bridge must stay private.

On Windows, macOS and Linux, IP-only access needs no hosts-file edit.
`127.0.0.1` refers to the computer running the browser; use the server's reachable
IP for another computer. Configure advertised origins together with your ports.
Changing only a URL or only a port in an existing installation is not a migration.

HTTP does not encrypt or authenticate the browser-to-server connection. Network
observers or attackers may intercept or modify passwords, sessions and content.
Use an external HTTPS gateway for networks where you do not accept that risk.

Ports separate browser origins but **not cookie scope**. WebAI and its bundled
webmail remain mutually trusted on the same IP. Port-specific session names,
cookie forwarding allowlists and exact-origin checks are implemented; do not
assume these isolate an unrelated untrusted service hosted on another port.

## Optional operator-managed HTTPS

WebAI does not generate, load or renew browser certificates. A self-signed
certificate, if desired, belongs to a separate gateway, not the application image.
Trust decisions and renewal belong to the operator/device owner. No tool
automatically edits trust stores or hosts files.

The supported `--mode https` configuration uses separate HTTPS origins and private
loopback HTTP upstreams. The gateway must:

- Preserve each configured Host, including a nondefault port.
- Overwrite `X-Forwarded-Proto` with `https`.
- Overwrite `X-WebAI-Preview-Proxy` with the installation's private proxy key.
- Support WebSocket upgrades for WebAI.
- Match the configured proxy peer and prevent clients from supplying trusted headers.

Privately retrieve that key with `docker exec webai printenv AI_PREVIEW_PROXY_KEY`.
Never put it in public examples, logs or chat. Route app and mail to the private
upstream ports printed by the matching installer. Do not assume a Docker peer
address from another host. Qualify your own gateway before relying on it.

The optional hostname-based HTTPS mode does not impose hostnames on ordinary
IP-only HTTP users.

## Outbound mail is separate

IMAP/SMTP require validated TLS; mandatory STARTTLS remains mandatory.
Accepting browser HTTP never disables validation of an external mail server.
The internal credential bridge uses an authenticated local socket, not a public
network listener or a certificate-based service.

## Verify your installation

On Windows:

```powershell
py -3 .\check-browser-installation.py webai
```

On macOS or Linux:

```sh
python3 check-browser-installation.py webai
```

The checker reads Docker configuration without changing it or printing secrets.
A pass does not establish mail authentication, network reachability or complete
security. Also check:

- Both configured IP ports are reachable from intended clients.
- Local-only installations are not remotely reachable.
- Open mail opens the correct mailbox in a new tab without replacing WebAI.
- The user performs the first real mail login; stop on an uncertain-password error.
- Sign-out/revocation prevents continued mail access.
- Database/model/bridge ports remain private.
- Optional HTTPS gateways have separately verified identity and trust.

Do not send real email merely to test browser transport. Keep private backups
before any replacement. See [security boundaries](SECURITY.md).
