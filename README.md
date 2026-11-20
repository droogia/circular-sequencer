# Circular Sequencer

A polyphonic step sequencer laid out as concentric rings, in a single self-contained HTML file.
Up to six sequencer **units** (circles) run side by side on one shared clock, each with its own rings and step counts.
Each ring is an independent loop with its own step count and length, so polyrhythms show up
directly on the circle. Each step can hold a single note or a full chord.

**Version:** v1.3

## Usage

Open `circular-sequencer.html` in Chrome or Edge. Nothing to install.

| Action | Effect |
|---|---|
| Click an empty cell | Stamp the ring's brush (the last chord edited on that ring) |
| Click a filled cell | Select it for editing |
| Drag | Paint the brush along the ring |
| Right-click / right-drag | Erase |
| Mouse wheel on a cell | Transpose by a semitone (Shift: octave) |
| Space / click the centre | Play / stop |
| ← → | Move the selection |
| ↑ ↓ | Transpose the selection (Shift: octave) |
| Del | Clear the selected step |
| Esc | Deselect |
| 1–6 | Select unit |
| P | Show / hide side panel |
| F | Full screen |

### Step editor
- Mini keyboard (3 octaves, shiftable): click keys to toggle notes in the selected step's chord.
- Chord presets (maj, min, 7, maj7, m7, sus2, sus4, dim, aug, 5, 8va) built on the lowest note.
- Inversions, transpose, velocity, gate length (in steps) and probability per step.

### Units
- Up to 6 circles, auto-arranged in the grid that gives the largest circles (3×2 for six).
- All units share the global clock and downbeat; click a unit (or press 1–6) to edit it in the side panel.
- Each unit is either **Synth** (chords, preview synth) or **Drums** (one drum sound per ring).
- Tiny **M** / **S** buttons on each disc for unit mute / solo.
- Per unit: type, number of concentric rings (1–8; extra rings are added inside, reducing removes the innermost), mute, solo, duplicate, delete.

### Drums
- Kit: kick, snare, closed/open hat, clap, low/high tom, rimshot, cowbell, crash — synthesized, with per-ring Tune and Decay.
- Drum sounds use General MIDI note numbers, ready for MIDI channel 10.
- On drum rings the mouse wheel and ↑/↓ change velocity instead of pitch.
- Switching a unit between Synth and Drums is non-destructive: stored notes are kept.

### Rings
- Steps (1–64) and loop length in beats (0.25–64), independent per ring.
- Preview synth per ring: waveform, volume, low-pass cutoff and resonance, attack, release.
- Mute / solo, Euclidean fill, rotate, clear. Up to 8 rings.

### Saving
The pattern autosaves to the browser's local storage. **Save JSON** / **Load JSON** move patterns between machines.

## How it works
- **Data model:** each event stores its position as a phase (0–1) around its ring, plus notes, velocity, gate and probability.
  This keeps the model independent of the step grid and ready for MIDI out and Strudel export.
- **Timing:** a lookahead scheduler wakes every 25 ms and schedules everything in the next 120 ms on the audio clock.
  A global beat counter keeps every ring aligned to the same downbeat and makes BPM changes smooth.
- **Sound:** built-in WebAudio preview synth (oscillator → low-pass filter with a short envelope → amp envelope), through a master compressor.

## Roadmap
- MIDI output via the Web MIDI API (per-ring channel, hardware synths / loopMIDI)
- MIDI clock out / sync
- Strudel export
- Off-grid (unquantized) note placement
- Per-unit tempo ratio and start/stop

## Changelog
- **v1.3** — Synth/Drums unit type with a built-in drum kit; tiny mute/solo buttons on each disc; demo gains a drum unit.
- **v1.2** — Rings count per unit, set directly from the unit header.
- **v1.1** — Units: up to 6 synchronized circles in a full-screen grid, unit mute/solo/duplicate/delete, panel toggle, full screen. v1.0 patterns load as a single unit.
- **v1.0** — First release: concentric rings, chords per step, preview synth, step editor, Euclidean fill, JSON save/load.
