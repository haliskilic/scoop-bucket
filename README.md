# haliskilic's Scoop bucket

[![Tests](https://github.com/haliskilic/scoop-bucket/actions/workflows/ci.yml/badge.svg)](https://github.com/haliskilic/scoop-bucket/actions/workflows/ci.yml) [![Excavator](https://github.com/haliskilic/scoop-bucket/actions/workflows/excavator.yml/badge.svg)](https://github.com/haliskilic/scoop-bucket/actions/workflows/excavator.yml)

A [Scoop](https://scoop.sh) bucket for
[ser2netweb](https://github.com/haliskilic/ser2netweb), a serial-to-network bridge with a web
UI (raw TCP, telnet, RFC 2217, UDP, Modbus gateway, MQTT). ser2netweb is an independent
project, unrelated to Corey Minyard's ser2net.

## Install

```pwsh
scoop bucket add haliskilic https://github.com/haliskilic/scoop-bucket
scoop install haliskilic/ser2netweb
ser2netweb                      # the first run asks for the web UI's address
```

Update with `scoop update ser2netweb` (after `scoop update`, which fetches the bucket).

- The manifest installs the release's own `ser2netweb-windows-x64.exe` (64-bit Windows 10/11
  or Windows Server 2016 and newer), checked against its SHA-256, as `ser2netweb.exe` on
  `PATH`.
- ser2netweb keeps its data (`config.json`, backups, logs, traces) in `data\` next to the
  exe, which Scoop persists: `~\scoop\persist\ser2netweb\data`. Updates keep it;
  `scoop uninstall --purge ser2netweb` deletes it.
- As a Windows service: `ser2netweb --install-service` from an elevated prompt. The
  service runs its own copy in `C:\Program Files\ser2netweb` (data in
  `C:\ProgramData\ser2netweb`), so run `ser2netweb --install-service` again after each
  `scoop update ser2netweb`, and `ser2netweb --uninstall-service` before
  `scoop uninstall ser2netweb`. More in the
  [Windows guide](https://github.com/haliskilic/ser2netweb/blob/main/docs/WINDOWS.md).

## How the manifest stays current

`.github/workflows/excavator.yml` runs every day (and by hand): Scoop's Excavator checks
ser2netweb's latest GitHub release (`checkver`), and for a new one rewrites
`bucket/ser2netweb.json` with its URL and the SHA-256 from the release's `SHA256SUMS`
(`autoupdate`), then commits it here with the workflow's own `GITHUB_TOKEN`. The
ser2netweb repository keeps a copy of the manifest in `packaging/scoop/ser2netweb.json`,
made by `tools/windows_channels.py scoop vX.Y.Z` and checked against Scoop's schema there.

Problems with ser2netweb itself go to
[its issues](https://github.com/haliskilic/ser2netweb/issues); a hash or install problem of
this bucket's manifest here.
