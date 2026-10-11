# IW4 Pocket 0.3.3

Built to get `mp_rust` running on the iPhone 13 Pro Max (A15) and to make problem reports easy. **Tested on an iPhone 13 Pro Max:** the graphics error is gone and the IW4 Pocket options work, but the app still runs out of memory while the map prepares its graphics (see Known).

**The graphics error that closed the app** (found in the 0.3.2 logs of an iPhone 13 Pro Max)
- Phones whose GPU cannot filter 32-bit float textures (A15 and older) made the renderer reject the texture table: `Texture binding 0 expects sample type Float { filterable: true }, but was given a view with format R32Float`. The app then quit with `code=1` while 66 of 1484 pipelines were ready.
- On those phones the sun shadow map, the spot shadow maps and the float depth now use a 1x1 placeholder in the table. Phones that can filter them (for example the iPhone 17 Pro Max) are unchanged.
- Dynamic shadows are held off on those phones, since their maps could not be read anyway.
- If another image causes a similar error, the log names it.

**IW4 Pocket options** (in the game: Options > IW4 Pocket options)
- Device profile, texture size, frames in flight, 120 Hz in matches, frame limit, compressed textures, dynamic shadows. Every row starts with **Auto**, has a one-line description, and applies after you restart the app.
- Choices are kept in `Documents/iw4l-env.txt`; lines you wrote there yourself are left alone.
- **Send logs**: asks for a second press, then opens a GitHub problem report with a short summary filled in (no personal data). You attach the two log files by hand.
- Shows how much memory iOS gives the app.

**Better logs**
- `gpu:` line (compressed textures, 32-bit float filtering), `shadow memory:` lines, `session summary:` (peak memory, least memory left, why the run ended), `previous run:` (tells a run closed by iOS from one that ended itself).
- Heap per asset kind now shows `total` and `own` (a weapon's own cost without its models and animations).
- The app folder path is written as `<app>`; the install id is no longer in the log. Repetitive lines (destructible deaths, static model meshes) are left out.

Known: on a 3 GB grant (iPhone 13 Pro Max) the app is closed by iOS during `mp_rust` while it prepares graphics pipelines (about 3.07 GB used of 3.07 GB, around 750 of 1484 pipelines done). This build does not lower memory; that is the next job. Phones with more memory are not affected.

Please send `iw4l-boot.log`, `iw4l-boot.prev.log` and `iw4l-memory-settings.txt` after trying `mp_rust`.
