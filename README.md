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

The stars are fixed in space. In a view from space they do not move. In a view from the
floor, the camera turns with the Earth, so the sky turns once per sidereal day, as it does
for a person standing on the Earth.

The drawing is not to scale. The pendulum is drawn far larger than real, and the
Earth's turn is sped up compared with the swing.

## Files

- `index.html`: the whole page (HTML, CSS and JavaScript).
- `geo-data.js`: coastlines and country borders, encoded as 16-bit integers.
- `narration/`: the recorded voice. One MP3 per caption, named by a hash of the spoken
  text, and `index.json`, the list of clips. A caption with no clip is read by the
  browser's own speech engine.

No build step. Open `index.html` through any web server.

## Credits

- Coastlines and borders: [Natural Earth](https://www.naturalearthdata.com/) 1:50m
  (public domain), through the [world-atlas](https://github.com/topojson/world-atlas) package.
- Narration: voice made with [ElevenLabs](https://elevenlabs.io/) text to speech.
- 3D rendering: [three.js](https://threejs.org/) r128 (MIT licence), loaded from cdnjs.
- Fonts: Atkinson Hyperlegible, Fraunces and JetBrains Mono (SIL Open Font License),
  loaded from Google Fonts.
