# One-shot README

### What is One-shot?

A free, minimalist one-shot sample player triggered by MIDI notes. It is a [Puredata (Pd)](http://www.puredata.info) patch so it will run on Mac, Windows and Linux. Useful for triggering a folder of sound effects from a MIDI keyboard, amongst other things. Includes safety features such as adjustable debounce and "all notes off".

> The sampling term "one-shot" is applied to a sample that once triggered by a note-on MIDI message will play through to its end, ignoring the note-off MIDI message that normally follows the note-on message. This is useful for sound effects and percussive samples that have a set duration and play back at the original sample pitch.

Samples are stored in a folder named `samples`. They are mapped in a text file called `keymap.txt` and settings are stored in `settings.txt`. Since One-shot is configured using only text files, the GUI is not required. In fact, this makes it rather well suited to running on a single-board screen-less computer such as a Raspberry Pi. More info on this coming soon.

One-key preloads samples into RAM on startup for responsive triggering.

### Requirements

- [Pd 0.48-0](http://msp.ucsd.edu/software.html) (untested on earlier versions, but may still work)
- MIDI keyboard
- Computer with a stereo audio output (even works on a Raspberry Pi)

### Instructions

First [download](http://msp.ucsd.edu/software.html) and install Pd if you don't already have it. Then download and unzip the One-shot files from the [One-shot GitHub page](https://github.com/making-sound/One-shot).

1. Place samples in "samples" folder (no spaces in sample file names).
2. Map MIDI notes to specific samples in the "keymap.txt" file.
3. Adjust options in "settings.txt" file.
4. Load Pd and setup audio and MIDI ("Media" menu -> "Audio Settings...", "Media" menu -> "MIDI Settings..."), and confirm audio and MIDI works ("Media" menu -> "Test Audio and MIDI...").
5. Open patch (file called `One-shot x.x.pd`) in Pd (File menu -> "Open").
6. The "Ready" toggle box will become ticked when everything has loaded okay. If it doesn't (it may take a while if you have several long samples), check the Pd window for error messages ("Window" menu -> "Pd").

If you update "keymap.txt" or "settings.txt", or change any samples whilst One-shot is running, click "Reload" or restart the patch.

### keymap.txt

keymap.txt defines the mapping between MIDI notes and samples.

Each line has two elements separated by a space: MIDI note number and sample file name. The sample file name must not include any spaces. The sample file will be played when the corresponding note number is received.

Example `keymap.txt` contents:
```
60 pings1.aiff
62 pings2.aiff
64 pings3.aiff
65 pings4.aiff
67 stereo_a.wav
```

### settings.txt

Each line contains a setting name and a setting value separated by a space.

Example `settings.txt` file contents:
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

- Long samples may take time to load after the patch is run. Wait for the "Ready" toggle to become ticked.
- Polyphony is fixed at 24 voices.
- Up to 88 stereo samples may be used.
- Incoming notes are ignored for the "fade_out" duration after an "all notes off" has been triggered.
- Outputs to audio outputs 1 and 2.
- Mono files are duplicated to outputs 1 and 2.

### To Do

- Load buffers sequentially and display error message if a problem is encountered.
- Error messages if keymap.txt or settings.txt have not loaded successfuly.
- Use readsf~ instead of tabread4~ to avoid large array indexing artifacts?
- Load duplicate sound files only once.
- Print progress messages to the Pd window to inform user of patch status when running it remotely from the command line.
- Add shell script to start the patch with correct audio/MIDI settings on a Raspberry Pi.

### Wish List

- Allow computer QWERTY keyboard to trigger samples. Will require a new setting in settings.txt to select either MIDI or QWERTY input.
- Adjustable polyphony
- Master volume setting?
- Program change folders (a keymap.txt and sample folder for each program change message received)?
