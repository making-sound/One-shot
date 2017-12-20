# One-shot

**Version 0.1. Still in development. Use at your own risk.**

A simple one-shot sampler to trigger samples from a MIDI keyboard. It is a Puredata (Pd) patch so it will run on Mac, Windows and Linux.

One-shot runs on a Raspberry Pi. More info coming soon.

### Requirements

- [Pd 0.48-0](http://msp.ucsd.edu/software.html) (untested on earlier versions, but may still work)
- MIDI keyboard
- Computer with stereo audio output

### Instructions

After downloading and unzipping:

1. Place samples in "samples" folder.
2. Map MIDI notes to specific samples in the "keymap.txt" file.
3. Adjust options in "settings.txt" file.
4. Load Pd and setup audio and MIDI (Media menu -> "Audio Settings...", Media menu -> "MIDI settings...") and confirm audio and MIDI works (Media menu -> "Test Audio and MIDI...").
5. Load patch.

If you update "keymap.txt" or "settings.txt", reload the patch for the changes to take effect.

### keymap.txt

keymap.txt defines the mapping between MIDI notes and samples.

Each line has two elements separated by a space: MIDI note number and sample file name. The sample file name must be contained within quote marks if there is a space in it (e.g. "file name.wav"). The sample file will be played when the corresponding note number is received.

### settings.txt

Each line contains a setting name and a setting value separated by a space.

Example settings.txt file contents:
```
all_off_key 59
fade_dur 250
midi_channel 0
velocity_sensitive 0
debounce 0
same_key_twice 1
voice_stealing 0
```

| Setting | Value | Description |
| - | - | - |
| `all_off_key` | Note number | All notes are faded out in `fade_dur` duration (see below) when this note is received. |
| `fade_dur` | Duration in milliseconds | Fade out time after all_off_key has been received. |
| `midi_channel` | MIDI channel number 0 - 16 | Channel 1 to 16 or 0 = all channels received. |
| `velocity_sensitive` | 1 = yes, 0 = no | Enable/disable velocity sensitivity. |
| `debounce` | Debounce duration in milliseconds | Avoid double-triggers within debounce duration, 0 = debounce off. |
| `same_key_twice` | 1 = yes, 0 = no | Allow/disallow the same key to trigger its sample twice in succession. |
| `voice_stealing` | 1 = on, 0 = off | If all voices are busy, allow/disallow new MIDI notes to steal voices.

### Notes

- Polyphony is fixed at 24 voices.
- Up to 88 stereo samples may be used.
- Incoming notes are ignored for the "fade_out" duration after an "all notes off" has been triggered.
- Outputs to audio outputs 1 and 2.
- Mono files are duplicated to outputs 1 and 2.

### To do

- Load buffers sequentially and display error message if a problem is encountered.
- Turn on DSP when buffers have loaded successfully.
- Error messages if keymap.txt or settings.txt have not loaded successfuly.
- Make visually presentable.
