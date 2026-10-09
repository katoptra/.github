# Katoptra

Katoptra is Greek for mirrors. Bytes here are closer than they appear.

Most repositories here are mirrors. These four repositories are not mirrors:

- [lib](https://github.com/katoptra/lib), the toolbox
- [dispatch](https://github.com/katoptra/dispatch), the scheduler
- [site](https://github.com/katoptra/site), the page at katoptra.org
- [.github](https://github.com/katoptra/.github), this profile.

A mirror has an upstream, a sink and a pipeline that copies the upstream into the sink. The
scheduler starts the runs, as jobs in GitHub Actions.

Cloudflare R2 serves the public mirrors. [katoptra.org](https://katoptra.org/) shows the
status of each mirror when you open the page.

| Mirror | Upstream | Cadence | Repository |
|---|---|---|---|
| [ctan.katoptra.org](https://ctan.katoptra.org/) | [CTAN](https://ctan.org), all of it | hourly | [ctan](https://github.com/katoptra/ctan) |
| [tlnet.katoptra.org](https://tlnet.katoptra.org/) | CTAN `systems/texlive/tlnet`, the tree that `tlmgr` installs from | daily | [tlnet](https://github.com/katoptra/tlnet) |
| [gnu.katoptra.org](https://gnu.katoptra.org/) | [GNU](https://www.gnu.org/)'s release tree, `ftp.gnu.org/gnu` | twice a day | [gnu](https://github.com/katoptra/gnu) |
| [gnu-alpha.katoptra.org](https://gnu-alpha.katoptra.org/) | [GNU](https://www.gnu.org/)'s alpha release tree, `alpha.gnu.org/gnu` | twice a day | [gnu-alpha](https://github.com/katoptra/gnu-alpha) |
| [nongnu.katoptra.org](https://nongnu.katoptra.org/) | [Savannah](https://savannah.nongnu.org/)'s nongnu releases | twice a day | [nongnu](https://github.com/katoptra/nongnu) |

Two more mirrors are private mirrors. They copy their upstreams into Proton Drive, not onto
the web. But their pipelines are in public repositories and use the same toolbox. Thus,
you can also fork these two mirrors.

| Mirror | Upstream | Cadence | Repository |
|---|---|---|---|
| GitHub | each repository of the accounts in `OWNERS`, one git bundle for each | daily | [github](https://github.com/katoptra/github) |
| Dropbox | one Dropbox account | nightly | [dropbox](https://github.com/katoptra/dropbox) |

There will be more mirrors.

## About these mirrors

- We made these mirrors because we use them. They are in one organization. Thus, the
  pipelines have one namespace and use one toolbox.
- The code is open source only as follows: you can fork it and run it. Or you can use the
  published mirrors. We do the same.

## How a mirror operates

- [lib](https://github.com/katoptra/lib) holds one copy of each part that more than one
  mirror can use. Its README is the manual. The parts are:
  - The toolbox, which starts a run, contains the run in an image, gets its secrets, does
    its check and writes its report
  - One engine for each transport, which moves the bytes.
- An rsync mirror gets a listing of upstream. It compares the listing with the state that
  the last run put in the bucket. Then it publishes the delta in batches, with a
  checkpoint after each batch. It examines the signed tree of TeX Live before it
  publishes. GNU and Savannah releases have detached signatures, which clients examine.
- A Proton mirror puts the content of its upstream into a staging tree. Then it uploads the
  tree in one call. The upload does not send a file that did not change, and a changed
  file becomes a new revision. Thus, the version history in Proton Drive is the history of
  the mirror.
- Each run occurs in one pinned image, on a laptop and also in Actions. After a run
  completes its pipeline, it sends a ping to a healthcheck. If the healthcheck gets no
  ping before the end of its grace time, it sends an alert.
- A run uses the name of each secret to get its value from a vault. No repository holds a
  credential or an endpoint. The `op://` references use the UUID of the vault.

## Want your own?

1. Fork a mirror.
2. In the README of that mirror, do the steps of "Want your own?". These steps give the
   lines to change, the bucket, the vault item, the accounts and the first run.
3. For each part that more than one mirror can use, refer to lib's README. That README has
   one description of each part. For example, it tells how to make the vault and the
   service account. It also gives the contents of the bucket and how the workflows run.

A full CTAN mirror holds approximately 140 GB. On R2, the storage cost is approximately $2.10
a month, at $0.015 for each GB-month.

## Contributing

- You can send pull requests. [CONTRIBUTING](https://github.com/katoptra/.github/blob/main/CONTRIBUTING.md) gives the rules.
- If you find a method to change the bytes that a mirror serves, [SECURITY](https://github.com/katoptra/.github/blob/main/SECURITY.md) tells how to send a private vulnerability report.

MIT licensed. Built by [Josh Vaughen](https://ijosh.com).
