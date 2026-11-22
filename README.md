# Circular Sequencer

A polyphonic step sequencer laid out as concentric rings, in a single self-contained HTML file.
Up to six sequencer **units** (discs) run side by side on one shared clock, each with its own rings.
Every ring is an independent loop with its own step count and length, so polyrhythms show up
directly on the disc. A ring plays a single synth note, a synth chord, or a drum sound per step.
Each unit holds up to eight patterns (A–H), and a **song** chains them into sections.

**Version:** v3.0.1

## Usage

**Live:** https://droogia.github.io/circular-sequencer/

Or download `index.html` and open it locally in Chrome or Edge. Nothing to install.

| Action | Effect |
|---|---|
| Space / **Play all** | Start / stop the clock and every unit |
| Click the centre of a disc / Shift+1–6 | Start / stop only that unit; it joins in sync at the current global position |
| Drag a disc by its title bar | Move the unit to another place in the grid |
| **A–H** on a disc (or in the panel) | Choose the unit's pattern; while playing the change is queued (blinking amber) |
| Tiny **M** / **S** on a disc | Unit mute / solo |
| Click an empty cell | Stamp the ring's brush (the last note, chord or hit edited on that ring) |
| Click a filled cell | Select it for editing |
| Drag | Paint the brush along the ring |
| Right-click / right-drag | Erase |
| Mouse wheel on a cell | Transpose by a semitone (Shift: octave); on drum rings: velocity |
| ← → | Move the selection |
| ↑ ↓ | Transpose (Shift: octave); on drum rings: velocity |
| Del | Clear the selected step |
| Esc | Deselect |
| 1–6 | Select unit |
| P | Show / hide side panel |
| F | Full screen |

A running unit has a dark green centre. While the clock runs, stopped units show a faint playhead,
so you can see where they will come in.

### Patterns
- Each unit has 8 pattern slots, A–H. A pattern is the unit's complete set of rings (notes and sounds).
- Clicking an empty slot creates a pattern with the same rings and no notes; **Copy to next** duplicates the current one.
- While playing, a change waits for the next bar — or, with **Switch at loop end** ticked,
  for the end of the unit's loop (its longest ring).

### Song mode
- Switch **Pattern / Song** in the header. The song is a grid: one row per section, one column per unit,
  each cell a pattern letter or "–" (silent), plus the section length in bars. One tempo per song.
- Space plays the song from the start section (▶ in the grid, or click a section in the strip below the discs).
  Clicking a section while playing jumps there at the next bar. **Loop** repeats the song.
- Section changes are scheduled on the audio clock, so they land exactly on the bar.
- Rings keep flowing across sections: odd-length loops (5, 7, 9 steps…) do not restart.

### Example songs
Eight example songs, each using all six units with arrangement sections (intro, fills, breaks, outro):
Techno, Deep House, Synthwave, Boom Bap, Ambient, Drum & Bass, Bossa Nova, Afrobeat 12/8.
Pick one from **Load example song…** (it replaces everything).

### Ring types
- **Synth · note** — one note per step, monophonic (each note cuts the previous one).
- **Synth · chord** — any number of notes per step; chord presets and inversions in the step editor.
- **Drum** — one drum sound per ring: kick, snare, closed/open hat, clap, low/high tom, rimshot,
  cowbell, crash. Synthesized, with per-ring Tune and Decay. Uses General MIDI note numbers (ready for MIDI channel 10).

Switching a ring's type is non-destructive: the stored notes are kept.

### Step editor
- Mini keyboard (3 octaves, shiftable): sets the note (note rings) or toggles chord notes (chord rings).
- Chord presets built on the lowest note, inversions, transpose, velocity, gate length (in steps), probability.

### Units and rings
- Up to 6 units, auto-arranged in the grid that gives the largest discs (3×2 for six).
- Per unit: number of rings (1–8; extra rings are added inside, reducing removes the innermost), mute, solo, duplicate, delete.
- Per ring: type, steps (1–64), loop length in beats (0.25–64), volume, sound settings, mute / solo,
  Euclidean fill, rotate, clear.

### Saving
The pattern autosaves to the browser's local storage. **Save JSON** / **Load JSON** move patterns between machines.
Files from older versions load automatically (single patterns become pattern A; unit types become ring types).

## How it works
- **Data model:** each event stores its position as a phase (0–1) around its ring, plus notes, velocity, gate and probability.
  This keeps the model independent of the step grid and ready for MIDI out and Strudel export.
- **Timing:** a lookahead scheduler wakes every 25 ms and schedules everything in the next 120 ms on the audio clock.
  One global beat counter drives every unit, so units started at different times stay aligned.
- **Sound:** built-in WebAudio preview synth (oscillator → low-pass filter with a short envelope → amp envelope)
  and a synthesized drum kit, through a master compressor.

## Roadmap
- MIDI output via the Web MIDI API (per-unit port/channel, hardware synths / loopMIDI)
- MIDI clock out / sync
- Strudel export
- Per-unit tempo ratio
- Off-grid (unquantized) note placement
- Undo

## Changelog
- **v3.0.1** — Rings whose cells are too narrow for upright note names show them radially (whole ring), instead of dots.
- **v3.0** — Patterns A–H per unit with queued switching (next bar or end of unit loop); Song mode with
  section grid, song strip, loop and jump; example songs for all eight styles.
- **v2.0** — Per-unit start/stop in sync with the global clock; Play all; draggable units; ring types
  (Synth · note, Synth · chord, Drum) replace unit types; 8 six-unit demos; green centre while running.
- **v1.3.1** — Main file renamed to `index.html` for GitHub Pages.
- **v1.3** — Synth/Drums unit type with a built-in drum kit; tiny mute/solo buttons on each disc; demo gains a drum unit.
- **v1.2** — Rings count per unit, set directly from the unit header.
- **v1.1** — Units: up to 6 synchronized circles in a full-screen grid, unit mute/solo/duplicate/delete, panel toggle, full screen. v1.0 patterns load as a single unit.
- **v1.0** — First release: concentric rings, chords per step, preview synth, step editor, Euclidean fill, JSON save/load.
