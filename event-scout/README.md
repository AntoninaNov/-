# KSE Event Scout + Registrar

A two-part pipeline. It finds KSE events that match your interests, registers
you, and puts them in your Google Calendar. Your calendar is the hand-off
between the two parts.

```
 Cloud scout (daily 07:46 Kyiv)              Registrar (your computer)
 reads #event + KSE EVENTS calendar          picks up 📝 events
 scores against interests.md      ── 📝 ──▶  fills the form in your logged-in browser
 adds strong matches to calendar             marks ✅ registered / ⚠️ needs you
```

The registrar must run on your computer: KSE forms only open in a browser
where you are signed in to KSE, and the cloud has no such browser.

## Event lifecycle in your calendar (yellow events)

| Title starts with | Meaning |
|---|---|
| `📝` | Strong match, registration pending. The registrar will handle it. |
| `✅` | Registered. The first description line says when, with which email, which format. |
| `⚠️` | Registrar couldn't finish. The first line says why, plus the link. |
| *(no mark)* | Strong match, no registration needed. |

## Part 1: cloud scout ✅ live

Scheduled task **KSE event scout**, daily at 07:46 Kyiv time, running inside
the Claude Code session it was created in. Instructions: [`playbook.md`](playbook.md)
\+ [`interests.md`](interests.md).

| Source | Status |
|---|---|
| Work Slack `#event` | ✅ |
| Google Calendar "KSE EVENTS" (student + staff events) | ✅ |
| Students Slack | ⛔ not connected (the Slack connector reaches one workspace) |

Strong matches (8–10) go in your calendar; "maybe" (5–7) and clashes are only
listed in the run report. It reads Slack and never writes there.

## Part 2: registrar ⏳ needs a one-time setup on your computer

Instructions and your form details: [`registrar.md`](registrar.md) (one file,
paste it whole).

1. Open the **Claude desktop app** (same account) and make sure the **Google
   Calendar** connector is on.
2. Pick the browser it will use: **Claude in Chrome** or the app's built-in
   browser. In that browser, sign in to **both** Google accounts
   (`anovak@kse.org.ua`, `anovak_work@kse.org.ua`) and open any `forms.kse.ua`
   form once so your KSE login is remembered.
3. In the desktop app, ask Claude to *"create a scheduled task named KSE event
   registrar that runs every 2 hours from 9:00 to 21:00"* and paste the whole of
   `registrar.md` as its prompt.
4. **Run it once by hand** and approve the sites it asks for (`forms.kse.ua`,
   `docs.google.com`, `forms.gle`, `zoom.us`), so later runs don't stop
   at a permission prompt.
5. It runs only while your computer is on and the app is open. Pending `📝`
   events simply wait for the next run.

### What the registrar fills in

| Field | Value |
|---|---|
| Email | staff events → `anovak_work@kse.org.ua`; general/student → `anovak@kse.org.ua` |
| Status | Staff, Education in Emergency Research Center, Analyst |
| Format | In person when both are offered |
| Required open questions | 1–2 honest sentences from "About me" |
| Required consents | Agreed; optional newsletters left unticked |
| Phone, patronymic, date of birth, IDs, file uploads, payment | Not provided → marks `⚠️ needs you` |

To let it fill more fields (e.g. phone), add them under "Your details" in
`registrar.md` and update the scheduled task's prompt.

### Guardrails

- Only KSE sites, Google Forms, Zoom registration and linktr.ee. Anything else
  → `⚠️`.
- Never types passwords or codes, never pays, never uploads files.
- One submission per event, at most 5 per run. Checks the form matches the
  event before filling it in.
- Edits only events the scout created (title + description).

## Changing things

- **Interests:** edit `interests.md`, then ask Claude here to *"sync my event
  scout with interests.md"*. The scheduled task keeps its own copy.
- **Form details:** edit `registrar.md`, then replace the desktop task's prompt.
- **Pause or stop:** ask Claude to pause or delete "KSE event scout" (cloud) or
  "KSE event registrar" (desktop).

## Possible upgrades

- Phone notifications for the scout: recreate it from the claude.ai routines
  page with the Slack and Google Calendar connectors attached.
- Students Slack: connect that workspace, or forward its event channel somewhere
  the scout already reads.
