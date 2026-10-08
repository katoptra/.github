# Contributing

These rules are applicable to each katoptra repository. If a repository has a CONTRIBUTING
file, that file adds to these rules and tells which rules it adds.

## Ground rules

- A mirror repository holds one mirror. The Taskfile of a mirror holds the identity of the
  mirror and the verbs that it overrides. For an rsync mirror, the identity includes the
  source, the bucket and the host. The verbs of the toolbox and of each engine are in
  [lib](https://github.com/katoptra/lib). If a change copies a verb into a mirror, that
  verb must be in lib, not in the mirror.
- Each run of a mirror occurs in the toolbox image, on a laptop and also in Actions. A
  change to a tool changes only the lock file of lib, `toolchain.lock.toml`.
- A mirror run identifies each secret with its name and gets the value from 1Password. A
  commit must not contain a credential, a private account identifier, an endpoint or a
  ping URL. A mirror repository holds only `op://` references.
- Storage is the cost of a mirror. If a change adds objects, bytes or write operations, the
  pull request must give the quantity that it adds.
- Each workflow pins its actions to a full commit SHA, with the version in a comment at the
  end of the line. Dependabot keeps these pins up to date.
- A new repository must not have the previous name of a transferred repository. If it
  does, GitHub removes the redirect from that name, and this change is permanent.

## Checks

Run `task check` before you open a pull request.

```sh
task check
```

`task check` is different in each type of repository:

- In a mirror, it makes a render of the pipeline in the toolbox image, without network
  access. The render shows each command of the pipeline. Then the check compares the
  render with `render.txt`.
- In lib, each of the two examples, `examples/rsync` and `examples/proton`, has a
  `task check` that does the same.
- In dispatch, it runs gofmt, go vet and go test.
- In site, it makes the page with Hugo and does checks on the result.

This repository has no `task check`. For a change here, the review is the check.

In a mirror, the check workflow does the same render on each pull request. It compares the
render with the `render.txt` in the commit. Thus, each change to the commands that a mirror
will run shows in the diff.

The verbs that write to a bucket have no mock. Do a test of these verbs on a fork, with a
scratch bucket. Then write in the pull request that you did this test.

## Commits and pull requests

Write the first line of each commit message as `<type>(<scope>): <summary>`, in the
imperative. That line must have less than 75 characters. The types are feat, fix,
refactor, docs, test, chore, perf and ci. Make one pull request for each change. We merge
each pull request as a squash merge. Do not add attribution trailers.

## Writing

Write the text of each katoptra repository in ASD-STE100, Simplified Technical English,
Issue 9 (January 2025). The owner of the organization keeps a copy of the standard (a PDF)
outside git. Do not copy text from the standard into a repository, a commit or a pull
request.

Use this checklist when you write:

- Use only approved words, with their approved meaning and part of speech. Use one word for
  one thing.
- In a procedure, write each instruction in the imperative. Write one instruction in each
  sentence, with a maximum of 20 words.
- In a description, write a maximum of 25 words in each sentence. Give each paragraph one
  topic and a maximum of six sentences.
- Use the active voice and the simple tenses. Put an article before a noun. Do not write a
  noun cluster of more than three words.
- Do not use an -ing form of a verb, unless a technical name has one. "Nothing",
  "ceiling", "string" and "during" are not such forms.
- Write each word in full, with no contractions. Use American spelling. Do not use a
  semicolon.
- Write each safety note as a caution. Put the command first and the risk after it. For
  example: "In a Proton mirror, do not run `task sync` from a laptop while an Actions run
  can be in progress. The two runs use one session, and then a new login can be
  necessary."
- Use a vertical list for a text that has more than two parts.

If the checklist and the standard do not agree, obey the standard.

Also obey these rules. If a rule here and the standard do not agree, obey the rule here:

- Keep the technical nouns of katoptra (mirror, upstream, bucket, state, listing, delta,
  batch, run, slot, toolbox, engine, verb, hook, render, reconcile and chain). The names of
  tools, files, commands, variables and products are also technical nouns.
- Do not write about the history of the system. The commit message gives the history. Do
  not write "replaces", "used to", "as before", "today", "now", "no longer" or
  "currently".
- Do not write the name of the vault. Write "a vault, addressed by UUID".
- Do not write the name of a private repository in a public file.
- Keep the marker of each `ponytail:` comment. Start each Go doc comment with the name that
  it is about.
- Write "the scheduler" for the system that starts runs. In a mirror repository, write
  "an external scheduler".
- Use "dispatch" only as the verb or as the name of the repository. Do not write
  "dispatcher".
- For a cadence, write "hourly", "daily" or "twice a day". Use "nightly" only for dropbox.

The standard is applicable to:

- Each Markdown file
- The landing page of tlnet
- The text of the site: `mirrors.toml`, `llms.txt`, `_index.md`, the visible text of the
  templates and the description in the manifest
- Each code comment and each docstring.

The standard is not applicable to:

- LICENSE files
- Text from upstream or from a third party, for example GNU's guidelines or the output of
  a tool
- Identifiers, commands, paths, URLs, code blocks and YAML keys (`normalise` keeps its
  name)
- Generated files: `render.txt` and the `want/` trees of the fixtures
- Output strings: `desc:`, `echo`, run summary rows, log and error lines, workflow input
  descriptions, `PAGE_FOOT` and repository descriptions
- The copy of the ijosh theme in site, the git history, commit messages and files outside
  git.

Change an output string only to correct a fact in it.

No tool examines the text for the standard. The review and the fact check do this work. A
fact check makes sure that the new text keeps each fact of the previous text: each number,
name, path, command, rule and hazard.

## Issues

Open an issue in the applicable repository. If you do not know which repository is
applicable, open the issue here. Do not open an issue for a security problem.
[SECURITY](SECURITY.md) tells you how to send a private vulnerability report.
