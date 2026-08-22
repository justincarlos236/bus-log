# Bus arrival logger

A single-page tool for recording the exact time a bus arrives at a stop, by hand.

Live: https://justincarlos236.github.io/bus-log/

## What it is for

Real-time bus arrival feeds publish *predictions*. They never publish the fact that
a bus actually turned up. Checking a prediction against reality therefore needs a
reference measured outside the system, which in practice means a person standing at
a bus stop.

Writing times down by hand is too slow to be accurate at the moment a bus stops, so
this replaces it with one tap. The recorded event is deliberately precise:

> the moment the bus comes to a stop and its door opens

Any consistent definition works. An inconsistent one is what makes a set of
observations unusable.

## Design constraints

Used outdoors, one-handed, often at night, sometimes with no signal.

- **No external requests.** One file, no fonts, no CDN, no analytics. It works with
  the network off once loaded.
- **Large targets.** Buttons are thumb-sized because the tap has to land within a
  second of the event, without looking.
- **Nothing to lose.** Entries are written to `localStorage` on each tap, so closing
  the page or locking the phone costs nothing.
- **Handoff without typing.** The Web Share API opens the phone's own share sheet,
  so a log goes straight into a messaging app. Falls back to the clipboard, then to
  a text prompt.
- **One instruction.** Anything longer will not be read at a bus stop.

## Configuring it for another stop

Two constants near the top of the script:

```js
var SERVICES = ["12", "12e", "21", "39", "53", "81", "109", "358", "518", "Other"];
var KEY = "buslog-78069-v1";
```

`SERVICES` are the services calling at the stop, listed so that recording is a single
tap rather than a search. `KEY` scopes the saved entries; changing it starts a fresh
log rather than mixing stops together.

## Output format

```
stop 78069
2026-08-22  19:51:11  358
2026-08-22  19:55:57  109
```

Plain text, one arrival per line, sortable and diffable.
