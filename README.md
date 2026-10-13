# scoop-swamp

An **unofficial**, personally maintained [Scoop](https://scoop.sh) bucket for
[swamp](https://github.com/swamp-club/swamp).

I'm a community contributor to swamp, not a member of the swamp team. This
bucket is not affiliated with or endorsed by Elder Swamp Club, Inc., and I
maintain it for my own use. It is shared in case it helps you.

**For official downloads, documentation, and support, go to
[swamp-club.com](https://swamp-club.com/).** The source and official releases
are at [github.com/swamp-club/swamp](https://github.com/swamp-club/swamp). If
something is wrong with swamp itself, report it there, not here.

## Install

```powershell
scoop bucket add swamp https://github.com/webframp/scoop-swamp
scoop install swamp/swamp
```

Upgrade with `scoop update` followed by `scoop update swamp`.

## What this bucket does

The manifest in [`bucket/swamp.json`](bucket/swamp.json) points at the
`swamp-windows-x86_64.zip` asset on the official GitHub release and pins its
SHA-256 hash. Scoop downloads the zip from swamp-club's release page, not from
this repository. Only x64 Windows is packaged.

Swamp is licensed under AGPL-3.0-only. See the
[upstream license](https://github.com/swamp-club/swamp/blob/main/LICENSE).

## How updates work

Updates are automatic and need no manual step.

1. The [Excavator workflow](.github/workflows/excavator.yml) runs every six
   hours (and on demand from the Actions tab).
2. It runs Scoop's `checkver` against swamp's latest GitHub release and compares
   it to `version` in the manifest.
3. If a newer release exists, `autoupdate` rewrites `version`, `url`, and
   `hash`, then commits the change to `main`.
4. The [Tests workflow](.github/workflows/ci.yml) validates the manifest on
   every push.

Swamp publishes several releases a day, so expect frequent bot commits.

## Maintenance notes

- **Workflow permissions.** The repo needs Settings → Actions → General →
  Workflow permissions set to *Read and write*, or Excavator cannot commit.
- **Version format.** Swamp tags look like `v20261013.192700.0-sha.91d688f1`.
  Scoop's default GitHub regex truncates this at `-sha`, so `checkver` in the
  manifest carries an explicit regex. If upstream changes its tag format, update
  that regex.
- **Check locally.** Run `scoop checkver swamp` against a checkout, or
  `scoop install .\bucket\swamp.json` to test an install before pushing.
- **Stalled workflows.** GitHub disables scheduled workflows after 60 days
  without repo activity. If upstream stops releasing, re-enable Excavator by
  hand.
- **Hashes.** Each update downloads the full zip (about 116 MB) to compute its
  hash. Upstream also publishes `checksums.txt`; switching `autoupdate` to read
  it would avoid the download.
- **Windows ARM64.** Upstream ships no ARM64 build, so none is packaged.

## Issues

Problems with this bucket (bad hash, failed update, install error) belong in
this repository's issues. Problems with swamp itself belong
[upstream](https://github.com/swamp-club/swamp/issues).
