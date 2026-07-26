# GRiDS Manual

A synthwave and darksynth groovebox for the Nintendo DS.

Inside there's a paraphonic PAD, a supersaw LEAD, a sub-heavy BASS, and an 80s
drum machine. A chord-aware 32-step sequencer holds it all together, with song
mode, stereo chorus, gated snares and a kick-synced pump.

Everything you hear is synthesized on the DS. PAD, LEAD and BASS render in
software on the ARM7 at 32768 Hz in stereo. The drums get built from analog
recipes at boot and fired on hardware channels. There isn't a single sample in
the ROM.

---

## Contents

1. [What you need](#1-what-you-need)
2. [Five minutes in](#2-five-minutes-in)
3. [The two screens](#3-the-two-screens)
4. [Hardware buttons](#4-hardware-buttons)
5. [The tabs](#5-the-tabs)
6. [Chord lock](#6-chord-lock)
7. [Automation](#7-automation)
8. [Playing it over MIDI](#8-playing-it-over-midi)
9. [Saving](#9-saving)
10. [Reading the interface](#10-reading-the-interface)
11. [If something seems broken](#11-if-something-seems-broken)
12. [Credits and legal](#12-credits-and-legal)

---

## 1. What you need

A Nintendo DS, DS Lite, DSi or 3DS with a flashcart, or an emulator. melonDS is
the one to use, since GRiDS drives the sound-capture hardware to feed its scope
and not every emulator implements that.

An SD card if you want to save. GRiDS runs perfectly well without one, you just
can't keep anything you make.

Optionally an mDS-style slot-2 MIDI cart, which gets you MIDI in, thru and clock.

Copy the .nds to your card and launch it. First boot takes a few seconds because
the drum kit is being synthesized right then, and the splash runs a progress bar
while that happens.

### Which build to use

| File | What it is |
|---|---|
| `grids.nds` | The instrument with an empty pattern bank. Start here. |
| `grids-night.nds` | Ships with NIGHTDRIVE, the factory demo |
| `grids-brut.nds` | Ships with BRUTALIZER, a darker one |
| `grids-neon.nds` | Ships with NEON CITY, which tours most of the features |
| `grids-comp.nds` | PASSENGER, a companion part meant to play on a second DS alongside `grids-night` |
| `grids-trial.nds` | Trial build. MIDI and saving are off. |

The demo builds aren't cut down in any way. They're the whole instrument with a
song already loaded, so load one, hit play, and then pull it apart to see how it
was put together.

---

## 2. Five minutes in

GRiDS opens on SEQ with the transport in front of you.

1. Press PLAY. On an empty build you won't hear anything, because there are no
   notes yet.
2. Go to CHD and lay down a progression. Tap a cell, dial ROOT and TYPE with the
   -/+ spinners, and press SET. You'll hear each chord as you dial. Do two or
   three cells spread across the pattern.
3. Now check your pattern length. This trips up everybody once. The chord grid
   always draws all 32 cells, but a new pattern is only 16 steps long, so the
   back half is dimmed and will never play. Set LEN on the SEQ transport to 32
   if you want the whole grid.
4. Back on SEQ, look at the box on the right of the editor row. It says LEAD or
   BASS, and that's the track you're writing to. Tap the grid to place notes;
   the keyboard along the bottom sets their pitch.
5. Turn the locks on. Back on CHD, set SNAP to CHORD and FOLLOW to ROOT. Your
   lead now re-pitches to each chord and the bass follows the root, so one
   written riff gets moved around by the progression.
6. Add drums on DRM, then go shape the sound on OSC, FLT and FX.

That's the core loop. The rest of the manual is detail on top of it.

---

## 3. The two screens

The top screen is the live panel. Filter response curve, the device dashboard
(waveform lights, ENV/ACC/DRIVE/VOL bars, filter target), a middle panel you can
switch, the 32-step pattern strip with the playhead, and a status line carrying
pattern, song slot, DSP load and BPM.

The bottom screen is the editor. Tabs across the top, the current page below
them, and a performance strip along the bottom holding a touch keyboard, a pitch
wheel that springs back to centre, a mod wheel, and octave up/down.

That strip is on every page except DRM and SET, which need the room for
themselves.

---

## 4. Hardware buttons

| Button | Action |
|---|---|
| START | Start and stop the pattern. Song mode plays from the SNG page. |
| A / B | Copy and paste the current pattern |
| X / Y | Undo and redo, 8 deep, covering every destructive edit |
| D-pad | Move the selected cell. Hold to repeat. |
| L | Switch the top screen's middle panel between scope and automation lanes |
| SELECT | Panic. Kills every voice instantly, from any page, without stopping the transport. |

---

## 5. The tabs

The bar runs left to right in three colour-coded groups.

CHD SEQ DRM KIT SNG are cyan. This is what you play, laid out in the order the
data actually flows: the chord lane feeds the tracks, the tracks and drums make
the pattern, the kit sculpts the drums, and SNG arranges the patterns.

OSC FLT MOD FX MIX are magenta. This is how it sounds, in signal order:
oscillator, filter, modulation, effects, mix.

SAV SET are neutral. Housekeeping.

The rule under the bar repeats those three zones, and whichever tab you're on is
the filled one.

### CHD, the progression

Two rows of 16 cells hold the chord lane. Tap a cell to select it and hear it,
dial ROOT, TYPE and INV, then press SET to write it. The spinners preview as you
go, so nudging one (or just tapping its value) sounds whatever chord you've got
dialled up.

Chord types are maj, min, sus2, sus4, min7, maj7, dom7, dim and add9.

INV is the inversion, meaning which note of the chord ends up at the bottom of
the voicing. It runs from 0 up to one less than the chord's tone count, so
triads reach 2nd inversion and 7th chords reach 3rd. Inversions are what let
consecutive chords move by small steps instead of leaping around. They change
the PAD voicing only, not the bass or the lead.

A chord holds until the next chord event, and that carries across patterns.

LATCH decides how long a tapped chord rings. Off, which is the default, means it
sounds only while you're holding it. On means it keeps going after you lift, so
you can set a progression going hands-free and play over it. Switching LATCH off
drops whatever it's currently holding, so that's your stop button.

CLR clears the selected chord. CLR ALL clears the whole lane.

Only steps inside the pattern's LEN will fire. The grid always draws all 32
cells and dims the ones past the end.

### SEQ, the note grid

The 32-step grid for LEAD and BASS, shown as two pages of 16.

LEAD / BASS on the right of the editor row picks the track you're writing.
PG1 / PG2 on the left picks which half of the 32 steps you're looking at. It is
not a track selector, which is worth saying because it looks like one.

Each step carries velocity (112 and above counts as an accent), gate, ratchet of
1 to 4 hits, probability, TIE and SLIDE.

The transport row has PLAY, BPM, LEN, SWG (swing, stored per pattern), REC, CLR
and ALL.

REC cycles through three states. OFF, then REC for live and step recording where
control moves also become automation, then LOC for parameter lock, where the
keyboard sets the selected step's note and control moves write to that step
only.

CLR clears the selected step. ALL clears the track lane and all four motion
lanes, and it asks first.

### DRM, the drum machine

Eight lanes on a 16-step view, with PG1/PG2 for the other half. Tapping a cell
cycles it through off, ghost, hit and accent. Every step also carries ratchet
and probability, and every lane has mute and solo.

The editor row reflects whatever cell you last tapped: its lane and step, PAN
across five stage positions from L2 to R2, ratchet, probability, and the TUNE
and DECAY of that lane's drum.

This page hides the keyboard and gives the grid the whole screen.

### KIT, the kit lab

One row per lane. TUNE goes down as far as -2 octaves, which matters because
pitched-down drums are half the darksynth vocabulary. Then DEC, VOL and PAN,
plus AMB for a baked room and GATE.

The drums play on hardware channels that no live effect can touch, so the reverb
has to be printed into the one-shot itself. That's why this page has a fence
down the middle. Columns to the left of it are live. The magenta AMB and GATE
columns are bake-time, so edits there sit pending and glowing until you press
BAKE, which re-synthesizes the changed drums behind a short modal. Tap a lane's
name to audition it.

There are four kits. NIGHT is clean and deep, CHROME is bright and snappy, BRUT
is driven hard, and NEON is the big 80s gated sound. You pick between them on
the SET page.

### SNG, song mode

Chains up to 64 slots. Each slot holds a pattern, a repeat count, synth and drum
mutes, and an optional tempo override. LOOP decides whether the chain loops or
stops at the end. CLR clears the selected slot, ALL clears the arrangement and
asks first.

### OSC, device timbre

WAVE picks saw or square, and on the LEAD it swaps the whole unison stack over
to hollow squares. Then ENV MOD, ACCENT, DRIVE and DECAY, plus one column that
changes depending on the device: DET for supersaw spread on LEAD, SUB for
sub-oscillator level on BASS.

The LEAD / BASS / PAD tabs under the sliders choose which device you're editing.
That includes the PAD, the chord synth, which has no sequencer track of its own
but exactly the same timbre controls as the other two.

Picking LEAD or BASS here also moves the sequencer's selected track. Picking PAD
only re-points the sliders, so your keyboard and grid stay put.

### FLT, the filter

An XY pad where X is cutoff and Y is resonance. FILTER TARGET routes it to LEAD,
BASS or BOTH, since each device has its own filter. The PAD's filter is reached
from the OSC page instead, or by MIDI CC.

### MOD, glide and arpeggiator

SLID is portamento for LEAD and BASS. ARP sets the mode: up, down, up-down or
random. ARPR sets the rate: 1/8, 1/16, 1/16T or 1/32.

The big ARP ON/OFF button here is the only switch that turns the arpeggiator on.
The ARP cell over on the CHD page just chooses where it gets its notes from.

Don't confuse any of this with the mod wheel on the performance strip. That adds
an ENV MOD offset on top of every device's own setting, so raising it opens all
three at once without wrecking the per-device amounts you dialled in on OSC. At
rest it does nothing, and loading a preset puts it back to rest.

### FX, the rack

DELAY, where TIME picks a musical division and stays locked to it when you
change tempo, plus feedback and mix. CHORUS, the stereo ensemble, with rate,
depth and mix. PUMP, the kick-synced duck, with depth and release.

### MIX, levels

SYN for the synth bus, then PAD, LEAD and BASS device levels, DRM for the drum
master and MST for everything. Mute and solo per machine. Under each device
fader is its PAN pill, L2 through C to R2. Pans record as automation, so an
auto-panned lead is one REC pass away.

### SAV, songs

Sixteen named slots on SD. Each one is a complete snapshot: every pattern
including the chord lane, the chain, the kit, and all parameters. NEW puts
everything back to factory-fresh.

### SET, settings

MIDI channel, thru, clock in and out, clock ratio, the drum kit picker, drum
sync defer, and TOP VIEW, which chooses what the top screen's middle panel
shows. This page hides the keyboard too.

---

## 6. Chord lock

Lay a progression on CHD, then lock the other voices to it using the row along
the bottom of that page. Each header is coloured for the device it acts on: cyan
for LEAD, magenta for BASS.

| Cell | Acts on | What it does |
|---|---|---|
| SNAP | LEAD | OFF / CHORD / SCALE. Pulls LEAD notes onto the nearest tone of the latched chord, or onto KEY plus SCALE. Nearest wins, and ties resolve downward, so it voice-leads instead of jumping. Applies to sequenced LEAD notes and to the touch keyboard while LEAD is the selected track. |
| KEY / SCALE | LEAD | The key root and parent scale: minor, phrygian, dorian, major. These do nothing unless SNAP is set to SCALE, and the page greys them out to say so. |
| FOLLOW | BASS | OFF / ROOT / R+5 / WALK. The track gives the rhythm and register, the chord gives the pitch. Sequenced BASS notes only, never the keyboard. |
| ARP | the arpeggiator | CHORD makes the arp voice the chord lane instead of your held keys. Needs the arpeggiator switched on over on MOD, and the transport running. |

### Getting FOLLOW right

You still have to place a note on every step you want the bass to sound, because
FOLLOW supplies pitch and not rhythm.

But the pitch you write barely matters. It gets replaced by the chord root and
lands within a few semitones of whatever you put there. All the written note
really does is choose the register.

So the practical approach is: pick one note, write it on every step the bass
should hit, leave rests where you want gaps, and let FOLLOW handle the pitching.
WALK turns that single repeated note into a rotating root, fifth, octave, third
figure that re-voices itself at every chord change. That's precisely how the
bassline in the PASSENGER demo is built.

Use the same note all the way through if you want a consistent register. Octave
placement is relative to what you wrote, so jumping around can shift the result
by an octave.

---

## 7. Automation

Press REC while playing and move a control. The move records into one of the
pattern's four motion lanes and replays on every pass. LOC mode writes to the
selected step instead, which is a p-lock.

Cutoff and resonance, ENV MOD, the FX rack, the chord lock cells, the device
pans, and drum TUNE, DECAY and PAN all record.

An automated control wears a magenta dot. Solid means its lane is driving it.
Hollow means you've overridden it by hand, so the lane is still assigned but
sitting quiet until the pattern changes. There are only four lanes, so the dots
are also how you see which four are spoken for.

A cell with a dot in the SEQ grid has a value at that step.

Press L for a live view of all four lanes on the top screen: each lane's
parameter, its shape across the 32 steps, and the value sitting under the
playhead right now. A free lane reads `---`.

Clearing is scoped. CLR is this one, ALL is everything, and ALL asks first. A
cleared step doesn't go silent, because motion values hold across gaps, so it
just inherits whatever came before it.

You never have to erase anything to audition a change. Moving a control by hand
overrides its lane until the pattern comes round again.

---

## 8. Playing it over MIDI

With an mDS-style cart in slot 2, GRiDS is three-part multitimbral. Counting up
from the base channel you set on the SET page:

| Channel | Part |
|---|---|
| base | LEAD |
| base + 1 | BASS |
| base + 2 | chords |
| 10 | the drum kit, GM notes |

Set the base channel to 13 or lower so all three parts fit. OMNI collapses
everything onto the selected track.

The chord channel is the interesting one. A note there is a chord root rather
than a pad note, and it fires the same chord event the CHD lane fires. So the
PAD voices it and it latches, which means LEAD snap, BASS follow and ARP chord
all track a chord you play from a keyboard exactly the way they track a
sequenced one. Play a triad and the lowest held note is taken as the root, so
you state one chord rather than three. Release honours LATCH.

The chord's quality comes from CC. CC25 picks the type, CC26 the inversion.

### CC map

| CC | Parameter | CC | Parameter |
|---|---|---|---|
| 1 | ENV MOD (mod wheel) | 74 | CUTOFF |
| 71 | RESO | 75 | DECAY |
| 79 | GLIDE | 88 | Master volume |
| 90 | WAVE | 94 | DETUNE |
| 95 | UNISON (raw count, 1 to 7) | 103 | SUB |
| 73 / 70 / 72 | ATK / SUS / REL | 102 | Filter target |
| 104 / 105 | ARP mode / rate | 107 to 109 | Delay time / feedback / mix |
| 110 to 112 | Chorus rate / depth / mix | 116 / 117 | Pump depth / release |
| 20 to 24 | KEY / SCALE / SNAP / FOLLOW / ARP | 25 / 26 | Chord type / inversion |
| 8 / 9 / 10 | Pan for PAD / BASS / LEAD | 7 | This channel's device level |

Also honoured: CC120 and CC123 for all sound off, CC121 to reset controllers,
pitch bend at plus or minus 2 semitones, and MIDI clock both in and out with
start and stop.

---

## 9. Saving

Songs save to SD in 16 named slots on the SAV page. A slot is a complete
snapshot: every pattern including the chord lane, the chain, the drum kit and
all parameters.

Without an SD card everything still works, you just can't save. NEW wipes back
to factory-fresh, meaning an empty bank, the default patch and 128 BPM, and it
asks before it does it.

Undo on X covers every destructive edit and goes eight deep.

---

## 10. Reading the interface

GRiDS uses one colour language, and it's the same on both screens.

| | Meaning |
|---|---|
| Cyan | Engaged, audible, on. Also the LEAD device. |
| Magenta fill | Soloed or armed: REC, ARP ON, the active tab. Also the BASS device. |
| Magenta frame | Destructive. These are the buttons that ask before they act. |
| Green | The PAD device |
| Dark | Muted |
| White frame | The current selection |
| Dim grey | Present but inert, so a control that isn't doing anything right now |

Two other conventions are worth knowing.

A cyan rim means you can tap it. Every changeable value box has one and passive
readouts don't. Buttons with verbs on them (PLAY, SAVE, CLR) don't need one.

Greyed out doesn't mean disabled. KEY and SCALE grey out when SNAP isn't on
SCALE, and the CHD ARP cell greys when the arpeggiator is off, but all of them
stay tappable so you can set things up before you switch on the thing that uses
them.

---

## 11. Credits and legal

GRiDS is an original work for the Nintendo DS. All the synthesis is original.
The voices, the drum recipes and the effects were all written from scratch for
this machine.

No samples are used anywhere in this ROM. The drum kit is synthesized from
analog recipes at every boot.

Not affiliated with, endorsed by, or derived from Roland, Sequential, Oberheim,
Nintendo or any other manufacturer. No third-party samples, trademarks or trade
dress are used. Any model numbers mentioned in genre descriptions are there to
describe a sound and nothing else.

Built with devkitARM and calico.
