## The change

<!-- Write one or two sentences. Tell how a run is different after this pull request. -->

## Storage and operations

<!-- Does this change add objects, bytes or write operations to a bucket? If it does, approximately how many does it add? If it does not, write "None". -->

## Test plan

- [ ] `task check` gives no error. If the commands changed, `task render-update` updated `render.txt`.
- [ ] The verbs that write to a bucket ran on a fork, with a scratch bucket. Or this change has no effect on these verbs.
