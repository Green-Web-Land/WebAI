# Install WebAI v0.9.2-preview.1

This guide applies to **v0.9.2-preview.1**; do not apply it to v0.9.0 downloads.
Use the matching image, manifest and operator helpers from [WebAI releases](https://github.com/Green-Web-Land/WebAI/releases).
WebAI is self-hosted software, not a hosted service.

## Requirements

- Linux x64 Docker runtime and Python 3.11 or newer for operator helpers.
- Windows and Intel Mac: Docker Desktop with Linux containers. Native ARM and
  Apple silicon emulation are not qualified.
- Allow 6 GiB RAM and 4 CPUs for the container, plus resources for your host.
- Allow space for downloaded parts, the joined archive, Docker image, user data
  and backups. Check the release manifest for actual download sizes.

Linux x64 is the qualified runtime. Windows/macOS commands are supplied, but their
Docker-host installation has not been tested end to end. Review Docker's separate
licence and installation requirements. Perform any host restart yourself.

## Windows

Install [Docker Desktop](https://docs.docker.com/desktop/setup/install/windows-install/)
and [Python](https://www.python.org/downloads/windows/). Start Linux containers.
Extract the matching operator helpers beside all downloaded image parts and
`DOWNLOAD-MANIFEST.json`. Open PowerShell there:

```powershell
docker version
py -3 --version
py -3 .\release-parts.py join . .\webai-image.tar.gz
docker load --input .\webai-image.tar.gz
py -3 .\install-webai.py --image EXACT_IMAGE_ID --name webai --enable-mail
```

Replace `EXACT_IMAGE_ID` with the complete `sha256:...` image ID in the matching
manifest. If `py` is unavailable, use `python` after checking its version.

## macOS

On an Intel Mac, install [Docker Desktop](https://docs.docker.com/desktop/setup/install/mac-install/)
and [Python](https://www.python.org/downloads/macos/). Extract helpers beside the
parts and manifest. In Terminal:

```sh
docker version
python3 --version
python3 release-parts.py join . webai-image.tar.gz
docker load --input webai-image.tar.gz
python3 install-webai.py --image EXACT_IMAGE_ID --name webai --enable-mail
```

Replace `EXACT_IMAGE_ID` with the full release image ID. Apple silicon is not qualified.

## Linux

Install [Docker Engine](https://docs.docker.com/engine/install/) on an x64 host
and Python 3.11 or newer. Arrange Docker access without exposing the Docker socket.
Extract matching helpers, parts and manifest into one folder:

```sh
docker version
python3 --version
python3 release-parts.py join . webai-image.tar.gz
docker load --input webai-image.tar.gz
python3 install-webai.py --image EXACT_IMAGE_ID --name webai --enable-mail
```

Replace `EXACT_IMAGE_ID` with the full release image ID.

## Local-only access

The default publishes two loopback ports:

- [WebAI](http://127.0.0.1:18080/workspace)
- Webmail: `http://127.0.0.1:18081/start`, normally opened using **Open mail**.

No browser hostname, hosts-file entry or certificate is required. Change ports
together using `--port 18090 --mail-port 18091` if needed. Local-only URLs work
on the computer running Docker, not another computer.

The installer creates a new container and refuses an existing name.
`--enable-mail` enables installation mail access; it does not authenticate,
read or send mail. Each user configures their own mailbox later.
The nonroot container has no privileged mode, Docker socket mount, automatic
deletion or automatic restart. Database, model and internal bridge ports stay private.

## LAN access

Choose a specific IPv4 address assigned to the Docker host and two unused ports:

```text
python3 install-webai.py --image EXACT_IMAGE_ID --name webai --bind 192.168.1.20 --port 18080 --mail-port 18081 --enable-mail
```

Windows uses `py -3`. Replace the example address. Open
`http://192.168.1.20:18080/workspace`; **Open mail** opens port 18081 in a new tab.
No hosts-file edits are required. You manage firewall access and any forwarding.
For user-managed forwarding, provide both `--application-origin` and
`--mail-origin` with the client-reachable IPv4 address and these same ports.
The installer does not build tunnels or configure host services.

HTTP exposes credentials, sessions and content to network interception and
modification. Use it only where you accept those risks. An external HTTPS gateway
is recommended for untrusted/shared networks and public access.
See [connection choices](HTTPS-SETUP.md) and [security boundaries](SECURITY.md).

## First Owner and mail settings

Privately read the initial setup token:

```text
docker exec webai cat /state/preview.token
```

Open `http://127.0.0.1:18080/workspace/setup`, or your configured LAN address,
and create the first Owner with a unique password of 12–128 characters.
Never share the setup token. Setup closes after the first Owner exists; it is
not a password-recovery bypass.

Use **System Configuration → Accounts** for signup policy, approval and optional
email verification codes. Signup begins disabled. Configure support SMTP before
requiring email verification. Each user configures mail through
**Office → Mailbox settings**, then selects **Open mail**.
IMAP/SMTP still require validated TLS. Browser HTTP does not disable outbound
TLS checks. See [AI providers](AI-ASSISTANT.md); paid fallback is not automatic.

## Optional external HTTPS

Certificates and gateway operation are outside the application image.
Use your own gateway; optional self-signed tooling, if supplied separately,
is never run automatically by WebAI. WebAI does not install trust or edit hosts.

The supported external-gateway mode uses two HTTPS origins and private upstreams:

```text
python3 install-webai.py --image EXACT_IMAGE_ID --name webai --mode https --application-origin https://webai.example.com --mail-origin https://mail.example.com --proxy-peer PROXY_PEER_IP --port 18080 --mail-port 18081 --enable-mail
```

Replace every placeholder. Upstreams publish on loopback only. The gateway must
preserve the configured Host, support WebSockets and overwrite trusted transport
headers. See [external gateway details](HTTPS-SETUP.md). This optional mode does
not make HTTPS or hostnames a requirement for ordinary IP-only installation.

## Stop, reopen and recover

```text
docker stop --timeout 45 webai
docker start webai
```

State belongs to the container. Deleting/replacing it is not an upgrade procedure.
After a host restart, start Docker and the same container.

The supplied backup helper refuses management-enabled installations. Preserve their additional private directory and mapping through an operator-reviewed recovery plan; automated management-aware backup/restore is not qualified. For an ordinary installation without management mode, stop it first; choose a new operator-private directory:

```text
python3 runtime_backup.py backup webai PRIVATE_BACKUP_DIRECTORY
python3 runtime_backup.py restore PRIVATE_BACKUP_DIRECTORY webai-restored --port 18080 --mail-port 18081
```

Windows uses `py -3`. For LAN restore include the original specific `--bind`
address. Keep the original container stopped and retain the original origin ports.
The helper refuses overwriting existing containers. Treat the entire backup,
including its manifest, as secret: it contains sessions, credentials and keys.
External folders need separate backups. Verify your data before retiring the
original. Do not run both copies against the same external files.

A general automatic conversion of an older hostname-based installation to IP-only
mode is not supplied. Use that release's recovery instructions and retain its
rollback data; arrange a reviewed conversion rather than editing isolated settings.
The private preview conversion does not establish a general upgrade guarantee.

If startup fails, preserve the container and privately inspect
`docker logs --tail 60 webai`. Do not share unredacted logs.

## Optional Owner/Admin management and public demo

Use [management access](MANAGEMENT-ACCESS.md) for a new Linux installation with separate private ingress. Read [demo and hosting](DEMO-AND-HOSTING.md) for temporary User registration and the public Home introduction. WebAI Setup is a separate read-only prototype; it does not execute this installation.

## Verification and limits

See RELEASE-QUALIFICATION.md for checks on this exact candidate. Earlier installation/recovery and provider observations are predecessor evidence. No general in-place upgrade, old-version migration, Windows/macOS host installation or arbitrary proxy configuration is qualified by this preview.

Checksums detect corruption, not replacement of both file and manifest.
Get files from the official release. Read [evaluation limits](EVALUATION.md),
[licence](LICENSE.md) and [version notes](RELEASE-NOTES.md).

[Return to overview](README.md)
