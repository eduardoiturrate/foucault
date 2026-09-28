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

You can pause at any moment, rotate the view, and change the latitude.

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

No build step. Open `index.html` through any web server.

## Credits

- Coastlines and borders: [Natural Earth](https://www.naturalearthdata.com/) 1:50m
  (public domain), through the [world-atlas](https://github.com/topojson/world-atlas) package.
- 3D rendering: [three.js](https://threejs.org/) r128 (MIT licence), loaded from cdnjs.
- Fonts: Atkinson Hyperlegible, Fraunces and JetBrains Mono (SIL Open Font License),
  loaded from Google Fonts.
