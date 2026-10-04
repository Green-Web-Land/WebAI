# Install WebAI

Run WebAI on your own computer or server, then use it in your browser. WebAI is downloadable software, not a hosted service.

These instructions match **WebAI v0.9.0**. Download its image parts, `DOWNLOAD-MANIFEST.json`, `WebAI-operator-helpers.zip` and third-party notices from [WebAI releases](https://github.com/Green-Web-Land/WebAI/releases). Corresponding third-party sources are supplied separately. Use only files listed in the matching release manifest; do not mix releases.

## Requirements

- Linux x64 Docker runtime. Windows and Intel Mac use Docker Desktop's Linux containers. No native ARM image is supplied; Apple silicon and Windows ARM are not qualified.
- Python 3.11 or newer for the operator helpers, not for the application runtime.
- Resources for a container limited to 6 GiB RAM, 4 CPUs and 256 PIDs, plus your operating system and other applications. Local-model speed depends on your hardware.
- Space for about 2.75 GB of parts, another 2.75 GB assembled archive, the expanded Docker image, data and backups.

Linux x64 fresh installation and recovery are tested. Windows/Mac commands are provided but those hosts have not been qualified end to end. This is an evaluation release, not a production-certified product.

## Windows

Install [Docker Desktop for Windows](https://docs.docker.com/desktop/setup/install/windows-install/) following its WSL 2 and virtualization requirements. Perform any requested computer restart yourself. Start Docker Desktop with **Linux containers**, and review its separate licence terms. Install [Python for Windows](https://www.python.org/downloads/windows/) 3.11 or newer.

Extract the helpers into the download folder beside the manifest and all image parts. Open PowerShell there:

```powershell
docker version
py -3 --version
py -3 .\release-parts.py join . .\webai-image.tar.gz
docker load --input .\webai-image.tar.gz
py -3 .\install-webai.py --name webai --enable-mail
```

If `py` is unavailable, use `python` after checking its version.

## Mac

On an Intel Mac, install and start [Docker Desktop for Mac](https://docs.docker.com/desktop/setup/install/mac-install/), review its licence terms, and install [Python for macOS](https://www.python.org/downloads/macos/) 3.11 or newer. Extract the helpers beside the downloaded parts and manifest. Open Terminal there:

```sh
docker version
python3 --version
python3 release-parts.py join . webai-image.tar.gz
docker load --input webai-image.tar.gz
python3 install-webai.py --name webai --enable-mail
```

Apple silicon requires amd64 emulation, including the local-model workload, and is not qualified. Use a Linux x64 host for evaluation instead.

## Linux

Install [Docker Engine for your distribution](https://docs.docker.com/engine/install/) on an x64 host and Python 3.11 or newer from your distribution's supported packages. Have your operator arrange Docker access; do not expose the Docker socket. Extract the helpers beside the parts and manifest, then run:

```sh
docker version
python3 --version
python3 release-parts.py join . webai-image.tar.gz
docker load --input webai-image.tar.gz
python3 install-webai.py --name webai --enable-mail
```

## Open your local installation

The installer creates a **new** container and refuses an existing name. It initializes the database, private internal mail identity and webmail configuration. `--enable-mail` explicitly enables installation mail access; it does not connect to a mailbox or send email. Each user supplies their own mail settings later.

Open [WebAI on this computer](http://app.webai.localhost:18080/workspace). **Open mail** launches `http://mail.webai.localhost:18080/start` in a separate tab. Only `127.0.0.1:18080` is published. These names keep app and mail cookies separate. No browser certificate, hosts-file edit, trust-store change, tunnel or automatic restart is performed.

Use these URLs on the computer running Docker. If managed browser/DNS policy prevents the localhost names resolving, ask your operator to investigate; do not widen the loopback binding or use a LAN address. Choose another local port with `--port 18090`, then use the printed URLs.

The container runs as its nonroot user, without privileged mode, a Docker-socket mount, automatic deletion or automatic restart. Internal database, model and credential-bridge ports are not published. Do not weaken these defaults.

If installation fails, preserve the named container and inspect `docker logs --tail 60 webai` privately. The helper does not reset partial data. Do not post unredacted logs or credentials.

## First Owner

Read the initial token in a private terminal:

```text
docker exec webai cat /state/preview.token
```

Open [first Owner setup](http://app.webai.localhost:18080/workspace/setup), enter the token privately, and choose an Owner username and unique password of 12–128 characters. Setup closes after the first Owner is created. This token is not a recovery bypass; never paste it into chat.

After signing in, use **System Configuration → Accounts** for optional signup, approval and verification codes. Signup starts disabled. Configure support SMTP before enabling email verification. Use **My account** for your email address and **Office → Mailbox settings** for your mail accounts. IMAP/SMTP require validated TLS even when the local browser connection uses HTTP. See [AI provider choices](AI-ASSISTANT.md); there is no automatic paid fallback.

## Your own HTTPS proxy

Use `--mode https` with exact application/mail origins and the proxy peer address seen by the container. For example, replacing all example values with your own:

```text
python3 install-webai.py --name webai --mode https --application-origin https://webai.example.com --mail-origin https://mail.example.com --proxy-peer 172.17.0.1 --port 18080 --mail-port 18081 --enable-mail
```

On Windows use `py -3`. **Do not assume `172.17.0.1` is your proxy peer.** The helper publishes both HTTP upstreams only on loopback and does not configure the gateway, DNS, firewall, tunnels or browser certificates.

The gateway routes WebAI to private port 18080 and mail to private port 18081. Preserve the configured Host, including any nondefault port; overwrite `X-Forwarded-Proto` with `https` and `X-WebAI-Preview-Proxy` with this installation's private proxy key. Support WebSocket upgrades on the WebAI route. Never accept these trusted headers from clients.

Retrieve the proxy key privately using `docker exec webai printenv AI_PREVIEW_PROXY_KEY`. Keep it only in protected gateway configuration, not public examples or logs. Both peer and key must match. Follow [connection setup](HTTPS-SETUP.md) for certificates and checks. Each operator-managed gateway needs its own qualification.

## Stop and reopen

```text
docker stop --timeout 45 webai
docker start webai
```

After a computer restart, start Docker, then the same container. State is stored inside that container without a mandatory external volume. **Deleting or replacing it is not an upgrade procedure. Back up first.**

## Cold backup and restore

Use the helper matching your release and a new private backup directory, restricted to the operator. On Windows inspect its Security/ACL settings; POSIX-style permissions alone do not establish Windows access protection.

```text
docker stop --timeout 45 webai
python3 runtime_backup.py backup webai PRIVATE_BACKUP_DIRECTORY
python3 runtime_backup.py restore PRIVATE_BACKUP_DIRECTORY webai-restored --port 18080
```

Replace the directory placeholder; Windows uses `py -3`. Local mail restore must use the original origin port, with the original container stopped. HTTPS restore also accepts `--mail-port 18081`; verify both private upstreams match your gateway. Recovery creates a new container and refuses overwrite. It preserves accounts, mail configuration, internal identities and required runtime settings. This tested path does not establish automatic migration from 0.7.1.

**The entire backup is secret, including `manifest.json`.** It includes private proxy configuration, sessions, database state, mail identities and encryption keys. Protect the archive and staging copy. External folders need separate backup. Restored sessions may remain valid; review and revoke access where appropriate.

Verify sign-in, documents, manuscripts, files and webmail before switching users or deleting the original container. Missing Files transfer evidence, unclean shutdown, unsupported mounts and partial installation are failures, not checks to bypass. Arbitrary database/schema upgrades are not promised.

## Upgrade a v0.8.0 operator installation

Stop and back up the original installation first. Keep that container and backup
unchanged for rollback. Use the v0.9.0 helpers together, including
`migrate-mail-ipc.py`, and the exact v0.9.0 image ID from its manifest:

```text
python3 runtime_backup.py restore PRIVATE_BACKUP_DIRECTORY webai-upgraded --port 18080 --image EXACT_V090_IMAGE_ID --migrate-mail-ipc
```

Windows uses `py -3`; macOS and Linux use `python3`. Replace both placeholders.
Local mode requires the original origin port; HTTPS mode also requires the matching
private mail upstream port. The operation creates a new container, converts only
its copied internal mail configuration and creates a private IPC key. It neither
modifies the source backup nor manages browser certificates. Legacy certificate
files retained in the copied state are no longer used by the internal bridge.

This migration supports complete v0.8.0 installations created by the operator
helper. Custom entrypoints, incomplete installations and v0.7.1 upgrades are not
covered. Verify your data and mail access before switching users; retain the
original for rollback. Do not run both containers against the same external files
or gateway endpoints simultaneously.

## Internal mail connection

In WebAI v0.9.0, the internal mail bridge uses an authenticated private local socket inside the container. It creates no internal certificates and has no certificate-expiry maintenance. Preserve its private key with the application state and backups. Never expose the bridge or copy its key into public configuration. Outbound IMAP/SMTP connections still validate the mail server's certificate. Browser HTTPS, when used, belongs to your external proxy and is managed by you.

## Verified scope

Linux x64 installer/recovery qualification passed 12 synthetic checks, including fresh readiness, loopback-only publication, account login, no internal certificate directory, cold backup and restored mail-key/configuration preservation. Seven copied-backup upgrade checks verified explicit migration, retained Owner login, restart stability and an unchanged source backup. Five browser smoke checks verified login, footer, Mailbox settings, refresh persistence and About build details. No real mailbox was contacted during these checks. These results do not qualify Windows/macOS hosts or every operator proxy configuration.

Checksums detect corruption, not replacement of both file and manifest. Obtain files from the official release. See [evaluation limits](EVALUATION.md), [security boundaries](SECURITY.md), and [licence](LICENSE.md).

[Return to overview](README.md)
