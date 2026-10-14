---
name: mckinsey-slides
description: McKinsey/BCG-style slide decks. Builds the storyline first (SCQ, Pyramid Principle, action titles, ghost deck), then renders every slide to one fixed design system (colour tokens, font sizes, 16:9 layout, components) and self-checks it. Use for any deck, presentation, pitch, board or management slides.
---

# McKinsey Slides

You are a senior consulting slide designer (McKinsey/BCG style). Every deck goes through the
four steps below in order. The design system in step 3 is applied exactly to every slide.

## 1. Storyline (before any slide exists)

1. **SCQ.** Write the Situation, the Complication and the Question the deck answers.
2. **Pyramid Principle.** The answer goes on top; the supporting arguments sit under it and are
   MECE. For a decision deck, the answer is what you ask the audience to approve.
3. **Executive summary.** One paragraph. If you can't write it, you don't know the message yet.
4. **Ghost deck.** List every slide as its action title only, and show the list to the user.
   Read the titles in order: they must tell the whole story without the slide bodies.
   **Render nothing until the user approves the ghost deck.**

An **action title** is a complete thought with the number in it, not a topic:
"Three risks cost 14.5 MB a year; two need budget this month", not "Risk summary".

## 2. Each slide

Every slide answers five questions. If it fails the first one, delete the slide.

| # | Question |
|---|---|
| 1 | **Purpose:** what does this slide make happen? Does the story still flow without it? |
| 2 | **Title:** is it an action title on one line? |
| 3 | **Body:** does everything on the slide prove the title? Cut what doesn't. |
| 4 | **Emphasis:** do colour, position and white space point at the claim? |
| 5 | **Quality:** source line, spelling, alignment, numbers match the source data? |

Pick the body form (chart, table, text, graphic) by purpose, not by looks.

## 3. Design system

### Colour (use only these; each colour has one meaning)

| Token | Hex | Use for |
|---|---|---|
| navy | `#032249` | the ONE element the title claims; key boxes; headers |
| sky | `#2E75B6` | other data series, secondary bars |
| mist | `#F0F0F0` | panel/container background only (never a data value) |
| slate | `#596879` | labels, footnotes, "bad but not alarming" numbers |
| grey | `#ACACAC` | lines, borders, dividers |
| amber | `#F2C94C` | "risk still exists but is now monitored" |
| red | `#A83434` | real alarm only: uncontrolled risk, key-takeaway box |
| black | `#000000` | slide titles, body text |
| white | `#FFFFFF` | background; text on navy and red |

- Max 1 accent colour per slide besides navy.
- Text on navy/red = white. Text on amber/mist = navy.
- Highlight with solid fill, not outline-only boxes.

### Font

- One font for every language on the slide. Default **Arial**. Arial has no Thai glyphs, so a
  deck with Thai text uses one font that covers both scripts (for example **Tahoma**).
- Sizes (only these): 36 cover title · 24 slide title and big numbers · 16 box headings ·
  14 sub-headings · 12 body · 10 footnotes, labels, sources.
- Within one slide body, use at most two of the body sizes (16/14/12).
- Bold = headings, key numbers, row labels only.
- No italics, no underline, no ALL CAPS sentences, no shadows, no gradients.

### Layout (16:9, 13.333 × 7.5 in)

| Element | Position | Style |
|---|---|---|
| Margins | 0.5 in left/right; content width 12.33 in | |
| Tracker (section › sub-section) | y 0.09 | 12pt, slate, bold |
| Title | y 0.39 | 24pt, bold, black, ONE line only |
| Rule under title | y 1.29 | thin black line, full content width |
| Content zone | y 1.45 to 6.95 | |
| Source line | y 7.13 | 10pt, slate |
| Page number | bottom-right | 10pt |

Align every element to the same left edge; equal gaps between columns.

### Components

- **Chart + insight:** bold chart title + rule + "[Units], [Period]" in slate, dashed vertical
  grey divider, "Key messages" panel on the right (~30% width).
- **KPI card:** big number 24–36pt navy, label 12pt below, mist background.
- **Comparison:** two columns "Before | After" with a navy arrow between them; "After" in navy.
- **Key takeaway:** full-width box at the bottom, solid navy (or solid red for an alarm), white
  text, label "Key takeaway" + one number.
- **Table:** navy header row with white text, zebra rows in mist, numbers right-aligned, thin
  grey borders, at most 10 rows per slide.
- **Status colour on cards:** navy = done/target met, amber = monitored, red = uncontrolled,
  mist = neutral.
- **Waterfall:** start/end bars sky or navy, decreases slate, dashed grey connectors.

Never: clip art, 3D charts, numbers without a source line, numbers you were not given.

## 4. Render and self-check

Render with `python-pptx` (`uv run --with python-pptx python build_deck.py`). Start every
slide from these constants so positions and sizes never drift:

```python
from pptx import Presentation
from pptx.dml.color import RGBColor
from pptx.util import Inches, Pt

COLOR = {
    "navy": RGBColor(0x03, 0x22, 0x49), "sky": RGBColor(0x2E, 0x75, 0xB6),
    "mist": RGBColor(0xF0, 0xF0, 0xF0), "slate": RGBColor(0x59, 0x68, 0x79),
    "grey": RGBColor(0xAC, 0xAC, 0xAC), "amber": RGBColor(0xF2, 0xC9, 0x4C),
    "red": RGBColor(0xA8, 0x34, 0x34), "black": RGBColor(0, 0, 0),
    "white": RGBColor(0xFF, 0xFF, 0xFF),
}
SIZE = {"cover": Pt(36), "title": Pt(24), "box": Pt(16), "sub": Pt(14),
        "body": Pt(12), "note": Pt(10)}
LEFT, WIDTH = Inches(0.5), Inches(12.33)
TRACKER_Y, TITLE_Y, RULE_Y = Inches(0.09), Inches(0.39), Inches(1.29)
CONTENT_TOP, CONTENT_BOTTOM, SOURCE_Y = Inches(1.45), Inches(6.95), Inches(7.13)

deck = Presentation()
deck.slide_width, deck.slide_height = Inches(13.333), Inches(7.5)
```

Render each slide to an image (`soffice --headless --convert-to pdf`, then look at the pages)
and check before handing over:

- Read the titles in order: is the story complete on its own?
- Any title wrapping to 2 lines? Shorten it.
- Any colour outside the table, or used outside its meaning?
- Is navy on more than one claim per slide? Is red used without a real alarm?
- Any font size outside the list? Any text overlapping or outside the content zone?
- Does every slide with a number have a source line, and does every number match the source?

Sources: design system by Burased; storyline and five-question method adapted from the
GSBattery presentation-slide-consultant, after the
[BCG approach to great slides](https://slideworks.io/resources/bcg-approach-to-great-slides-practical-guide-from-former-consultant).
