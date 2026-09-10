# Katoptra

Greek for mirrors. Bytes here are closer than they appear.

Each repository is one mirror: an upstream, a sink, and a pipeline that syncs the two on
a schedule from GitHub Actions. The public mirrors are served from Cloudflare R2 and
listed with live status at [katoptra.org](https://katoptra.org/).

| Mirror | Upstream | Cadence | Repository |
|---|---|---|---|
| [ctan.katoptra.org](https://ctan.katoptra.org/) | [CTAN](https://ctan.org), all of it | hourly | [ctan](https://github.com/katoptra/ctan) |
| [tlnet.katoptra.org](https://tlnet.katoptra.org/) | CTAN `systems/texlive/tlnet`, what `tlmgr` installs from | daily | [tlnet](https://github.com/katoptra/tlnet) |

Two more mirror into Proton Drive rather than onto the web. What they copy is private;
the pipelines are public and run on the same toolbox, so they are forks like the others.

| Mirror | Upstream | Cadence | Repository |
|---|---|---|---|
| GitHub | every repository under the owners it names, one git bundle each | nightly | [github](https://github.com/katoptra/github) |
| Dropbox | one Dropbox account | nightly | [dropbox](https://github.com/katoptra/dropbox) |

More are on the way.

## Why this exists

- I built these for myself. They live here so the pipelines are namespaced and share one
  toolbox.
- Open source in the sense that you can fork and run it. Or just use the published
  mirrors like I do.

## How a mirror runs

- Everything the mirrors share lives once, in [lib](https://github.com/katoptra/lib):
  the toolbox that starts a run, contains it in an image, resolves its secrets, checks
  it and reports it, and an engine per transport that moves the bytes. lib's README is
  the manual.
- An rsync mirror lists upstream, diffs the listing against the state the last run left
  in the bucket, publishes the delta in batches and checkpoints after each. It verifies
  data and signatures wherever upstream provides them.
- A Proton mirror stages what its upstream holds and uploads it in one call; unchanged
  files are skipped, changed ones become revisions, and Proton's version history is the
  history of the mirror.
- Every run happens inside one pinned image, on a laptop and in Actions alike, and pings
  a healthcheck when it finishes. Silence is the alert.
- Secrets reach a run by name from a vault. No credential, account identifier, bucket
  name or endpoint is in any repository.

## Want your own?

- Fork a mirror and follow its README's "Want your own?": the lines to change, the
  bucket, the vault item, its own accounts, the first run.
- Everything the mirrors share, from making a vault and a service account to what the
  bucket holds and how the workflows run, is in lib's README, once.
- A full CTAN mirror costs under two dollars a month.

## Contributing

- Pull requests are welcome. [CONTRIBUTING](https://github.com/katoptra/.github/blob/main/CONTRIBUTING.md) has the ground rules.
- Found a way to serve altered bytes? [SECURITY](https://github.com/katoptra/.github/blob/main/SECURITY.md) explains how to report it privately.

MIT licensed. Built by [Josh Vaughen](https://ijosh.com).
