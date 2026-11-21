---
title: Viola plugin tutorial
parent: plugins
order: 2
permalink: /plugins/viola/
---

Build a viola plugin that asks the learner to name a note on the D string. You’ll reuse the artwork and recordings from the example pack, write your own manifest, and add a second exercise that asks learners to identify the bridge.

## Working folder

Download <a href="https://github.com/altosaurus/viola-plugin-tutorial/releases/latest/download/viola.zip">the example viola pack</a>. Find the directory that contains Alto’s database, `alto.db`, and create a `plugins` folder there if needed. See [database locations]({{ '/data/' | relative_url }}).

Inside `plugins`, create a folder named `my-strings` and extract the ZIP directly into it. Keep `CREDITS.txt` with the media. These are the files you’ll use for the first exercise.

```text
my-strings/
  pack.json
  images/viola-front.webp
  animations/string.gif
  sounds/D4.wav
  sounds/E4.wav
  CREDITS.txt
```

## D-string exercise

Open the extracted `pack.json` in a plain-text editor. Replace its contents with this working example. It uses the picture and the D4 and E4 recordings you just extracted.

```json
{
  "format": 2,
  "id": "my-strings",
  "name": "My Strings",
  "instruments": [
    {
      "id": "studio-viola",
      "name": "Studio viola",
      "image": "images/viola-front.webp",
      "sustained": true,
      "strings": [
        {"open": "C3", "nut": [399, 414], "bridge": [1517, 446]},
        {"open": "G3", "nut": [399, 391], "bridge": [1517, 412]},
        {"open": "D4", "nut": [399, 371], "bridge": [1517, 373]},
        {"open": "A4", "nut": [399, 353], "bridge": [1517, 337]}
      ],
      "correct_answer_animation": {"file": "animations/string.gif", "anchor": "string"},
      "samples": {"D4": "sounds/D4.wav", "E4": "sounds/E4.wav"},
      "gain": 0.85
    }
  ],
  "exercises": [
    {
      "id": "d-string",
      "title": "First Finger on the Viola D String",
      "type": "fingerboard-notes",
      "instrument": "studio-viola",
      "notes": {"D4": ["E4"]}
    }
  ]
}
```

Save and restart Alto. Open **First Finger on the Viola D String**. This one-note check always shows E4 at the second guide on the D string. With sound enabled, a correct answer plays `sounds/E4.wav`. Change `["E4"]` to `["D4", "E4"]`, save, and restart to practice both open D and E with their own recordings.

## Fingerboard calibration

1. **Picture.** Put a straight-on photograph or drawing of your instrument in `images/`. Show the whole playable length from nut to bridge. Set `image` to that file’s path. PNG, WebP, and JPEG are supported.
2. **Strings.** Open the final image in an editor that displays the pointer’s pixel coordinates. On each string, record the point where it leaves the nut and the point where it meets the bridge. Enter each as `[x, y]`, measured **from the top-left corner of the original image**. List the strings from lowest open pitch to highest. Each string has its own two points, so a tapered neck or a diagonally framed picture works.
3. **Recordings.** Put isolated, pitched recordings in `sounds/`. Export each as **mono, 16-bit PCM WAV**. Identify the actual recorded pitch in `samples`. For instance, `"G3": "sounds/plucked-g.wav"` pairs that recording with G3. One recording is enough to start. Additional recordings across the range reduce how far Alto shifts a neighboring sample. Say where the recordings came from in `sound_info`, and keep their license and credits in the pack.
4. **Exercise.** Give your instrument its own `id`. In an exercise, set `instrument` to that ID and choose the playable pitches under `notes`. For a pack with two instruments, add a second entry to `instruments`, with its own ID, picture, endpoints, and recordings. Each exercise selects one instrument. Several exercises can select the same one.
5. **Calibration check.** Temporarily ask only for an open string and the note one octave above it, such as `"G3": ["G3", "G4"]`. The open-string marker should sit on the nut and the octave marker halfway to the bridge on that same string. If both are displaced, recheck the image’s original dimensions and your coordinates.

## A picture question about the bridge

For a keyboard, wind instrument, or percussion instrument, supply an image and mark the location for each question directly. An instrument used only for picture questions can omit `strings`. A question about a part can be silent. To play a pitch when the learner answers correctly, add a `note` such as `"C4"` and include a recording under the instrument’s `samples`.

To try this with the starter folder above, add a comma after the first object in `exercises` and add this second object. Save, restart, and open **Parts of My Viola**. The point marks the bridge on the original 2040 × 768 image. For your own instrument, use its ID, pick a point on your picture, and write the choices the learner should see.

```json
{
  "id": "parts",
  "title": "Parts of My Viola",
  "instrument": "studio-viola",
  "type": "image-choice",
  "questions": [
    {
      "prompt": "Which part of the viola is marked?",
      "point": [1500, 400],
      "choices": ["Scroll", "Bridge", "Tailpiece"],
      "answer": "Bridge"
    }
  ]
}
```

{% include shot.html src="plugin-viola-parts.png" alt="Parts of My Viola shows a blue marker on the bridge, with Scroll, Bridge, and Tailpiece as the answer choices." caption="In Instruments → Parts of My Viola, the blue marker sits on the bridge where it supports the strings. Choose Bridge." %}

## Pack installation

To test the plugin you built, compress the contents of `my-strings` into `my-strings.zip`. Open the ZIP and check that `pack.json` is at its root. Move the editable folder outside Alto’s `plugins` folder so only one copy of the pack will load. Install the ZIP through **Menu → Tools → Plugins… → Install plugin from file…** and restart. Open both of your exercises under **Instruments** and check the D-string playback and bridge marker.

## The complete viola example

The original download shows how the same two exercise types work with four strings, two photographs, and a full set of samples. Install the original `viola.zip` through the Plugins dialog and restart Alto. Open **Instruments → Viola Note Identification** and turn on the exercise speaker.

To listen to the installed recordings, open **Menu → Settings → Sound**, select **Bowed viola**, and press **Hear it**. Its source is **Viola Instrument Pack**, with 49 notes from C3 to C7. Choose **Use this instrument** to play these recordings in other exercises that use your selected sound.

{% include shot.html src="plugin-viola-sound.png" alt="Alto’s Sound settings with Bowed viola selected and Viola Instrument Pack listed as its source." caption="Settings → Sound lists Bowed viola alongside the built-in instruments. Select it to preview the recordings with Hear it." %}

On a correct fingerboard answer, the marked string vibrates from the dot to the bridge while the note plays. The pack also adds **Parts of the Viola**, where learners name 30 marked parts: the scroll, pegbox, pegs, nut, fingerboard, each string, bridge, f-holes, tailpiece, fine tuner, bouts, corner, purfling, and top plate on the front, and the neck, button, back plate, and ribs on a second photograph of the back. Small parts are shown close up.

The four open strings in the picture are C, G, D, and A from bottom to top. The **nut** is the ridge where the strings enter the black fingerboard. A dot there means to play the open string. A ruler above the neck shows where to look: each small tick raises the pitch by one half step, and each labeled tick marks a position that the pack names. The viola example names **1st** to **7th** position, where the first finger lands. A faint line leads from the dot to its tick. Here the dot is under the **1st** position tick on the D string, two half steps above open D. Choose **E**.

<picture class="viola-example">
  <source media="(max-width: 700px)" srcset="{{ '/assets/plugins/viola-fingerboard-detail.png' | relative_url }}">
  <img src="{{ '/assets/plugins/viola-fingerboard.png' | relative_url }}" width="720" alt="Close view of the viola supplied by the example pack, with E marked on the D string under the 1st position tick.">
</picture>

[View the full-size viola artwork without the exercise markings]({{ '/assets/plugins/viola/images/viola-front.webp' | relative_url }}), or [the view of its back]({{ '/assets/plugins/viola/images/viola-back.webp' | relative_url }}).

The pack includes 49 bowed viola recordings from IRCAM’s TinySOL collection. Each semitone from C3 through C7 has its own one-second sample. The fingerboard exercise uses the recording for the marked pitch, including when that pitch appears on more than one string. TinySOL’s CC BY 4.0 license allows you to adapt and redistribute the audio with attribution. Keep `CREDITS.txt` with the recordings and describe any further edits. [TinySOL recordings](https://doi.org/10.5281/zenodo.3685367) · [CC BY 4.0 terms](https://creativecommons.org/licenses/by/4.0/)

The artwork is a photograph of an 1893 viola by August Martin Gemünder, from the Metropolitan Museum of Art’s Open Access collection (public domain). Both its front and its back were cut out from their studio backdrops and turned on their sides. The museum lists a 36.9 cm vibrating string length and a 67 cm overall length, and the picture matches its bout measurements. [Museum record](https://www.metmuseum.org/art/collection/search/504403)
