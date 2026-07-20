---
publish: true
created: 2026-07-16T10:32:05.266+01:00
modified: 2026-07-16T10:32:10.945+01:00
---

Links: [[WebDev]], [[Apple]]
Date: 2025-04-29
Visibility (remove one):

- [[Public]]

---

# HLS - HTTP Live Streaming

- Filename extension is .m3u8

- The idea is that we stream the same segment in different qualities, so that we can swap seamlessly between them.
  - Segments are of equal length, in `.ts` files (ts stands for MPEG-2 Transport Stream)
  - An index of segments is then stored in the .m3u8 format.

- The client is made aware of different available streams, for different qualities.

- You can fucking insert ads dynamically, awesome sauce.

- Support in browsers is dodgy, because we need Media Source Extensions for Firefox and Chromium: [[Media Source Extensions]]

### Sources

- Wikipedia: https://en.wikipedia.org/wiki/HTTP\_Live\_Streaming
