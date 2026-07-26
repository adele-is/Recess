# CLAUDE.md — Recess

A one-page site for Recess (Adele Cubitt Cohen, Auckland). Single HTML file, no
build step, no framework, no dependencies beyond a Google Fonts link.

---

## Files

| File | Owner | Purpose |
|---|---|---|
| `index.html` | Claude | The whole site. Markup, CSS, behaviour, and the `CONTENT` block. |
| `copy.md` | Adele | Every word on the site, in plain language. The source of truth for copy. |
| `CLAUDE.md` | Marty | This file. Rules for editing. |
| `adele.jpeg` | — | Portrait used in the "Just me" panel. |

---

## The golden rule

**When a copy change comes in, edit only the `CONTENT` object at the top of the
`<script>` block in `index.html`. Nothing else.**

Do not touch the markup, the CSS, the render functions, or the panel logic unless
the request is explicitly about layout or behaviour. If a copy change seems to
require a markup change, say so and ask before doing it.

The most common failure mode on a project like this is quiet drift: a class name
"tidied", a heading "improved", a hover state lost. The `CONTENT` block exists to
make that impossible for routine updates.

---

## The update workflow

1. Adele edits `copy.md`.
2. She sends `copy.md` and `index.html` to Claude.
3. Claude transcribes the changed strings into `CONTENT`. Verbatim — no rewriting,
   no "polishing", no fixing what looks like a typo without flagging it first.
4. Claude returns the updated `index.html`.

If `copy.md` and `CONTENT` disagree, `copy.md` wins.

---

## Copy conventions inside `CONTENT`

- **All strings use backticks**, not quotes. Adele types apostrophes and quotation
  marks constantly; backticks keep both safe. Keep it that way.
- `**double asterisks**` become `<strong>`. The CSS decides whether that renders as
  weight or as the accent colour, depending on context. Do not hardcode `<strong>`
  or `<span>` into the strings.
- A literal newline inside a string becomes `<br>`. Used in headings only.
- All other HTML is escaped automatically. Typing `<` renders a `<`. This is
  deliberate — don't remove the escaping to allow inline HTML.
- `body` is an array of paragraphs. `list` and `offerings` are arrays. Add or
  remove entries freely; the page adapts.
- Deleting a `price` or `note` string hides that line rather than leaving a gap.

---

## Structural rules

- Panel keys (`room`, `me`, `gather`) are load-bearing. They generate the door
  cards, wire the nav links, and match the panel skeleton IDs in the markup.
  **Do not rename them.** Reordering the keys reorders the door cards — that's the
  intended way to change door order.
- The three panels have deliberately different shapes: `room` and `gather` are
  text + list; `me` is photo + text + offerings. This is by design. Don't
  homogenise them.
- `meta.title` and `meta.description` are duplicated in the `<head>` as static
  fallbacks, because link-preview scrapers often don't run JavaScript. **If one
  changes, change both.** This is the only intentional duplication in the file.

---

## Design system — locked

Don't change any of this without an explicit request.

**Colour**

```
--black  #0d0d0d   page
--white  #f8f8f6   text
--grey   #888      secondary text
--dim    #2a2a2a   tertiary / rules
--mid    #1a1a1a   panel background
--pop    #c8ff00   accent — used sparingly, and that restraint is the point
--rule   rgba(255,255,255,0.07)
```

**Type**

- DM Sans 300/400/500 — everything editorial
- DM Mono 300/400 — tags, labels, prices, nav, footer, the email address
- Headings are light weight with tight negative tracking. Emphasis comes from
  weight 500, never from size jumps.
- Mono is always uppercase with wide letter-spacing. That contrast — airy light
  sans against tight uppercase mono — is the identity. Preserve it.

**Layout**

- Hairline rules (`--rule`) separate everything. No shadows, no border-radius,
  no gradients anywhere. Zero. This is a deliberate flat, ruled, editorial system.
- Desktop padding 48px, mobile 24px. Single breakpoint at 768px.
- Three-column door grid collapses to one column on mobile.

**Motion**

- Colour and background transitions only, 0.2s. Panels open and close with a
  display swap, then a smooth scroll. No fades, no slides, no scroll-triggered
  reveals. Restraint is the aesthetic — don't add animation.

---

## Voice

Adele's, not Claude's. If asked to draft or extend copy, match this:

- Direct and warm. Short declaratives, deliberate fragments. "Monthly. Closed
  door. No agenda."
- Em dashes for the aside, the turn, the punchline.
- Confident, never boastful. It describes what happens in the room rather than
  claiming outcomes.
- Occasional bluntness ("I'll call bullshit") is on-brand and stays in.
- Second person in the panels. First person in "Just me" — that panel is Adele
  speaking, the others describe.
- **NZ English throughout.** Organisation, programme, colour, favour.
- No corporate speak. No "solutions", "leverage", "unlock", "journey",
  "empower", "curated experience". "Curated" itself is used twice, sparingly and
  literally — don't add more.
- Never add exclamation marks.

---

## Deliberate choices — leave them alone

- The site has no contact form and no booking link. That's the positioning.
  Don't add either.
- The third door is labelled "Or", not "The Gathering". Understated on purpose.
- The nav omits the third panel. Two doors in the nav, three on the page.
- `.hero-top h1` carries an extra 80px top padding on top of the section padding.
  It looks like a mistake in the CSS. It isn't — it's what floats the headline.
- The "Fundable as professional development" lines are there so buyers can
  expense it. Keep the phrasing procurement-friendly.
- Panels are closed by default. The page opens as three doors, nothing more.

---

## Quality floor

Anything shipped should still be true:

- Works down to 375px wide.
- Keyboard reachable — door buttons are real `<button>` elements, nav links are
  real anchors.
- No external JS. No analytics, no tracking, no cookie banner.
- The file opens correctly from `file://` with no server.
