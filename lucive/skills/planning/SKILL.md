---
name: planning
description: Plan well with Lucive. Use when the user asks about their plan, tasks, intents, backlog, or scratchpad, or wants to add, schedule, complete, or reorganize work in Lucive.
---

# Planning with Lucive

Lucive Planning organizes work across four horizons: today, week, month, and
year. Each horizon has an intent (the user's own framing of what matters) and
tasks. Tasks live in exactly one home: a specific day (scheduledDate), a
week/month/year period (a commitment), or the backlog.

## Ground rules

1. **Read before writing.** Call `read_plan` for the relevant horizon before
   adding tasks or answering questions about the plan. For broad questions
   ("what's in my backlog", "what did I finish"), use `list_tasks`.
2. **Placement is one of three, never two.** When creating a task, pass
   `scheduledDate` for a specific day, OR `horizon` to commit it to the
   current week/month/year, OR neither for the backlog. If placement is
   ambiguous, prefer the backlog and say where you put it.
3. **Completing is not deleting.** Use `complete_task` for finished work.
   `delete_task` is destructive and only for mistakes or explicit removal
   requests; confirm before deleting anything you did not just create.
4. **Intents are the user's voice.** `set_intent` overwrites the period's
   intent line. Propose the wording and confirm before replacing a non-empty
   intent. If the tool reports the period does not exist yet, tell the user
   to open that view in Lucive once; never work around it.
5. **The Journal is out of bounds by design.** Lucive's Journal is
   end-to-end encrypted on the user's devices and is not part of this
   connection. If asked, explain the boundary; do not attempt workarounds.
6. **Say what changed.** After writes, summarize what was created, completed,
   or updated, and where it landed (today, this week, backlog).

## Useful patterns

- Morning brief: `read_plan` today + week, then summarize scheduled tasks,
  intent, and anything on the scratchpad worth acting on.
- Weekly reset: `read_plan` week, `list_tasks` backlog, propose which backlog
  items become this week's commitments, create them only after the user
  agrees.
- Capture: quick "add X to my list" requests go to the backlog unless the
  user names a day or horizon.
