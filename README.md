# Zone releases

Signed binaries and the installer for the Zone client (`zone`). This repository holds no source code.

## Install

```sh
curl -fsSLO https://github.com/Black-Path-Labs/zone-releases/releases/latest/download/install.sh
sh install.sh
```

The installer picks the build for your system, downloads `SHA256SUMS` and its signature, checks the signature against the release key (`cosign.pub`), checks the binary's checksum, and installs `zone` into `~/.local/bin`. It refuses an unsigned release unless you set `ZONE_ALLOW_UNSIGNED=1`. Download the script first and read it rather than piping `curl` into `sh`.

Builds: Linux (x86_64, aarch64, static musl), macOS (x86_64, aarch64), Windows (x86_64).

## Verify by hand

```sh
cosign verify-blob --key cosign.pub --bundle SHA256SUMS.cosign.bundle SHA256SUMS
sha256sum -c --ignore-missing SHA256SUMS
```

## Note on `install.sh`

The checksum list of v0.1.0-rc.13 was signed before the installer pointed at this repository, so its line for `install.sh` will not match the file published here. The binaries and their signature are unchanged. Later releases are signed with the final installer.

Source is private. Questions: matt@blackpathlabs.com
