# mull
Mozaic Updater for Linnstrument Lights

## What is it?
A tool to allow MIDI files to control Linnstrument lights

### What does it do?
mull updates the Linnstrument lights to show both notes in the current scale & chord, as well as in the immediate next scale & chord. The intention is to help in improvising on the Linnstrument by using color codes to show which notes are "playable" at any point, with the colors showing the role each note plays in the tune at that point.

To use it, create a MIDI file/stream containing the chords for the entire tune, and send it to the Linnstrument 1(?) beat *ahead* of the music being played. 

### How does it work
mull works like this:

Initial setup
- create a list of where every C, C#/Db, D, D#/Eb, E, F, F#/Gb, G ... note is on the Linnstrument
- create a list of color codes that can be sent to the Linnstrument
- maintain a map of which notes are currently set to which color

When a user initially selects a root note & scale:
- mull sets every root note to a certain color (e.g. blue)
- mull sets every note in the scale to a certain color (e.g. white)

When a new chord is sent to mull via MIDI, it changes the Linnstrument lights as follows:

| NOTES IN SCALE            | Note in next chord | Note not in next chord   |
|---------------------------|--------------------|--------------------------|
| Note in current chord     | PURPLE             | BLUE                     |
| Note not in current chord | YELLOW             | BLUE if root, else WHITE |

| NOTES NOT IN SCALE        | Note in next chord | Note not in next chord |
|---------------------------|--------------------|------------------------|
| Note in current chord     | ORANGE             | BLACK                  |
| Note not in current chord | ORANGE             | BLACK                  |

### To update colors on the Linnstrument's pads

Setting the color of *each pad* requires 3 distinct MIDI CC messages to be sent to the Linnstrument
- set MIDI CC 20 to the column (0-15 or 0-24, depending on the model of Linnstrument)
- set MIDI CC 21 to the row (0-7)
- set MIDI CC 22 to the color
  - 0 to revert to the color configured in Linnstrument settings
  - 1 to display RED
  - 2 to display YELLOW
  - 3 to display GREEN
  - 4 to display CYAN
  - 5 to display BLUE
  - 6 to display MAGENTA
  - 7 to display BLACK (off)
  - 8 to display WHITE
  - 9 to display ORANGE
  - 10 to display LIME
  - 11 to display PINK

Note that the values of MIDI CC 20 & 21 are both persistent, so it's possible to e.g. work along a row of lights by setting CC 21 once, then setting CC 20 & 22 for each light in the row.
