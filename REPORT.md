# Status report: Tue 24 Nov 2026, 09:45

The state of the command deck after this morning's restart. It covers who
is online, what is in review, what landed, and what is still open.

## The restart

The machine restarted at about 09:35. That closed every agent window.
Claude and Grok were started again first. At 09:38 Claude reopened Vibe,
Codex and Gravity, each in its own kitty window, and the Lumo app. The
comms-deck service, push-courier and scratchpad-keeper came back
on their own. All three are `active`.

## Seats

| Seat | State at 09:45 | Notes |
| --- | --- | --- |
| claude | busy | Writing this report. |
| grok | busy | Reviewing comms-deck #39 and #38. Its MCP server, hooks and status line match what `install` writes. |
| vibe | busy | Reviewing comms-deck #45 and #41. |
| gravity | idle until Tue 1 Dec, 05:08 | Running again, but out of usage until the reset. Don't hand her work. |
| codex | paused | Running again, but out of usage until Wed 9 Dec. It logged "couldn't see the prompt line" with notes still queued. |
| alice | idle | Grok Bot app seat. The app came back with the restart. |
| juno | hidden | Grok Bot app seat, no chat open yet. |
| lumo | idle | The app was reopened. She reads notes only when she calls `read_messages`. |

## Pull requests in review (comms-deck, into `test`)

Nobody reviews their own PR. The `Reviewed by:` line is written by the reviewer.

| PR | What | Reviewer | State |
| --- | --- | --- | --- |
| #45 | Scratchpad: Mistral Vibe top right, small chat removed | vibe | in review |
| #41 | Codex: countdown to its usage-limit reset in ACTIVITY | vibe | in review |
| #39 | Lumo: waiting notes ride along with any tool reply | grok | in review |
| #38 | `status --json` and `wake <seat>`; never wake a spent agent; self-test can't type into real windows | grok | in review |
| #40 | Load balancer spec: routes reviews too (docs only) | claude, vibe | Approved at rev 3. The routing decision from 07:58 still has to be written in before merge. |
| #20 | Setup guide: Antigravity review, permissions and checks | claude | Not approved. It is 65 commits behind `test`, conflicts in `docs/SETUP.md`, and has two small fixes (self-test globals, two unsourced lines). |

The owner merges. Nothing merges by machine.

## Landed this morning (merged into `test`)

- #48 Vibe: never hand over a 4th note in one turn.
- #47 Vibe: the deck's five MCP tools are pre-allowed.
- #46 Mistral Vibe joins as the vibe seat.
- #44 Panel times: "tomorrow", then the weekday for up to 6 days, then the date.
- #43 ACTIVITY: a late poll shows idle.
- #42 ACTIVITY: idle-until rows are grey, with no line under them.
- #37 Alice's send loop: a whitespace-blind box check, and a back-off after a failure.
- #36 Activity text: `<system-reminder>` blocks and markup tags are dropped.
- #35 quantum-kittens follow-ups: six words under each row, idle until always last, changelog.
- #33, #34 ACTIVITY is sorted by availability, and the router uses the same `availability()`.
- #32 scratchpad-keeper keeps the scratchpad arranged at login and whenever windows change.
- #31 Chat screen tidy, and Alice's double delivery fixed.
- #30 Integration: every open PR merged and tested together.
- #29 App seats: a draft abandoned for 10 minutes is cleared.

## Repos

- Public: comms-deck, quantum-kittens, navigation-control-system, lumo-seat.
- chronophage (the no-AI watchdog) was picked to go public next. Both of
  its commits were checked, and they contain no secrets, names or personal
  paths. It waits for the owner's go-ahead.
- The rest stay private.

## Still open

- **Devin CLI** on the scratchpad, top right (Claude, starting from #22). The owner does the sign-in.
- **Notification ticker:** notification text scrolls across the top bar
  instead of pop-ups (Grok). This is a desktop config change, so it needs
  the owner's yes first.
- **Hide idle agents from USAGE** while keeping them in ACTIVITY (Vibe, branch `vibe/hide-idle-usage`).
- **Lumo's connectors:** after she was opened from the launcher, check that her connectors are on.
- **Chronophage timer:** it stays off until #38 (`status --json`, `wake`) is live.

---

Written by: Claude (2026-11-24), from the deck's log, the open pull
requests, and Alice's 09:38 recap.
