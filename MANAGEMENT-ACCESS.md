# Owner and Admin management access

This preview adds two system-wide options under account administration: **Require local/tunnel access for Owner** and **Require local/tunnel access for Admin**. Both default off. Owner alone changes them. Selecting a role blocks that role on public and LAN entry points, including HTTPS with a valid certificate. User accounts, temporary registration and permanent User conversion keep their existing behavior.

A restricted account still needs its normal password and an active, unexpired account. A management-issued session cannot be replayed on public entry points. Policy changes revoke sessions and pending mail access for the affected role, including the confirming Owner where applicable. Role changes also revoke sessions. Disabling restrictions never revives old sessions.

## Supported initial deployment

The initial management gateway supports a Linux operator host and Linux WebAI container with this release's qualified image labeled `webai.management-contract=unix-gateway-1`. Published v0.9.1 and existing preview images do not implement this contract. See RELEASE-QUALIFICATION.md for the exact distribution-image results. Do not use the following procedure against an older image.

The public application remains HTTP-first, behind the operator's chosen HTTPS gateway. Management uses separate application/mail Unix sockets inside a private directory and a distinct secret generated there. A foreground Python 3 helper, outside the application image, listens only on IPv4 loopback. The operator supplies SSH local forwarding. No public proxy routes to those sockets or gets the management secret. Localhost spelling, forwarded headers, private IPs and a bearer token alone do not satisfy the policy.

Default example origins:

| Route | Browser/SSH local port | Private upstream |
|---|---|---|
| Management application | `http://127.0.0.1:18480` | `/management/app.sock` |
| Management webmail | `http://127.0.0.1:18481` | `/management/mail.sock` |

These must differ from public application/mail origins and from other local listeners. The browser must retain the exact `127.0.0.1` host and ports; an embedded browser that rewrites them is not qualified. IPv6 loopback and arbitrary aliases are not accepted by this initial adapter. App and mail cookies have separate management namespaces because different ports alone do not isolate cookies.

## Operator sequence for a new qualified installation

These are explicit operator actions, not actions performed by Development or WebAI Setup. Use reviewed copies of `management-gateway.py` and `install-webai.py` from the same release. Choose a short absolute private directory, with no symlink components or shared writable parents. Keep the private directory out of source control, support exports and public proxy mounts.

1. Initialize a new directory using the host console. For the supported container application UID 999:
   ```sh
   sudo python3 management-gateway.py init --directory /srv/webai-mgmt --application-uid 999 --app-port 18480 --mail-port 18481
   ```
   Initialization refuses overwrite. The directory is mode 0700 and its `key.json` is mode 0600. This starts no listener and changes no firewall or trust store.
2. Create a **new**, qualified installation using the usual reviewed installer options and `--management-directory /srv/webai-mgmt`. The helper validates the image label, ownership and distinct origins, then explicitly mounts that directory at `/management`. It sets `AI_MANAGEMENT_DIR=/management`. The same directory contains the application and mail socket endpoints. The installer does not start the host gateway or create a tunnel.
3. Run the foreground gateway as an operator permitted to read the directory and connect to its sockets; for example a console command running as UID 999:
   ```sh
   sudo -u '#999' python3 management-gateway.py serve --directory /srv/webai-mgmt
   ```
   Keep the helper and its parent directories readable to that selected operator. No permanent service is installed. A separate `check` operation validates private configuration only; it does not prove a working tunnel or upstream.
4. On the user's computer, establish SSH **local** forwarding to the loopback listeners on the gateway host:
   ```sh
   ssh -NT -o ExitOnForwardFailure=yes -L 127.0.0.1:18480:127.0.0.1:18480 -L 127.0.0.1:18481:127.0.0.1:18481 USER@GATEWAY-HOST
   ```
   Add the user's own SSH key, port and host options. This is an example, not a change to any current forwarding. Never publish these management ports on a LAN/wildcard address or proxy them publicly.
5. Open `http://127.0.0.1:18480/workspace/login`, sign in as Owner, open account administration and choose the two role settings. Select **Verify management access**, review the effects and confirm. The one-use proof lasts five minutes and is bound to the Owner, exact selected settings and policy revision. Changing settings or waiting past expiry requires fresh verification. Sign in again after affected sessions are revoked. Open mail through the management application to use the separate mail handoff.

Do not retrofit this contract by adding only an environment variable to an already provisioned installation. Existing webmail configuration must also be deliberately provisioned for its private server; the one-time initializer refuses overwrite. Migration/restore needs an explicit operator plan and qualified target. This release does not retrofit an existing installation.

## Loss of management access

Keep authenticated server-console access available before enabling restrictions. Repair the helper/tunnel first where possible. There is no automatic public fallback and no HTTP recovery endpoint.

The local recovery command runs against the deployment's own private PostgreSQL socket while that database is available. From the authorized operator console, explicitly select `owner`, `admin` or `both`:

```sh
docker exec --user postgres WEBAI-CONTAINER /app/AI.Application --recover-management=owner --confirm-disable-restriction
```

Replace `WEBAI-CONTAINER` with the exact installation. This clears only the selected restriction, records a console recovery audit entry and revokes affected sessions. It does not reveal passwords, create accounts or change roles. A fresh login is required. Console/Docker access is already powerful administrative authority; the command is not a substitute for protecting that access.

## Operational limits

Preserve the database policy/audit, normal private state, private management directory, ownership and deployment mapping together during an operator-planned cold backup/recovery. Do not restore stale socket files as live endpoints. The existing `runtime_backup.py` intentionally refuses management-enabled installations because its format does not preserve this extra private mount. An automated management-aware backup/restore workflow has not been qualified.

The gateway bounds request bodies to 16 MiB, concurrent requests to 64 per application/mail listener and idle WebSocket relays to two minutes. Long-running connections may reconnect; authorization is checked again. This adapter does not claim unrestricted large-upload or long-idle-session qualification. Ordinary public routes retain their existing limits.

Operational records contain policy changes/revisions/actors and console recovery. Access-denial counters use two fixed rows rather than one unbounded record per public rejection. Credentials, session tokens and gateway secrets are not included in the report. Records are not tamper-proof against an administrator controlling the machine.

Developer verification, independent QA and actual SSH/HTTPS deployment acceptance remain distinct. These instructions do not configure the operator's gateway, forwarding or certificates automatically.
