# Katoptra

Greek for mirrors. Bytes here are closer than they appear.

| Mirror | Of | Every | Repo |
|---|---|---|---|
| [ctan.ijosh.com](https://ctan.ijosh.com/) | all of [CTAN](https://ctan.org) | hour | [ctan](https://github.com/katoptra/ctan) |
| [tlnet.ijosh.com](https://tlnet.ijosh.com/) | the TeX Live repository `tlmgr` installs from | day | [tlnet](https://github.com/katoptra/tlnet) |

Live status at [mirrors.ijosh.com](https://mirrors.ijosh.com/). More on the way.

## The rules of the house

- Same bytes as upstream, under upstream's own paths. Nothing added, nothing renamed.
- Where upstream signs, we check the signature before a single file goes live.
- A run that dies repeats one batch, not the whole tree.
- No credentials in any repo. Ever. Secrets arrive by name and leave when the run does.
- Silence is the alarm. Every mirror pings a dead man's switch, and a missed ping sends the email.
- One toolbox image runs everything, on a laptop and in Actions alike.

## Want one?

- Fork a mirror.
- Set three vars: where it comes from, which bucket, which hostname.
- Point the workflow at your vault.
- A full CTAN mirror costs under two dollars a month. The bandwidth is free.

## Contributing

- Pull requests welcome. [CONTRIBUTING](https://github.com/katoptra/.github/blob/main/CONTRIBUTING.md) has the ground rules.
- Found a way to serve altered bytes? [SECURITY](https://github.com/katoptra/.github/blob/main/SECURITY.md) says how to tell us quietly.
- MIT licensed. Built by [Josh Vaughen](https://ijosh.com), [jshvn](https://github.com/jshvn).
