# Katoptra

Katoptra is Greek for mirrors. Each repository here is one mirror: an upstream, a bucket
or a destination, and a sync pipeline that runs on a schedule from GitHub Actions inside
a pinned toolbox. The public mirrors are served from Cloudflare R2 and listed with live
status at [mirrors.ijosh.com](https://mirrors.ijosh.com/).

| Mirror | Upstream | Cadence | Repository |
|---|---|---|---|
| [ctan.ijosh.com](https://ctan.ijosh.com/) | [CTAN](https://ctan.org), all of it | hourly | [ctan](https://github.com/katoptra/ctan) |
| [tlnet.ijosh.com](https://tlnet.ijosh.com/) | CTAN `systems/texlive/tlnet`, what `tlmgr` installs from | daily | [tlnet](https://github.com/katoptra/tlnet) |
| Dropbox to Proton Drive | a Dropbox account | nightly | [dropbox](https://github.com/katoptra/dropbox) |

Planned: CRAN, CPAN, iCloud Photos.

## How a mirror runs

1. List upstream and diff it against the state the last run left in the bucket.
2. Work the delta in batches, each fetched, verified, uploaded and checkpointed before
   the next starts. A run that dies repeats one batch, not all.
3. Where upstream signs its tree, verify before publishing. TeX Live's control files are
   checked against their SHA-512 and GPG signatures with the key fingerprint pinned, and
   every package container against the signed checksum list.
4. Read a sample back over the public domain, write the run summary, and ping
   healthchecks.io. Silence is the alert.

Every step runs inside one pinned toolbox image, on a laptop and in Actions alike. No
credential lives in any repository: each mirror has one 1Password vault and one
service account that can read only that vault, and secrets reach a run by name. The
pipelines share one toolbox and their sync engines from a common library.

## Want your own?

Fork a mirror, set its three vars, and point the workflow at your vault. Each
repository's README has the steps and the cost. A full CTAN mirror runs for under two
dollars a month.

## Contributing

Pull requests are welcome. Read [CONTRIBUTING](https://github.com/katoptra/.github/blob/main/CONTRIBUTING.md)
for the ground rules and [SECURITY](https://github.com/katoptra/.github/blob/main/SECURITY.md)
for what the mirrors guarantee and how to report a problem privately.

MIT licensed. Built by [Josh Vaughen](https://ijosh.com), [jshvn](https://github.com/jshvn).
