# AMALGAM
**A Methodically ALgorithmically Generated Aleatoric Melody**

AMALGAM is a Python-based generative music system that synthesizes original melodies and chordal accompaniments directly from a chord progression — no samples, no MIDI, no external synth. Everything from the audio waveform to the note choices is generated algorithmically and rendered straight to sound.

Originally built and run as a Colab notebook.

## What It Does

Given a chord progression (e.g. `Am - F - C - G`), AMALGAM:

1. **Synthesizes audio from scratch** — each note is built additively from sine-wave harmonics, with a different harmonic "recipe" (timbre) for melody, chord, and bass voices.
2. **Generates a chord-based accompaniment** in one of four styles: `block`, `bass_chord`, `arpeggio`, or `waltz`.
3. **Generates an original melody** on top, using music-theoretic rules rather than randomness alone:
   - Chord tones are favored on strong beats, non-chord tones on weak beats
   - A weighted, distance-based note selection keeps the melody centered around a target pitch
   - A leap limiter avoids awkward large jumps
   - Phrase contours (`up`, `down`, `arch`, `wave`) shape the melodic line over each bar
   - Support for major, natural minor, harmonic minor, and melodic minor scales (with proper raised 6th/7th handling)
   - A dedicated cadential "ending sentence" resolves the piece back to the tonic
4. **Mixes and plays** the resulting layers as a single audio buffer.

## Core Components

| Module | Responsibility |
|---|---|
| Audio synthesis | Additive sine-wave synthesis per timbre (melody/chord/bass) |
| Music theory | Scale construction, chord symbol parsing, chord-tone lookup |
| Voice weighting | Distance-weighted note choice, leap avoidance, smooth voice leading |
| Rhythm | Predefined rhythm cells, bar subdivision by "activity" level |
| Melodic grammar | Metric-position-aware note pools, contour-based weighting, passing-tone insertion |
| Accompaniment | Four chord-accompaniment styles with independent voicing logic |
| Ending phase | Fixed cadential pattern to close out the piece |
| Mixing/playback | Combines tracks and plays via `IPython.display.Audio` |

## Requirements

- Python 3
- `numpy`
- `IPython` (for in-notebook audio playback via `Audio`/`display`)

## Usage

Run in a Jupyter/Colab notebook. Define a chord progression as a list of `(chord_symbol, duration_in_ticks)` tuples, then call the generation functions:

```python
progression = [
    ("Am", 8), ("Am", 8),
    ("F", 8), ("F", 8),
    ("C", 8), ("C", 8),
    ("G", 8), ("G", 8),
    ("C", 16)
]

generate_chord_background(
    progression=progression,
    bpm=90,
    style="arpeggio"  # block, bass_chord, arpeggio, or waltz
)

generate_melody(
    progression=progression,
    bpm=90,
    key="C",
    scale_type="major",  # major, nat_minor, harm_minor, melo_minor
    activity=0.6
)

piece = mix_tracks(melody_gain=0.75, chord_gain=1.0)
sound(piece, sr)
```

## Status / Known Issues

- Voice leading in `smooth_voicing` prevents crossing but does not yet fully prevent overlapping voices on the same pitch.
- The `arch` contour uses a simplified convex/concave approximation rather than a true arch-shaped filter.

## License

Add a license of your choice here (e.g. MIT).
