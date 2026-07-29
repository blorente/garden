---
publish: true
created: 2026-07-16T10:30:53.922+01:00
modified: 2026-07-16T10:31:08.993+01:00
---

Links: [[WebDev]], [[WebAudio]]
Date: 2025-04-30
Visibility (remove one):

- [[Public]]

---

# WebAudio Primer

Concepts:

- A **context**: I think just a container for nodes
- An **Audio Routing Graph**: A graph of audio **nodes**
- A **Node**: A series of sources that collate into one or more outputs, essentially `f(source1, source2, ...) -> (output1, out2, ...)`
  - There are prebuilt nodes with effects, such as `GainNode`.
  - Actually, sources and destinations **are also nodes themselves**
- **Sources**: a stream of tiny, time-sliced audio data.
  - Could be sythetized (oscillator), or from a decoded buffer.
- **Outputs** Can point to other nodes, or to a **Destination**
  - A **Destination** usually means "the audio output of the user"
- **Channels:** Distinct rails for frequencies. Mono has one channel, stereo has two (left and right), and 5.1 has 5... + 1.
  - Each **source** and **output** can have several **channel**s.

> [!aside] Channel notation
> So, ever wondered what 5.1 and 2.0 mean in "Dolby 5.1"? The 5 is the number of full-frequency-range channels, and the 1 is the number of low-frequency-range channels, usually called subwoofers. Notations of different channels: https://developer.mozilla.org/en-US/docs/Web/API/Web\_Audio\_API/Basic\_concepts\_behind\_Web\_Audio\_API#audio\_channels

- A **Buffer** is a stream of samples.
  - It can have several **channels**, a defined **length**, and a **sample rate**
- A **Frame** is just a collection of all samples happening at a particular point in the buffer.
  - In mono buffers, one frame will contain one sample.
  - In stereo, one frame is two samples (left and right),
  - And so on

> [!note]  Common sample rates
> A common sample rate is 44.1 kHz (44.100 frames per second). There is complicated audio theory behind this, but for now just know that most sound cards will up-sample lower-frequency buffers to 44.1 kHz automatically, like an image that gets upsized.

- **Up/Down-mixing** happens when the channels in the input don't match those in the output.
  - Up-mixing happens when we add channels, down-mixing when we remove channels.

- Currently, the approach recommended by the tutorial to load a file, which is to use an audio element, has the issue that looping is difficult.
  - ❗️However, we could possible ameliorate this if we manipulate the playback.

- There is a [[WebCodec]] API in the works, that might help.

- a **CODEC** is the program that `enCODEs and DECodes` the raw data.

- A **Container** is a file format that contains one or more encoded tracks

- A codec supports several **Containers**. For instance, a FLAC-encoded audio file could ship in an MP4 file.
  - Opus seems to be the preferred codec

### Sources

- Intro: https://developer.mozilla.org/en-US/docs/Web/API/Web\_Audio\_API
- Audio Concepts: https://developer.mozilla.org/en-US/docs/Web/API/Web\_Audio\_API/Basic\_concepts\_behind\_Web\_Audio\_API
- Tutorial: https://developer.mozilla.org/en-US/docs/Web/API/Web\_Audio\_API/Using\_Web\_Audio\_API
