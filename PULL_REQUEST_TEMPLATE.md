## What changes

<!-- One or two sentences. What a run does differently after this. -->

## Storage and operations

<!-- Does this add objects, bytes or write operations to a bucket? By roughly how much? "None" is a fine answer. -->

## Test plan

- [ ] `task check` passes, and `render.txt` was updated with `task render-update` if the commands changed
- [ ] Verbs that write to a bucket were run on a fork against a scratch bucket, or this change touches none
