# agent-ops-flags

Public runtime flags for the autonomous Claude PR agent. Contains no secrets.

## Global kill switch

`state` holds one word:

- `on`  — the fleet runs normally (agent reviews / tests / merges).
- `off` — every agent, in every repo, pauses on its next run. Each run fetches
         this file first; when it reads `off` the job exits cleanly (green, no
         review, no merge).

To pause the entire fleet instantly, edit `state` to `off` and commit. To
resume, set it back to `on`. No redeploy needed — it takes effect on the next PR
event fleet-wide.

The workflow fails OPEN (keeps running) if this file is briefly unreachable, so
a network blip never silently halts the fleet; the switch engages whenever the
file is reachable and reads `off`.
