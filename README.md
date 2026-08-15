# PhosphorOS

A fake terminal styled like an old CRT monitor, in one self-contained HTML file. No dependencies.

## Run

Open `phosphor-shell.html` in any browser — double-click it or `open phosphor-shell.html`. Nothing to install or build.

Type `help` for commands, `fx` to toggle the effect layers, `theme amber` to retint the tube.

## How it works

- One `div` is the actual terminal. Keystrokes are captured document-wide into a JS buffer and rendered as text lines; commands (`help`, `fx`, `theme`, `echo`, `clear`, `reboot`, …) are plain functions in a lookup map. A hidden input catches mobile soft-keyboard typing.
- The CRT look is a stack of independent layers on top of the text: glow and red/blue fringing are `text-shadow`s on the type; scanlines, vignette, and the drifting roll bar are overlay divs; flicker and glass curvature are CSS effects on the frame. Each layer toggles live via `fx <name> on|off` (or `fx all off` for the naked terminal).
- All colors derive from a single `--ph` CSS variable, so `theme green|amber|ice` is just one property swap.

`prompt.md` is a build prompt that regenerates the whole demo from scratch; `chat-session.md` is the transcript it came from.
