# Katoptra

Greek for mirrors. Bytes here are closer than they appear.

Each repository is one mirror: an upstream, a bucket, and a pipeline that syncs the two
on a schedule from GitHub Actions. The mirrors are served from Cloudflare R2 and listed
with live status at [mirrors.ijosh.com](https://mirrors.ijosh.com/).

| Mirror | Upstream | Cadence | Repository |
|---|---|---|---|
| [ctan.ijosh.com](https://ctan.ijosh.com/) | [CTAN](https://ctan.org), all of it | hourly | [ctan](https://github.com/katoptra/ctan) |
| [tlnet.ijosh.com](https://tlnet.ijosh.com/) | CTAN `systems/texlive/tlnet`, what `tlmgr` installs from | daily | [tlnet](https://github.com/katoptra/tlnet) |

More are on the way.

## How a mirror runs

- Lists upstream and diffs it against the state the previous run left in the bucket.
- Works the delta in batches. Each batch is fetched, verified, uploaded and checkpointed
  before the next starts, so a run that dies repeats one batch, not the tree.
- Verifies signatures where upstream signs. TeX Live's control files are checked against
  their SHA-512 and GPG signatures with the key fingerprint pinned, and every package
  container against the signed checksum list. Nothing unverified goes live.
- Reads a sample back over the public domain, writes the run summary, and pings
  healthchecks.io. Silence is the alarm.
- Runs every step inside one pinned toolbox image, on a laptop and in Actions alike.
- Holds no credentials. Each mirror has one vault and one service account that reads
  only that vault. Secrets arrive by name and leave when the run does.

## Want your own?

- Fork a mirror.
- Set three vars: the source, the bucket, the hostname.
- Point the workflow at your vault.
- A full CTAN mirror costs under two dollars a month. R2 does not bill for bandwidth, so
  popularity is free.

## Contributing

- Pull requests are welcome. [CONTRIBUTING](https://github.com/katoptra/.github/blob/main/CONTRIBUTING.md) has the ground rules.
- Found a way to serve altered bytes? [SECURITY](https://github.com/katoptra/.github/blob/main/SECURITY.md) explains how to report it privately.

MIT licensed. Built by [Josh Vaughen](https://ijosh.com).
