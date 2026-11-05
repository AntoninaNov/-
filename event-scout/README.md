# KSE Event Scout

Every morning (07:46 Kyiv time) an agent reads new KSE event announcements,
decides which ones match your interests, and puts the good ones in your Google
Calendar with the registration link on top.

## What it reads

| Source | Status |
|---|---|
| Work Slack: `#event` | ✅ connected (account `anovak_work@kse.org.ua`) |
| Google Calendar "KSE EVENTS" (events for students and staff) | ✅ connected |
| Students Slack | ⛔ not connected (the Slack connector reaches one workspace) |

## What it does with each event

| Match | Action |
|---|---|
| Strong (8–10) | Added to your primary calendar in **yellow**. A `📝` in the title means "registration needed, link at the top of the description". |
| Medium (5–7) | Not added; listed under "Maybe" in the run report. |
| Low (0–4) | Skipped. |
| Already in your calendar | Not duplicated, including events you added yourself. |
| Clashes with your calendar or a class | Not added; listed under "Conflicts". |

After you register, delete the `📝` from the title.

## What it deliberately does not do

- **Register for you.** KSE registration forms (`forms.kse.ua`, Google Forms)
  need your KSE login, and this cloud environment cannot reach those sites. The
  agent surfaces the link and you register; it takes about 30 seconds.
- Post, react or DM anything in Slack. It only reads.
- Edit or delete your existing events, or answer invitations.
- Repeat details of your other calendar events in its reports.

## Where it runs

It is a scheduled task named **KSE event scout** that runs inside the Claude
Code session it was created in. Each run's report appears in that conversation.
Archiving that session stops the scout.

## Changing it

- **Interests:** edit [`interests.md`](interests.md) (or describe the change),
  then ask Claude: *"sync my event scout with interests.md"*. The scheduled
  task keeps its own copy of [`playbook.md`](playbook.md) + `interests.md`, so
  file edits alone do not reach it.
- **Time, pause, stop:** ask Claude to change, pause or delete the "KSE event
  scout" scheduled task.

## Possible upgrades

1. **Phone notifications:** recreate the scout as a routine from the
   claude.ai routines page with the Slack and Google Calendar connectors
   attached. Connectors cannot be attached from inside a session in this
   organization.
2. **Real auto-registration:** run it from the Claude desktop app with Claude
   in Chrome, so it can fill in forms in your logged-in browser. It works only
   while your computer is on, and it should still stop on forms that ask
   something it cannot know.
3. **Students Slack:** connect that workspace, or forward its event channel
   into a place the scout already reads.
4. **Confirmation tracking:** read registration confirmations from
   `event@kse.org.ua` in Gmail and remove the `📝` automatically.
