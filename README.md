# Foucault Between the Poles

A 3D film, in the browser, that explains why a Foucault pendulum turns
360° × sin(latitude) per day.

**Watch it:** https://eduardoiturrate.github.io/foucault/

At the North Pole the swing turns once per day. At the Equator it does not turn.
The film explains every latitude in between, in eight chapters:

1. The question: why does Paris take 31.8 hours?
2. At the North Pole
3. At the Equator
4. A common mistake: "the swing plane stays fixed relative to the stars" is true only at the pole
5. Split the Earth's spin into a vertical part (Ω sin φ) and a horizontal part (Ω cos φ)
6. The cone: a geometric proof with no forces
7. Seen from the floor: the Coriolis push, and the star and petal patterns in the sand
8. Try it yourself: any latitude, speed, view and way of starting the swing

You can pause at any moment, rotate the view, and change the latitude. The voice is on
at the start. The Text button hides the captions, to leave more room for the 3D scene.

On a phone, the page shows the film title until the film starts. It hides the numbers panel
and some controls, and fits the 3D scene in the space that is left.

## Physics

The pendulum motion is the exact solution of the linear Foucault equations in the
rotating frame of the floor (x = east, y = north):

    x'' = -ω0² x + 2 Ω sin φ · y'
    y'' = -ω0² y - 2 Ω sin φ · x'

The page computes each frame directly from time, so the scrub bar can jump to any
moment. The solution was checked against a numerical (RK4) integration.

The drawing is not to scale. The pendulum is drawn far larger than real, and the
Earth's turn is sped up compared with the swing.

## Files

- `index.html`: the whole page (HTML, CSS and JavaScript).
- `geo-data.js`: coastlines and country borders, encoded as 16-bit integers.
- `narration/`: the recorded voice. One MP3 per caption, named by a hash of the spoken
  text, and `index.json`, the list of clips. A caption with no clip is read by the
  browser's own speech engine.
- `tools/make-narration.py`: makes the clips with ElevenLabs text to speech.
- `tools/render-video.mjs`: renders the film as a video with sound, vertical or horizontal.

No build step. Open `index.html` through any web server.

## Updating the narration

After a caption changes:

1. Open the page, and in the browser console run
   `copy(JSON.stringify(foucault.captions(), null, 1))`.
   Paste the result into `narration/captions.json`.
2. Put your ElevenLabs API key in the `ELEVENLABS_API_KEY` environment variable or in
   the file `~/.elevenlabs_key` (never in the repository).
3. Run `python tools/make-narration.py --dry-run` to see how many characters will be sent,
   then `python tools/make-narration.py`. Only new or changed captions are sent, and
   clips that no caption uses are deleted. The voice is Brian.

## Video for social apps

`tools/render-video.mjs` renders chapters 1 to 7 as a video with the recorded narration
(H.264 and AAC in MP4), and an `.srt` subtitle file for each video. The files go into
`video/`, which git ignores.

- `node tools/render-video.mjs`: vertical, 1080 × 1920 at 30 frames per second, with the
  phone layout. Also one video per chapter (1 to 2.5 minutes each). About 1 hour.
- `node tools/render-video.mjs --wide`: horizontal, 1920 × 1080 at 60 frames per second,
  with the desktop layout and the numbers panel. About 2 hours.

The captions are not in the picture; use the `.srt` files. The script opens
`index.html?video` (or `?video=wide`) in Chrome without a window, draws each frame at its
exact time, and sends the pictures to ffmpeg. It needs Node 22 or later, Google Chrome, and
ffmpeg. For a short test, use `--from` and `--to` with times in seconds, for example `--to 20`.

## Credits

- Coastlines and borders: [Natural Earth](https://www.naturalearthdata.com/) 1:50m
  (public domain), through the [world-atlas](https://github.com/topojson/world-atlas) package.
- Narration: voice made with [ElevenLabs](https://elevenlabs.io/) text to speech.
- 3D rendering: [three.js](https://threejs.org/) r128 (MIT licence), loaded from cdnjs.
- Fonts: Atkinson Hyperlegible, Fraunces and JetBrains Mono (SIL Open Font License),
  loaded from Google Fonts.
