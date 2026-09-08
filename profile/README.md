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

## Why this exists

- I built these for myself, and I use every one of them. They live here so the pipelines
  can share one toolbox instead of each carrying its own.
- Open source in the sense that you can fork and run it. Or just use these published
  mirrors like I do.

## How a mirror runs

- `list` the upstream, `diff` it against the `state` the last run left behind, then
  `publish` the delta and `checkpoint` the state.
- `verify` data and signatures whenever upstream provides them.
- Works in batches, sized for a GitHub Actions runner.
- Pings a healthcheck after each run if you give it one.
- Keeps its state and checkpoints in R2 or any S3-compatible bucket. Cloudflare is the
  default for its versatility and free egress.
- I preferentially use 1Password for secrets, but GitHub secrets work too.

## Want your own?

- Fork a mirror.
- Set three vars in its Taskfile: `SOURCE` (the upstream rsync URL), `BUCKET` (your
  bucket name) and `HOST` (the domain in front of it).
- Point the workflow at your vault, or your repository secret.
- A full CTAN mirror costs under two dollars a month.

## Contributing

- Pull requests are welcome. [CONTRIBUTING](https://github.com/katoptra/.github/blob/main/CONTRIBUTING.md) has the ground rules.
- Found a way to serve altered bytes? [SECURITY](https://github.com/katoptra/.github/blob/main/SECURITY.md) explains how to report it privately.

MIT licensed. Built by [Josh Vaughen](https://ijosh.com).
