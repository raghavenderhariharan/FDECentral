# FDE Central Hub

A dynamic, animated webpage for the **FDE Central Biweekly Connect** — trainings and recordings.
Each session's recording plays back as chapter clips (a playlist), and everything shared on that
day (decks, documents, toolkits, emails) is listed beside it, grouped by category. The whole page
is a single, self-contained `index.html` with no build step.

## What it shows

- **Session timeline** — a date strip across the top. The latest session loads by default; new
  sessions appear as they are added.
- **Now playing** — the session's recording as a featured player card, with date, host, time and
  a summary. Clicking opens the recording in SharePoint (sign-in required).
- **Chapters** — a horizontal playlist of clip cards, one per topic covered. Each clip opens the
  full recording in SharePoint.
- **Shared on this day** — a resource library of every file posted for the session, grouped
  (Session materials, Atlanta Meeting, M3 Gateway Toolkit, …) with colour-coded file-type badges.
  Each card links to the file; each group header links to its SharePoint folder.
- **Decisions & follow-ups** — the session's agreed decisions and action items with status pills.

## Source of truth

Content comes from the SharePoint library:

> Infor Velocity Suite › Shared Documents › FDE_Infor_Velocity_Suite ›
> FDE- Infor Velocity Suite › **FDE Central Biweekly Connect**

Folders are organised by date (e.g. `28-Sep-2026`). All links in the page resolve back into that
library and require an Infor sign-in.

## Adding a new session

The page is driven entirely by the `DATA.sessions` array near the bottom of `index.html`.
To publish a new session, append one object to that array:

```js
{
  id: "2026-10-12",
  d: "12", m: "Oct", y: "2026",       // shown on the timeline tab
  dateLabel: "12 October 2026",
  weekday: "Biweekly connect",
  badge: "Session 02",
  title: "FDE Central Biweekly Connect",
  time: "6:00 – 7:00 PM IST",
  organizer: "Raghavender Hariharan",
  folderUrl: SP_ROOT + "/12-Oct-2026",
  summary: "…",
  recordings: [
    { title: "…", kind: "MP4 · …", dur: "≈ 60 min", url: "<SharePoint recording link>" }
  ],
  topics: [ { tag: "…", title: "…", who: "…", desc: "…" } ],
  library: [
    { group: "Session materials", icon: "doc", folderUrl: SP_ROOT + "/12-Oct-2026",
      items: [ { name: "exact-file-name.docx" } ] }
  ],
  decisions: [ "…" ],
  followups: [ { text: "…", owner: "…", status: "Planned" } ]  // Planned | Open | In progress | Ongoing
}
```

Notes:
- File links are built automatically as `folderUrl + "/" + encodeURIComponent(name)`, so you only
  supply the **raw file name** — spaces, `&` and apostrophes are handled for you.
- `icon` for a library group is one of `doc`, `users`, `code`.
- The newest session is shown first by default; order the array oldest → newest.

## Running / viewing

Open `index.html` in any browser, or host it on any static web server. It is also published as a
Claude artifact for quick sharing. No dependencies beyond Google Fonts.
