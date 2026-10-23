# Meterkat companion releases

Meterkat shows your Claude, Codex and Antigravity usage limits in an Android widget and notification. The **desktop companion** runs on the computer where those tools are signed in and sends end-to-end encrypted snapshots to your phone. Website and setup help: **https://meterkat.fabian-duerkop.de**.

This repository only hosts the signed companion binaries. The source code is open source under the MIT licence at **[Fade97/meterkat-companion](https://github.com/Fade97/meterkat-companion)**, whose release workflow builds these files (from 1.0.2 on with a build-provenance attestation, see below). Licence: [MIT](LICENSE); 1.0.0 and 1.0.1 still embed the earlier freeware terms but are now MIT-licensed too. The hosted relay the companion uses by default is a separate free service under its [terms of use](https://meterkat.fabian-duerkop.de/terms).

## Download

Open the [latest release](https://github.com/Fade97/meterkat-releases/releases/latest) and pick your file:

| System | File |
| --- | --- |
| Windows 10/11 (x64) | `meterkat-companion-<version>-windows-x64.exe` |
| Linux x64 (glibc 2.28+) | `meterkat-companion-<version>-linux-x64.tar.gz` (or the bare binary) |
| Linux arm64 (glibc 2.28+) | `meterkat-companion-<version>-linux-arm64.tar.gz` (or the bare binary) |

## Set up

- **Windows:** in PowerShell in the download folder, run `.\meterkat-companion-<version>-windows-x64.exe setup`. The build is not code-signed. If SmartScreen says "Windows protected your PC", choose **More info → Run anyway**.
- **Linux:** `tar -xzf meterkat-companion-<version>-linux-x64.tar.gz && ./meterkat setup`

Setup finds your AI tools, installs itself for your user (no admin rights), asks about autostart, then shows a QR code. Scan it with the Meterkat app. Later: `meterkat inspect`, `meterkat pair`, `meterkat revoke`, `meterkat delete`, `meterkat update`, `meterkat licenses`.

Installed companions update themselves from this repository once a day. They only install a release whose `SHA256SUMS` carries a valid Ed25519 signature from an embedded release key. To turn updates off, run `meterkat update disable`.

## Verify a download

Release public key:

```
-----BEGIN PUBLIC KEY-----
MCowBQYDK2VwAyEAsfoAgmlN/MNRT0/+INbbeSy/9oDUfK5CWQMZmUg0hNc=
-----END PUBLIC KEY-----
```

Save it as `meterkat-release.pub`. Download `SHA256SUMS` and `SHA256SUMS.sig` from the same release into the folder that holds your file. Then:

```sh
base64 -d SHA256SUMS.sig > SHA256SUMS.sig.bin
openssl pkeyutl -verify -pubin -inkey meterkat-release.pub -rawin -in SHA256SUMS -sigfile SHA256SUMS.sig.bin   # "Signature Verified Successfully"
sha256sum -c SHA256SUMS --ignore-missing                                                                       # "<file>: OK"
```

On Windows, compare `Get-FileHash <file>` with the line for your file in `SHA256SUMS`.

From 1.0.2 on you can also check that a file was built by the public release workflow from the tagged source:

```sh
gh attestation verify <file> -R Fade97/meterkat-companion
```

## Support

Read the [FAQ](https://meterkat.fabian-duerkop.de/support), or email meterkat@fabian-duerkop.de. Please never send pairing links, QR codes or tokens.

Meterkat is an independent app and is not affiliated with or endorsed by Anthropic, OpenAI or Google.
