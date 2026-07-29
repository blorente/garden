---
publish: true
created: 2026-07-16T10:31:34.043+01:00
modified: 2026-07-16T10:31:17.523+01:00
---

Links: [[WebDev]], [[WebAudio]], [[WebCodec]]
Date: 2025-04-30
Visibility (remove one):

- [[Public]]

---

# WebCodecs API Primer

So, it turns out that the fine folks at the WebAudio working group have decided to give users direct access to the WebCodecs to encode/decode files.

Now, support is a bit iffy (https://caniuse.com/?search=webcodec), but it should be fine in everything except Safari.

So maybe we don't use it yet.

So, the process we have to do is:

- Create a decoder, telling it where to put the output (each decoded frame)
- Configure the decoder, telling it which codec to use.
- Then, call a function that will stream the encoded chunks.
  - This means that we might have to extract the encoded chunks from the container (like extracting the audio MP3 portion from an MP4 file).
  - This is done with a demuxer.
  - A demuxer can also be used to parse mp3 files and extract the headers, for instance, so that we have fully-formed chunks to decode by stream.

Available codecs are:

- Registry: https://www.w3.org/TR/webcodecs-codec-registry/
- Supports mp3, flac, opus, vorbis, and mp4a (AAC), so it should be fine.
- Note that WAV doesn't have to be encoded.

### Sources

- https://developer.mozilla.org/en-US/docs/Web/API/WebCodecs\_API
