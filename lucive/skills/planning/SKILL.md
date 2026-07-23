---
name: planning
description: Plan well with Lucive. Use when the user asks about their plan, tasks, intents, backlog, or scratchpad, or wants to add, schedule, complete, or reorganize work in Lucive.
---

# Planning with Lucive

Lucive Planning organizes work across four horizons: today, week, month, and
year. Each horizon has an intent (the user's own framing of what matters) and
tasks. Tasks live in exactly one home: a specific day (scheduledDate), a
dated week/month/year period (a commitment), the general backlog, or an
uncommitted week/month/year backlog for a future planning cycle.

## Ground rules

1. **Read before writing.** Call `read_plan` for the relevant horizon before
   adding tasks or answering questions about the plan. For broad questions
   ("what's in my backlog", "what did I finish"), use `list_tasks`.
2. **Placement is exactly one home.** Pass `scheduledDate` for a specific day,
   OR `horizon` to commit it to the current dated week/month/year, OR
   `backlogHorizon` to keep it as an uncommitted candidate for a future
   week/month/year planning cycle. Omit all three for the general backlog.
   Never pass more than one placement field. If placement is ambiguous,
   prefer the general backlog and say where you put it.
3. **Promotion moves; it does not copy.** To pull an existing candidate into
   the current plan, call `update_task` on its id with `horizon`. This removes
   it from the backlog. To return a commitment to a backlog, call
   `update_task` with `backlogHorizon` (or `null` for the general backlog).
   Do not create a duplicate task.
4. **Recurring tasks stay date-based.** Do not promote or demote a recurring
   series template or generated occurrence; removing its scheduled date would
   break recurrence advancement. Reschedule an occurrence with a non-null
   `scheduledDate`, or use `set_recurrence` / `clear_recurrence` for the
   series.
5. **Completing is not deleting.** Use `complete_task` for finished work.
   `delete_task` is destructive and only for mistakes or explicit removal
   requests; confirm before deleting anything you did not just create.
6. **Intents are the user's voice.** `set_intent` overwrites the period's
   intent line. Propose the wording and confirm before replacing a non-empty
   intent. If the tool reports the period does not exist yet, tell the user
   to open that view in Lucive once; never work around it.
7. **The Journal is out of bounds by design.** Lucive's Journal is
   end-to-end encrypted on the user's devices and is not part of this
   connection. If asked, explain the boundary; do not attempt workarounds.
8. **Say what changed.** After writes, summarize what was created, completed,
   or moved, and name the precise home (today, current week commitment, week
   backlog, general backlog).

## Useful patterns

- Morning brief: `read_plan` today + week, then summarize scheduled tasks,
  intent, and anything on the scratchpad worth acting on. After a weekend or
  a gap, call `read_scratchpad` with `lookbackDays` (3 covers a weekend) so
  notes from earlier days are not missed.
- Weekly reset: `read_plan` week, then
  `list_tasks({filter: "backlog", backlogHorizon: "week"})`. Propose which
  candidates become this week's commitments, and promote the agreed tasks
  with `update_task({id, horizon: "week"})`.
- Capture: quick "add X to my list" requests go to the general backlog unless
  the user names a day, a dated commitment, or a horizon backlog. "Maybe next
  week" belongs in `backlogHorizon: "week"`; "commit this week" belongs in
  `horizon: "week"`.
- Review recent days: `read_recent_days` returns the last week (ending
  yesterday) - what was completed each day and what was scheduled but is
  still open. Reach for it in weekly reviews, after gaps, and whenever the
  user asks what got done or what slipped.
- Process the scratchpad: `read_scratchpad` (no arguments = the global
  inbox), then propose the breakdown - which lines become tasks and where
  they land (today, a dated commitment, a horizon backlog, or the general
  backlog) and which are just notes.
  Create the tasks, then `clear_scratchpad` to empty the inbox (clearing is
  a plain reset and is not undoable - never blank it via update_scratchpad),
  and close by summarizing exactly what was created and where. Never clear
  before the derived items exist. The clear action is available on this
  connector: if a `clear_scratchpad` call fails, report the actual error
  instead of assuming the tool does not exist.
