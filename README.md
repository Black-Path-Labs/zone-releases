# Zone releases

Signed binaries for the Zone client (`zone`). This repository holds no source code.

Builds: Linux (x86_64, aarch64, static musl), macOS (x86_64, aarch64), Windows (x86_64), Android (aarch64).

## Install

Each release carries its own `install.sh` as a release asset; this repository's main branch does not, so take it from a release, never from here.

v0.1.0-rc.19's installer still reads a private repository, so until the next release install by hand (Linux x86_64 shown; pick your target from the release's assets):

```sh
v=v0.1.0-rc.19; t=zone-0.1.0-x86_64-unknown-linux-musl
for f in $t SHA256SUMS SHA256SUMS.cosign.bundle; do curl -fsSLO https://github.com/Black-Path-Labs/zone-releases/releases/download/$v/$f; done
curl -fsSLO https://raw.githubusercontent.com/Black-Path-Labs/zone-releases/main/cosign.pub
sha256sum cosign.pub   # 151adb2d673d95e9a5640dbaa749624fc25afa822e15e2b2cf129315d9404f72
cosign verify-blob --key cosign.pub --bundle SHA256SUMS.cosign.bundle --insecure-ignore-tlog=true SHA256SUMS
sha256sum -c --ignore-missing SHA256SUMS && mkdir -p ~/.local/bin && install -m 0755 $t ~/.local/bin/zone
```

Without cosign, openssl checks the same signature (ECDSA P-256 over `SHA256SUMS`):

```sh
tr -d '\r\n ' < SHA256SUMS.cosign.bundle | sed -n 's/.*"signature":"\([A-Za-z0-9+/=]*\)".*/\1/p' | openssl base64 -d -A > sig
openssl dgst -sha256 -verify cosign.pub -signature sig SHA256SUMS
```

From the next release on, the release's installer reads this repository:

```sh
curl -fsSLO https://github.com/Black-Path-Labs/zone-releases/releases/latest/download/install.sh && sh install.sh
```

Download it first and read it rather than piping `curl` into `sh`.

## Release key

`cosign.pub` here is the current release key (SHA-256 `151adb2d673d95e9a5640dbaa749624fc25afa822e15e2b2cf129315d9404f72`), the key from v0.1.0-rc.18 on. Releases are signed with cosign and no transparency-log entry, hence `--insecure-ignore-tlog=true`. The rc.14 key (`425a8715...`, rc.14 to rc.17) and the rc.9 key (`cb13cea4...`) are revoked. An older release verifies with the `cosign.pub` from its own source tag, not with this file.

A running `zone` daemon upgrades itself from this repository's releases, checking the same signature.

Source is private. Questions: matt@blackpathlabs.com
