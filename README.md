# Sound Symbolism Dataset

Per-set dataset for the sound symbolism human experiment. Each folder contains everything
for one stimulus set: the image set, the subject's
responses, and their eye-tracking recording.

## Layout

```
sound symbolism data collection/
  set01/
    images/                              186 renamed stimulus images
      pair95_round_left_KEEPEE.png
      ...
    response_set01_P1                    subject P1's responses (Google Sheet)
    set01_P01_S1_gaze_data.json          eye-tracking samples, session 1
    set01_P01_S1_frame_timestamps.json   screen-recording frame timing, session 1
    set01_P01_S2_gaze_data.json
    set01_P01_S2_frame_timestamps.json
  set02/
  ...
  set55/
```

| | |
|---|---|
| Set folders | 55 (`set01` … `set55`) |
| Images | 10,230 (186 per set) |
| Response sheets | 55 |
| Eye-tracking JSON files | 318 |

One subject per set, and the numbering lines up throughout: set *N* holds
subject P*N*'s data. 

## Images

`images/` holds one renamed copy of every image that set's trials actually use.
Filenames follow:

```
pair{original_pair}_{layout}_{word}.png

pair95_round_left_KEEPEE.png     pair 95, round shape on the left, word "KEEPEE"
pair102_sharp_left_sharp.png     pair 102, sharp shape on the left, word "sharp"
```

- `layout` is `round_left` or `sharp_left` — which side the rounded shape appeared on.
- `word` is the label shown on that trial. Three kinds appear (see `word_type` below);
  pseudowords are uppercase, adjectives lowercase.

Images are copies of a shared master library, renamed per set so each set's folder
is self-contained. The same master image recurs across sets under different names
whenever it was paired with a different word.

## Response sheets

`response_set{NN}_P{subject}` is a Google Sheet, one tab, 186 rows — one per trial,
ordered by `trial_index`.

| Column | Meaning |
|---|---|
| `set_number` | stimulus set (matches the folder) |
| `trial_index` | trial order within the set |
| `master_image_file` | original master-library image name |
| `layout` | `round-left` or `sharp-left` (note: hyphens here, underscores in filenames) |
| `word` | the label shown |
| `word_type` | `pseudoword`, `adjective`, or `canonical` |
| `correct_side` | expected/keyed response side |
| `original_pair` | shape-pair id, ties trials using the same image pair |
| `user_choice` | **the subject's response** |

Roughly 162 pseudoword, 20 adjective and 4 canonical trials per set.

## Eye-tracking data

Three JSON files per recording session, named `set{NN}_P{NN}_S{session}_*.json`.

**`*_gaze_data.json`** — raw gaze samples (~10 MB per session):

```json
{
  "screen_width": 2560,
  "screen_height": 1440,
  "samples": [
    {"timestamp": 1777320698.020421,
     "gaze_x": 966, "gaze_y": 1280,
     "smooth_x": 966, "smooth_y": 1280,
     "left_pupil": 2.9036, "right_pupil": 3.0191}
  ]
}
```

`gaze_x`/`gaze_y` are raw screen coordinates; `smooth_x`/`smooth_y` are the smoothed
track. Timestamps are Unix epoch seconds.

**`*_frame_timestamps.json`** — screen-recording frame timing (~0.4 MB per session):

```json
{
  "recording_start": 1777321345.165566,
  "fps": 15, "width": 2560, "height": 1440,
  "scale_factor": 1.0, "frame_count": 6114,
  "frames": [{"frame": 0, "timestamp": 1777321345.1706939}]
}
```

Both files share the same epoch clock, so gaze samples align to video frames by
timestamp.

### Coverage caveats

- **Sets 4 and 28 have no gaze data per technical failures.** Response sheets and stimuli are present.
- **`set01`** has only two sessions, `S1`–`S2`.
- **`set34`** has `S1` split across two files — `set34_P34_S1_1_gaze_data.json` and
  `set34_P34_S1_2_gaze_data.json`

