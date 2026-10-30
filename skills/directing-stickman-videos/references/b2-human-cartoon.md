# B2 · Human Cartoon

A narrated, locally rendered short drawn as **full human figures in ink and flat
colour**, not stick figures. B2 is a new option beside B1 and the A styles; it
inherits nothing from them and changes none of them.

First produced as *How Di Music Come* (Banging Music, 2026-10): a Jamaican folk-tale
series where a caveman invents a musical idea and a 1970s yard frames the telling.

## Visual language

- Warm paper `#F2E9D8`, visible grain multiplied over the finished frame at ~16%.
- Loose black ink outlines, flat solid colour fills, **no gradients on the figure**,
  muted ochre and umber palette. Light comes from the fire or the doorway, painted
  into the plate; figures are not shaded per-pose.
- **Full human characters**, with faces, hands and hair. This is the difference
  from A5 and B1: no white-filled heads and no stick limbs. Proportions are
  slightly caricatured (the child has an oversized head) but consistent per role.
- Two worlds per episode: the **frame** (a modern, domestic, lit interior or yard)
  and the **myth** (a cave, fire-lit, darker and warmer). The cut between them is
  hard; there are no dissolves.
- Lettering is one hand: OFL Caveat, shipped in the project, used for the title,
  the cover and the end card. Never a second display face.

## Cast grammar — the part that must not drift

Approved masters are frozen art, purchased once:

- **Star topology.** Every pose is generated *from its master*, never from another
  pose. `gen_cast.py --from <role> --pose "<verb phrase>"` enforces this.
- Each master records a SHA-256 in `assets/cast/manifest.json`. A pose list grows;
  a master changes only by explicit re-approval, because it invalidates every pose
  derived from it.
- Poses are cut out with rembg (u2net, alpha matting). Never flood-fill: it leaves
  halos on hair.
- One **character scale factor** per role (e.g. 0.50 adult, 0.40 child), so a body
  stays the same size across poses. Position by `cx` and `bottom`, not by eye.
- Draw the pose without scenery. If a prompt says "beside a fire", the fire is
  baked into the cutout and floats over the plate.

## Pose budget: one pose per verb

A pose is a **verb**. If the narration says *he flings the fan*, a raised-arm pose
with the fan still in hand is the wrong picture — the action never happens on
screen. Write each shot as subject + verb + object, give every distinct verb its
own pose, and when an object leaves the hand, draw **two** assets: the empty-handed
pose and the object as a separate prop that can be animated. Budget about two
poses per narrated line, plus one or two props a film.

## Composition

HTML rendered by HyperFrames; the timeline is generated, never hand-edited.

- A clip table (`clips.py`) of `(id, src, scale, cx, bottom, start, dur)`; layout
  is computed from each master's real pixels.
- **Pre-scale every image to display size** (`assets/opt/`, ~1.4× display). A few
  dozen full-resolution PNGs exhausts the compositor's image memory and produces
  renders with smeared, banded faces. This is the single most expensive mistake in
  the style.
- Plates get a slow push (1.0 → 1.05) per scene. One idea per scene: a pose change,
  a prop move, or a camera move — not all three.
- Beats that have no sound can be shown by light: a short `brightness()` pulse on
  the plate marks a hit the audience can see.

## Sound

- **The voice sets the clock.** Generate one continuous read, split it at detected
  pauses, snap the scene boundaries to them, and write those numbers into the clip
  table. Never speed the narrator to hit a runtime.
- Music opens the film and closes it; the theme **changes sides** — score at the
  open, then diegetic out of a radio at the payoff, so the audience discovers the
  music was in the room all along.
- Cut diegetic beds from the catalogue's **own stems**: measure the tempo, take a
  whole number of bars, loop with a ~30 ms equal-power seam. Record the source and
  cut points in the clip table.
- Master the finished film, not the mix: `loudnorm=I=-16:TP=-1.5:LRA=11`.

## Brand and publishing

- Every film ends on the house card: three seconds, paper, the URL, the line
  *old soul. new machine.* No other end screen over it.
- Covers are composed from the episode's own plates, mattes and lettering, never
  new generated art, so a cover cannot promise something the film lacks.
- Check the thumbnail at 320 px. One figure, three words.

## Not B2

B2 does not replace or modify A1–A6 or B1. Its cast, plate, punch-in, audio and
brand rules apply only when B2 is selected.
