# PhosphorOS

A fake terminal styled like an old CRT phosphor monitor, in one self-contained HTML file. No libraries, no external fonts — just inline CSS and JS.

Open `phosphor-shell.html` in a browser. It boots, then hands you a prompt.

## What's in the look

One working shell sits at the bottom; everything else is an independent, toggleable effect layer stacked on top:

| layer | what it does |
|---|---|
| `glow` | phosphor bloom around the text (the single most important effect) |
| `scanlines` | horizontal raster lines |
| `vignette` | darkened corners |
| `curve` | rounded glass + edge falloff |
| `flicker` | slight brightness instability |
| `rollbar` | slow brightness band drifting down the screen |
| `aberration` | red/blue color fringing |

Restraint is the design: 90% of the look is glow + scanlines at low opacity. The rest should be felt, not noticed.

## Commands

`help` lists everything. The interesting ones:

- `fx` — table of all layers and their state; `fx scanlines off`, `fx all off` to dissect the look
- `theme green|amber|ice` — retint the whole tube via one CSS variable
- `reboot` — power-off collapse animation, then the boot sequence again
- `ls` / `cat README.txt` — you know what to do

Respects `prefers-reduced-motion`. Works with mobile soft keyboards.

## Rebuilding it

`prompt.md` contains a complete build prompt — paste it into an LLM chat to have the whole demo rebuilt from scratch.
