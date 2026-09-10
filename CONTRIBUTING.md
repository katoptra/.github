# Contributing

These rules hold across every katoptra repository. A repository with a CONTRIBUTING of
its own adds to them and says what it adds.

## Ground rules

- One mirror per repository. A mirror's Taskfile is its identity, the source, bucket and
  host, plus the verbs it replaces. The shared verbs live in the library. If a change
  copies a verb into a mirror, the verb belongs in the library instead.
- Every run happens inside the toolbox image, on a laptop and in Actions alike. A change
  to the tools is a change to the library's lock file and nothing else.
- Secrets reach a run by name from 1Password. No credential, account identifier, bucket
  name, endpoint or healthcheck URL is ever committed. A repository holds `op://`
  references only.
- Storage is the bill. If a change adds objects, bytes or write operations, the pull
  request says by how much.
- Actions are pinned to a full commit SHA with the version in a trailing comment.
  Dependabot keeps them current.
- Never create a repository at a name a transferred repository used to have. GitHub
  deletes the redirect permanently.

## Checking a change

```sh
task check    # render every command of the pipeline inside the toolbox, no network, and diff it against render.txt
```

The check workflow runs the same render on every pull request and compares it with the
render committed in the repository, so any change to what a mirror will execute shows
up in the diff. The verbs that write to a bucket have no mock. Test those on a fork
against a scratch bucket, and say so in the pull request.

## Commits and pull requests

`<type>(<scope>): <summary>`, imperative, under 75 characters. Types: feat, fix,
refactor, docs, test, chore, perf, ci. One pull request per change, squash merged. No
attribution trailers.

## Where to file

Issues go on the repository they concern. If it is not clear which one, this
repository is fine. Security problems are reported privately; see
[SECURITY](SECURITY.md).
