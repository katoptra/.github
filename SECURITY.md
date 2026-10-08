# Security

This policy is applicable to each katoptra repository that has no SECURITY file.

## The guarantees of the mirrors

- A public mirror copies the files that upstream publishes, byte for byte. It serves them
  at the same paths as upstream. When upstream publishes a checksum adjacent to a file, the
  mirror copies the checksum adjacent to that file.
- If upstream signs an index of its tree, the mirror examines the signatures before it
  publishes. For TeX Live, the pipeline does these checks:
  - It makes sure that `texlive.tlpdb` and the TeX Live installers and updaters agree with
    their SHA-512 checksums and GPG signatures. The pipeline pins the fingerprint of the
    TeX Live primary key.
  - It rejects a signature from an expired or revoked key.
  - It makes sure that `texlive.tlpdb.xz` decompresses, byte for byte, to the
    `texlive.tlpdb` that it examined. `tlmgr` downloads the `.xz` copy.
  - It compares each package container with its checksum in the signed tlpdb.
  - It uploads the tlpdb only after each container in the tlpdb is in the bucket.
  - On the client, `tlmgr` does the same checks again.
- GNU and Savannah sign each release file with a detached signature, a `.sig` file. The
  mirror copies each `.sig` file adjacent to the file that it signs. The client examines
  the signature (`gpg --verify`). The mirror does not examine it.
- If a check finds a problem in a batch, the pipeline does not upload that batch. The
  bucket stays at its last checkpoint.
- After the pipeline publishes, it reads a sample of the uploaded files through the domain
  of the mirror. It compares the sample with the staged copy.
- Each public mirror also reads one canary file through its domain, as a Perl client
  (`libwww-perl`). It compares the file with the copy in the bucket. If the domain changes
  the file or does not let a Perl client read it, the run stops with an error.
- A private mirror holds personal data. These rules are applicable to it:
  - The pipeline keeps the state of the mirror encrypted at rest.
  - Logs and summaries show counts, not names.
  - No workflow uploads an artifact.
  - The runner is a single-tenant machine that GitHub deletes after the job.
- No repository holds a credential. The organization has one 1Password vault, with one
  item for each mirror. One service account reads the vault. A run identifies each secret
  with its name and holds the value only during the run. Only a dispatched sync run holds
  the token of the service account. A pull-request check gets no secret.

A public mirror serves all other files with no change from upstream. Send a report about a
problem in a package to its upstream: CTAN, TeX Live, GNU or Savannah. This organization
copies the files that they publish.

## Reporting

Send a private vulnerability report for each of these problems:

- A way to serve changed content, or content with no correct signature, through a
  katoptra mirror
- A vulnerability in a pipeline, in the scheduler or in katoptra.org.

To send the report:

1. Find the repository that has the problem. The scheduler is
   [katoptra/dispatch](https://github.com/katoptra/dispatch), and katoptra.org is
   [katoptra/site](https://github.com/katoptra/site).
2. In that repository, open the Security tab.
3. Select "Report a vulnerability".

If you do not know which repository has the problem, send the report on
[katoptra/.github](https://github.com/katoptra/.github/security/advisories/new). Do not
open an issue for the problem. All persons can read an issue.

## Supported versions

The supported versions are the default branch of each repository and the mirrors in
operation. There are no releases.
