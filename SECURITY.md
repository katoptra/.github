# Security

This policy applies to every katoptra repository that has no SECURITY file of its own.

## What the mirrors guarantee

- A public mirror copies what upstream publishes, byte for byte, and serves it under
  upstream's own paths. A checksum upstream publishes beside a file is mirrored beside
  it.
- Where upstream signs its tree, the mirror verifies before publishing. TeX Live's
  `texlive.tlpdb`, its installers and its updaters are checked against their SHA-512
  and GPG signatures, with the TeX Live primary key fingerprint pinned in the pipeline.
  A signature from an expired or revoked key is rejected. Every package container is
  checked against the checksum in the signed tlpdb, and the tlpdb goes live only after
  every container it names is in the bucket. `tlmgr` repeats the check on the client.
- A batch that fails any check is not uploaded. The bucket stays at its last
  checkpoint.
- After each publish the pipeline reads a sample of what it uploaded back through the
  public domain and compares it with the staged copy.
- A private mirror holds personal data. Its state is encrypted at rest, logs and
  summaries carry counts and never names, no workflow artifact is uploaded, and the
  runner is a single-tenant machine destroyed after the job.
- No credential lives in a repository. Each mirror has one vault and one service
  account that can read only that vault, and secrets reach a run by name for the
  length of the run.

Everything else on a public mirror is served as upstream serves it. Problems with the
packages themselves belong upstream, to CTAN, TeX Live, CRAN or CPAN. This
organization copies what they publish.

## Reporting

If you find a way to serve altered or unsigned content through a katoptra mirror, or a
weakness in a pipeline, report it privately through GitHub's vulnerability reporting
on the affected repository: the Security tab, then Report a vulnerability. If it is
not clear which repository, report it against
[katoptra/.github](https://github.com/katoptra/.github/security/advisories/new).
Please do not open a public issue for it.

## Supported versions

The default branch of each repository and the live mirrors. There are no releases.
