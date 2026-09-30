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
- `tools/render-video.mjs`: renders the film as videos with sound, vertical or horizontal.
- `tools/make-music.py`: makes the background music of the videos.
- `geo-data-hd.js`: more detailed coastlines and borders, used only for the videos.

No build step. Open `index.html` through any web server.

## Updating the narration

After a caption changes:

1. Open the page, and in the browser console run
   `copy(JSON.stringify(foucault.captions(), null, 1))`.
   Paste the result into `narration/captions.json`.
2. Put your ElevenLabs API key in the `ELEVENLABS_API_KEY` environment variable or in
   the file `~/.elevenlabs_key` (never in the repository).
3. Run `python tools/make-narration.py --dry-run` to see how many characters will be sent,
   then `python tools/make-narration.py`. Only new or changed captions are sent (and the
   lines in `publish/video-lines.json`). Each new clip is cut 0.25 s after its last word,
   to remove clicks at the end of the ElevenLabs files. Clips that no caption uses are
   deleted. The voice is Brian.

## Videos

`tools/render-video.mjs` renders the film as videos (H.264 and AAC in MP4), with the recorded
narration, music, and an `.srt` subtitle file for each video. The files go into `video/`, which
git ignores.

- `node tools/render-video.mjs --captions`: 7 vertical Shorts, 1080 × 1920 at 30 frames per
  second, 0:44 to 1:44 each. Each one opens with a spoken hook and big text over the best
  shot of its chapter, shows "Foucault pendulum – N of 7", a progress bar and the captions one
  sentence at a time, and ends with a card that points to the next part. About 45 minutes.
- `node tools/render-video.mjs --wide`: the horizontal film, 1920 × 1080 at 60 frames per
  second, about 8 minutes: a cold open of three shots, a title card, the 7 chapters with the
  numbers panel and a chapter name at the start of each, the "Try it" app in use, and an end
  card. About 1.5 hours.

How the videos differ from the page:

- Each step lasts as long as its voice plus 0.3 s, but at least 55 % of its reading time on the
  page. The voice plays 7 % faster, with the same pitch.
- The camera drifts slowly all the time, and chapters dissolve into each other.
- The music is made by code (`tools/make-music.py`), so there is no licence to check. It drops
  under the voice and comes back in the pauses. The sound is set to -14 LUFS.
- The map uses Natural Earth 1:10m (`geo-data-hd.js`, 2.5 MB), drawn at 8192 × 4096. The page
  itself keeps the lighter 1:50m data.
- The hooks, cold open, outro and card texts are in `publish/video-lines.json`.

The script opens `index.html?video` (or `?video=wide`) in Chrome without a window, draws each
frame at its exact time, and sends the pictures to ffmpeg. It needs Node 22 or later, Google
Chrome, ffmpeg, and Python with numpy and scipy. Useful options: `--plan` prints the timing and
the film's chapter list, `--stills 2:3:1` saves a picture of chapter 2, step 3, at 1 s,
`--part 3` renders one Short, and `--from`/`--to` render a test range in seconds.

`tools/upload-youtube.py` uploads the Shorts to YouTube, with the texts in
`publish/youtube-shorts.json`; run it without `--yes` first to see what it would send. With
`--meta publish/youtube-film.json --state video/youtube-film-state.json` it uploads the film
instead. The texts for TikTok are in `publish/tiktok.md`.

## Credits

- Coastlines and borders: [Natural Earth](https://www.naturalearthdata.com/) 1:50m
  (1:10m in the videos)
  (public domain), through the [world-atlas](https://github.com/topojson/world-atlas) package.
- Narration: voice made with [ElevenLabs](https://elevenlabs.io/) text to speech.
- Music in the videos: made with code for this film.
- 3D rendering: [three.js](https://threejs.org/) r128 (MIT licence), loaded from cdnjs.
- Fonts: Atkinson Hyperlegible, Fraunces and JetBrains Mono (SIL Open Font License),
  loaded from Google Fonts.
