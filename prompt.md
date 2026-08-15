# CRT Terminal — build prompt

Paste everything below this line into another chat to have the demo rebuilt.

---

Build a single-file HTML page: an interactive fake terminal styled like an old
CRT phosphor monitor. No external resources (no CDN fonts, no libraries) —
inline all CSS and JS. Monospace system font stack (Menlo, Consolas, Courier New).

VISUAL BASE
- Full-viewport screen on a dark #101010 page ground.
- Screen background: not pure black — a radial gradient from a faint
  phosphor-tinted center (~5% of the phosphor color mixed into #050505)
  out to near-black at the edges.
- One phosphor color drives everything (text, cursor, borders), stored in a
  CSS variable: default green #33ff33. Derive a "bright" tint (mixed toward
  white) and dim/faint alpha tints from it with color-mix().
- Font size ~clamp(13px, 1.9vmin, 18px), line-height 1.4, generous padding.
- Text selection inverts: phosphor background, black text.

CRT EFFECT LAYERS — each one independent and toggleable via a class on the
screen container (fx-glow, fx-scanlines, fx-vignette, fx-curve, fx-flicker,
fx-rollbar, fx-aberration). All overlays are pointer-events: none.
1. glow — text-shadow: 0 0 3px (phosphor at ~55% alpha) plus 0 0 14px
   (phosphor at ~26% alpha). The single most important effect.
2. scanlines — overlay div with repeating-linear-gradient(0deg,
   rgba(0,0,0,.28) 0 1px, transparent 1px 3px), mix-blend-mode: multiply.
3. vignette — overlay with radial-gradient, transparent to rgba(0,0,0,.55)
   at the corners.
4. curve — curvature approximation: ~2.4vmin border-radius on the screen
   plus a strong inset box-shadow (inset 0 0 12vmin rgba(0,0,0,.9)).
5. flicker — keyframe animation on the whole screen, ~3.7s loop, opacity
   dipping irregularly between 1 and 0.94. Barely perceptible.
6. rollbar — a horizontal brightness band (height ~22%, linear-gradient
   transparent → rgba(255,255,255,.04) → transparent) drifting from above
   the screen to below it over ~9s, looping.
7. aberration — chromatic fringing via text-shadow: 1px 0 rgba(255,0,80,.4)
   and -1px 0 rgba(0,140,255,.4). When glow AND aberration are both on,
   combine all four shadows in one rule (text-shadow doesn't stack).
Respect prefers-reduced-motion: disable flicker, rollbar, and cursor blink.

CURSOR
- Solid block, ~0.62em wide × 1.15em tall, phosphor-colored, blinking with
  steps(1) at ~1.06s (hard on/off, no fade). Gets a phosphor box-shadow
  when glow is on.

BOOT SEQUENCE (on load and on reboot)
Typewriter-print these lines character by character with short pauses:
  PHOSPHOR BIOS v2.4          (bright)
  64K PHOSPHOR RAM ....... OK
  DEFLECTION COILS ....... CALIBRATED
  ELECTRON GUN ........... WARM
then a blank line, then:
  PhosphorOS 1.0 — all effect layers are live.
  Type 'help' to see what you can do, 'fx' to dissect the look.
Input is disabled until boot finishes, then the prompt appears.

SHELL BEHAVIOR
- Prompt: guest@phosphor:~$
- Global keydown handling (no visible input element); also keep a hidden
  <input> focused so mobile soft keyboards work, and refocus it on click.
- Enter submits; Backspace edits; ArrowUp/ArrowDown walk command history;
  Ctrl+L clears the screen; Ctrl+C prints the current line with ^C and
  cancels it. Auto-scroll to bottom on every print.

COMMANDS
- help — list all commands.
- about — explains the layer anatomy: one working shell at the bottom,
  overlays floating above it, glow/fringing as text-shadows on the type;
  the look is mostly glow + scanlines at low opacity, everything else
  barely perceptible.
- fx — table of all 7 layers, their on/off state, and a one-line
  description of each.
- fx <name> on|off — toggle one layer; fx all on|off — everything at once.
- theme green|amber|ice — swap the phosphor variable (#33ff33, #ffb000,
  #9fd8ff); the whole screen retints via the CSS variable.
- echo <text>, date, whoami (replies "guest (aren't we all)"), clear.
- reboot — power-off animation: the screen collapses vertically to a thin
  bright horizontal line, then to a dot, brightness flaring up (~0.5s,
  transform: scale + filter: brightness), goes dark ~1s, then re-runs the
  boot sequence.
- Easter eggs: ls shows README.txt; cat README.txt prints the about text.
- Unknown command → "<cmd>: command not found — try 'help'".

TONE
Restraint is the design: 90% of the look is glow + scanlines at low
opacity; flicker, rollbar, and aberration should be felt, not noticed.
