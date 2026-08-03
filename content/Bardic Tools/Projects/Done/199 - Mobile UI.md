---
publish: true
created: 2025-07-09T12:03:29.236+01:00
modified: 2026-07-29T09:51:37.000+01:00
published: 2026-07-29T09:51:37.000+01:00
---

Goals:

- To give a great experience for mobile users that want to use Bardic Tools to play stuff, after they've prepped on their laptops

Ideas:

- You only have one scene active at a time.
  - You can change scene by navigating a menu
- Buttons are bigger.
- Things are not sortable.

### Plan

- Add setting to global settings
- Start with a rough layout sketch
- \[ ]
- Consider removing auto-toggling between screen layouts based on window size
- Make sure permalinks work, as selecting just one scene might get _tricky_.

### 2026-05-18

Alright, now to get a list of missing stuff for this feature:

### Burning down items

- Ambience Card error when loading settings when loading in mobile shape. ✅ 2026-05-18
- This is triggered because we get a variable that has the right shape onMount, but `setVolume` is `undefined`.
- I'm guessing the order of operations here is that svelte doesn't bind the variable until after it's mounted?
- Wait, there is no ambience card to mount. Because the board is fucking empty, right?
- Why do we even try to mount ambience cards?
- And why doesn't it fail for music Cards?
- Wait, that failure doesn't matter, because the card is just being unloaded anyway, so we can safely ingore it. Fuck off.

## Main List of TODOs

- Solve bugs
  - Ambience Card error when loading settings when loading in mobile shape. ✅ 2026-05-18
  - goToPrepMode doesn't work at all, it's like it's not triggering.
- Presets should be present in the control band.
- A button to enter play mode from the UI, beside the library big button.
  - Add a button to go back to prep mode from the main controls of the play mode.
- Mobile layout for play mode, now that I have clearly defined control elements and the logic is all there.
- Make sure card overflows are handled nicely.
- Accordion for music playlists? Like this:

```
Playlist A  (closed)
---
Playlist B (Open)
- Track B.A
- Track B.B
- Track B.C
---
Playlist C (closed) // All playlists in view at all times.
```

- The scroll is completely broken, for isntance on music.
- Does global volume work?
- Investigate extending the artwork to the footer or something like that ✅ 2026-06-02
  - Not worth it, makes the interface so noisy
- Add a shadow when there are more than 4 presets, to make sure they know they can scroll ✅ 2026-06-02
- "return to prep mode" button
- Make sure the back panel never shows, it can be jarring. ✅ 2026-06-02
- Enboss Bardic Tools Logo on the back of hte cards area.
- Back to prep mode button on empty state ✅ 2026-06-03
- make scene loader big. ✅ 2026-06-03
- Preset buttons:  ✅ 2026-06-03
  - Make preset buttons have a lighter BG shade for JUICE ✅ 2026-06-03
- PWA ✅ 2026-06-03
  - manifest with icon and name ✅ 2026-06-03
- Scene picker breaks with 3 scenes
- Hide the other elements, sometimes it flashes.
- Onboarding ✅ 2026-06-04
- Empty music displays a bad empty state, I should have proper empty states for all lanes. ✅ 2026-06-04
- Default to play mode on mobile
- Play buttons should be on the bottom, visible on mobile prep mode ✅ 2026-06-04
- Preset buttons:
  - Give feedback for loading a preset.
- This just crashes sometimes on the actual iphone ✅ 2026-06-04
  - But just on _my_ iPhone ✅ 2026-06-04
  - It was fucking CSS
- Address tutorial feedback from Nay ✅ 2026-06-04
- Empty state for scene picker ✅ 2026-06-04
- PWA
  - Install prompt
- Widen the definition of mobile to "anything portrait-shaped that has fewer than an iPad Mini's worth of pixels wide"

Probably out of scope

- PWA.
  - A tracker for "offline play".
  - Caching sounds by the service worker.
- Make sure that a screen resize (e.g. tilting the phone) doesn't leave orphaned, playing cards.
  - Add message to note that screen rotations are not supported, using the ScreenRotation API
  - Out of scope because it becomes immediately a non-problem once I switch to a central controller.
- Have starter kits also use the mobile UI.

#### Mobile Layout for play mode.

First, let's hide the play mode controls.
Then, let's start placing things on the screen.
