# WebAI Setup 0.1.0-preview.1

This Linux x64 pre-release is a read-only installation-environment inspector. It displays available capabilities, previews a non-executable example work order, and exports a sanitized report. Install, update, migrate, repair, uninstall, elevation, certificate changes, LAN access and self-update are unavailable in this prototype.

[Download this Setup pre-release](https://github.com/Green-Web-Land/WebAI/releases/tag/webai-setup-v0.1.0-preview.1) · [WebAI overview](README.md)

Build identity: `0.1.0-preview.1+20261006.p1.1`. Two download formats contain the same application.

## Self-contained executable

Download `WebAI.Setup-0.1.0-preview.1-linux-x64`, verify it against `SETUP-MANIFEST.json` and `SHA256SUMS.txt`, and run as your ordinary user:

```sh
chmod u+x WebAI.Setup-0.1.0-preview.1-linux-x64
./WebAI.Setup-0.1.0-preview.1-linux-x64
```

No separately installed .NET runtime is required. Bundled native components extract into the user's private directory at startup. A compatible glibc Linux environment and native runtime dependencies are required; qualification used the documented Debian-based Linux x64 fixture. Other distributions, Windows, macOS and ARM are not qualified.

## Framework-dependent package

Extract the complete `WebAI.Setup-0.1.0-preview.1-linux-x64-framework-dependent.zip` into its own directory. Keep the DLL, runtime configuration, dependency manifest, static assets and accompanying files together. Install compatible **Microsoft.NETCore.App and Microsoft.AspNetCore.App 10.0.12** shared frameworks yourself. Both are required; the full SDK is optional.

```sh
dotnet --list-runtimes
dotnet WebAI.Setup.dll
```

The package requests version 10.0.12. A later compatible 10.0 patch can satisfy runtime patch roll-forward; 10.0.12 was verified. An unrelated major runtime is insufficient. To restrict selection to the installed 10.0 patch line, set `DOTNET_ROLL_FORWARD=LatestPatch` when launching. With missing frameworks, the .NET host reports “You must install or update .NET” and identifies the required framework/version. This helper does not install or maintain your runtime.

## Private local session

The helper listens only at `http://127.0.0.1:18760`. Its connection page does not grant access. Open the private `launch-*.txt` record created in your per-user `WebAI.Setup` state directory, then open that launch link in your browser. Treat it as a credential; do not share or publish it. `SETUP_STATE` can select a dedicated private directory owned by your user (mode 0700); launch records use mode 0600. The helper does not print the secret link into ordinary logs.

Use the exact address and port above. Host/origin checks reject other addresses. Session cookies are HttpOnly and SameSite Strict. Launch capabilities are single-use and expire after five minutes; sessions expire after 15 minutes idle or eight hours total. End session or restart the helper to revoke access. Stop the helper before switching download formats; both share the same port and state-lock rules. Do not expose this prototype to the LAN.

Docker inspection is optional and reads only allowlisted metadata from a local Unix socket when your account already has access. A missing or inaccessible daemon is reported explicitly; no environment changes are attempted. Do not run the helper with elevated privileges merely to make an inspection succeed. Saved support reports exclude raw environment/configuration secrets; inspect any report before sharing it.

## Verification and limits

Developer verification covered 36 shared synthetic adapter/access checks, 71 Chromium/Firefox checks per format, and four actual process-restart checks per format. The framework-dependent package ran with both 10.0.12 shared frameworks and no SDK; a separate missing-framework fixture produced the expected diagnostic. The self-contained package ran without installed .NET. The application DLL was byte-identical across formats and found intact inside the bundled executable.

This does not establish independent QA, user acceptance, installer lifecycle behavior, real-host Docker management, broader operating-system compatibility or full accessibility qualification. Checksums establish integrity, not publisher signatures. See the accompanying licence and `WebAI.Setup-0.1.0-preview.1-NOTICES.tar.gz` for component notices.
