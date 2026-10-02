# Installation and recovery

Run WebAI on your own computer with Docker, then open it in your browser. Choose the instructions for your operating system below. You do not need to install a separate database or local AI service.

**Download status:** A software download is not yet available. These instructions describe the packaged demo and require its image archive and matching operator helpers. Obtain those files from [WebAI releases](https://github.com/Green-Web-Land/WebAI/releases) when available; do not substitute an unrelated image.

## Runtime design

The container is Linux x64 (`linux/amd64`) and includes the application, a relational database and a local AI model in one image. Windows and Mac hosts run this Linux container through Docker Desktop; this is not a native Windows or Mac application. Linux x64 is the exercised platform. Windows and Mac installation have not yet been verified end to end. There is no native ARM image.

The combined candidate was exercised with 6 GiB container memory, no additional swap allowance, 4 CPUs and 256 PIDs. One offline CPU answer took about 32 seconds. This single check is not a throughput benchmark or a minimum-host guarantee; leave memory and CPU capacity for Docker, the operating system and other applications. Hosts with 16 or 32 GB RAM still need adequate free resources.

The image contains the software and model, not a backup of your changed runtime data. State lives inside the container without a mandatory mounted data volume. Deleting or replacing that container does not automatically preserve it. Back up and restore before replacement.

## Prepare your download

Download every image part, `DOWNLOAD-MANIFEST.json` and the operator helpers from the same release. Extract the helpers and place `release-parts.py` beside the manifest and parts. Keep `runtime_backup.py` for recovery. Install Python 3.11 or newer for these helpers; Python is an operator tool, not an application dependency. Allow disk space for the parts, assembled archive, loaded image and your runtime data.

## Windows

1. Install [Docker Desktop for Windows](https://docs.docker.com/desktop/setup/install/windows-install/) on a supported x64 Windows computer. Follow its WSL 2 and virtualization prerequisites. If setup requests a restart, restart the computer yourself before continuing.
2. Start Docker Desktop and use **Linux containers**, not Windows containers. Docker Desktop has its own license terms, separate from WebAI.
3. Install [Python for Windows](https://www.python.org/downloads/windows/) if needed. Open PowerShell in the folder containing your downloaded parts and helper.
4. Check Docker and Python, assemble the archive, then load it:

```powershell
docker version
py -3 --version
py -3 .\release-parts.py join . .\webai-image.tar.gz
docker load --input .\webai-image.tar.gz
```

Use Python 3.11 or newer. If `py` is unavailable but `python --version` reports a suitable version, use `python` instead. Continue with **Start WebAI** below. Windows on ARM is not qualified for this image.

## Mac

1. Install [Docker Desktop for Mac](https://docs.docker.com/desktop/setup/install/mac-install/), selecting the installer for your Mac processor, and start it. Review Docker Desktop's own license terms.
2. Install [Python for macOS](https://www.python.org/downloads/macos/) if `python3 --version` does not report 3.11 or newer.
3. Open Terminal in the download folder and run:

```sh
docker version
python3 --version
python3 release-parts.py join . webai-image.tar.gz
docker load --input webai-image.tar.gz
```

Continue with **Start WebAI** below on an Intel Mac. **Apple silicon:** the current image is not native ARM. Running an amd64 image would require emulation and is not qualified for WebAI, particularly its local AI workload. Use a Linux x64 host instead for evaluation; do not assume Mac support from a successful image load alone.

## Linux

1. Use an x64 Linux host. Install [Docker Engine using the instructions for your distribution](https://docs.docker.com/engine/install/) and start its service. Have your operator arrange Docker access; do not make the Docker socket publicly accessible.
2. Install Python 3.11 or newer using your distribution's supported packages if needed.
3. Open a terminal in your download directory and run:

```sh
docker version
python3 --version
python3 release-parts.py join . webai-image.tar.gz
docker load --input webai-image.tar.gz
```

If Docker requires elevated access, have the authorized operator perform these commands. Continue below.

## Start WebAI

These commands are for a **local evaluation on your own computer**. Docker must have enough available memory and CPU capacity for the resource limits below, in addition to the host's needs. In the following single-line command, replace `IMAGE_TAG_FROM_RELEASE` with the exact tag reported by `docker load` and documented for that release. The command works in PowerShell and Linux or Mac terminals:

```text
docker run -d --name webai --platform linux/amd64 --cap-drop ALL --security-opt no-new-privileges --memory 6g --memory-swap 6g --cpus 4 --pids-limit 256 -p 127.0.0.1:18500:8080 -e AI_BLAZOR_PREVIEW=1 -e AI_UI_PRIVATE_HTTP=1 IMAGE_TAG_FROM_RELEASE
```

The release image supplies its nonroot runtime user. Do not add `--privileged`, mount the Docker socket, or expose internal database/model ports. Do not add `--rm`: removing the container would remove its internal state. The startup command deliberately has no automatic restart policy.

Check startup with `docker ps --filter name=webai` and open [WebAI on this computer](http://127.0.0.1:18500/workspace). Wait for healthy status before setup. For startup failures, inspect `docker logs --tail 60 webai` locally; do not post unredacted logs containing private information.

HTTP is limited to loopback for this evaluation. Do not replace `127.0.0.1` with a public address. Network access requires a separately secured and verified HTTPS configuration. Do not use real paid-provider credentials or sensitive data in an unqualified deployment. WebAI is downloadable software, not a hosted service.

The assembly helper verifies part and complete-archive checksums and refuses to overwrite the destination. Checksums detect corruption, not a malicious replacement of both files and manifest. Loading does not start a container; do not substitute `docker import`.

## First Owner

On your private terminal, retrieve the initial setup token from your newly created container:

```text
docker exec webai cat /state/preview.token
```

Open [first Owner setup](http://127.0.0.1:18500/workspace/setup), enter the token privately and choose a unique Owner username and a password of 12–128 characters. Do not include the token in reports or chat. Setup closes after the first Owner is created and is not a password-recovery bypass. Sign in with that account afterward.

Use **System → Configure workspace** to choose your menu contents. Administrative entries remain role restricted. **System Administrator** is a separate planned group for future subsystems, not an installed feature. For provider choices, follow [AI Assistant](AI-ASSISTANT.md).

## Stop and reopen

```text
docker stop --timeout 30 webai
docker start webai
```

Starting the same container retains its data. After restarting your computer, start Docker first, then run `docker start webai`. Deleting or recreating the container is different: follow backup and restore before replacement.

## Cold backup and restore

The release must include its matching recovery helper. The existing candidate workflow is:

```text
docker stop --timeout 30 CONTAINER
python3 runtime_backup.py backup CONTAINER NEW_PRIVATE_BACKUP_DIRECTORY
python3 runtime_backup.py restore BACKUP_DIRECTORY NEW_CONTAINER --port UNUSED_PRIVATE_PORT
```

Run from the extracted helper directory, replacing each uppercase placeholder with your actual container, directory or unused port. On Windows use `py -3` instead of `python3`. Use the matching helper supplied with your software version. It refuses running or unclean backup sources and validates the archive before restoring into a new container. Restore is not an in-place overwrite. Verify readiness, account access, documents, manuscripts and file operations before switching users or deleting the old container.

Backups contain database state, accounts, sessions, internal files and the encryption keyring. Treat them as sensitive credentials and data. Optional mounted folders are excluded and need separate backup and permission planning. Restoring a backup can restore sessions and provider-key state; review and revoke them where appropriate.

Clean shutdown creates Files transfer evidence. Altered or missing evidence must not be bypassed. Old invalidated operation receipts are not repaired by copying database rows. Arbitrary database-version or schema upgrades are not promised.

## Current limitations

The documented command passed eight checks in a new, empty Linux Docker container: readiness, setup page access, first Owner creation, setup-token invalidation, account authentication, password sign-in, readiness after stop/start, and retained account/session access. This does not qualify every host configuration. Windows/Mac installation, forgotten-Owner recovery and network-facing HTTPS/proxy behavior are not yet fully verified. Do not use this demo for critical or production data. See [evaluation limitations](EVALUATION.md).

[Return to overview](README.md)
