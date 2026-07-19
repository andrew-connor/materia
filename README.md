# Materia

**An interactive 3D viewer that shows everyday and iconic objects side by side with the raw materials they're made of.**

Materia puts two synchronized 3D views next to each other:

- **Form** — the object as it exists in the world, with each part colored by the material it's actually made of.
- **Composition** — that same object broken down into its constituent materials, rendered as a stack of cubes. Each cube is sized by the cube root of that material's volume and the cubes are stacked largest-at-the-bottom, so the stack reads as a true-to-scale "bill of materials."

Both views share the same world scale (meters), so you can directly compare how much actual *stuff* is inside something against how big it appears. A skyscraper turns out to be mostly air; a rocket is almost entirely fuel.

> Replace this line with a screenshot once the repo is live:
> <img width="1909" height="832" alt="materia_bike_screenshot" src="https://github.com/user-attachments/assets/e196c3f4-7cda-4041-a76b-c9782e3790f1" />

---

## Highlights

- **26 objects** spanning four categories — vehicles, buildings & monuments, living things, and household items.
- **Form vs. Composition** views, camera-locked together so they rotate and zoom as one (toggle the lock to frame each independently).
- **Enclosed Volume** readout on the Form view — the true solid volume of the object's geometry, computed from the mesh itself via the divergence theorem.
- **Total Material Volume** summary on the Composition view, plus a per-material breakdown with volumes and percentages.
- **Real densities.** Every material carries a real-world density, so the volumes correspond to physically meaningful masses.
- **Toggles** for camera lock, units (m³ / ft³), and on-screen dimension lines.
- **Self-contained.** One HTML file, no build step, no dependencies to install. Three.js loads from a CDN at runtime.

---

## The object library

**Vehicles & Craft** — Apollo / Saturn V Rocket · Bicycle · Boeing 747-400 · Cargo Freighter · Diesel-Electric Locomotive · Hot Air Balloon · Mid-size Sedan

**Buildings & Monuments** — Burj Khalifa · Eiffel Tower · Empire State Building · Gateway Arch · Notre-Dame Cathedral · Statue of Liberty · Washington Monument

**Living Things** — African Elephant · Blue Whale · Coast Redwood · Dog · Human

**Household** — Cardboard Box · Laptop Computer · Milk Jug · Refrigerator · Shoe · Soccer Ball · Soda Can

---

## Running it

### Locally

The simplest path is to open `index.html` directly in a browser. Three.js is loaded from a CDN, so you'll need an internet connection.

If your browser blocks ES-module scripts over `file://` (some configurations do), serve the folder over a local web server instead:

```bash
# Python 3
python3 -m http.server 8000
```

Then visit `http://localhost:8000`.

### On the web (GitHub Pages)

This repo is structured to deploy as-is on GitHub Pages. The entry point is `index.html` at the repository root, so once Pages is enabled the site is available at:

```
https://<your-username>.github.io/<repo-name>/
```

Follow GitHub's standard *Settings → Pages → Deploy from a branch (main / root)* flow.

---

## How it works

Materia is a single HTML file with vanilla JavaScript and one runtime dependency:

- **Rendering** — [Three.js](https://threejs.org/) `0.160.0`, loaded via an ES-module import map from a CDN. `OrbitControls` is pulled from the matching addons path. There is no bundler and no build step.
- **Dual viewport** — a single WebGL canvas is split with the scissor test into two viewports, with two transparent overlay `<div>`s capturing pointer events so each side can drive its own `OrbitControls` instance without fighting over the shared canvas.
- **Geometry** — every object is built procedurally from Three.js primitives and custom `BufferGeometry` meshes (for example, the shoe and milk jug are swept surfaces stitched from cross-sectional outlines; the soccer ball is a per-triangle-colored truncated icosahedron).
- **Enclosed Volume** — computed by summing the signed tetrahedron volumes of every triangle in the object's mesh (the divergence theorem). This measures the volume of solid material, not the bounding box.
- **Composition cubes** — each material's volume is turned into a cube of side length ∛volume, so a cube's *edge* scales linearly while its *volume* matches the material. Cubes are stacked largest-first.
- **Mass** — each material in the palette has a real density (kg/m³), so material volume × density gives a physically meaningful mass. The object factoids are tuned so the modeled material volumes land within roughly ±10% of each object's real-world mass.

All numbers are deliberate approximations chosen for intuition and comparison, not engineering precision.

---

## Tech

- Three.js `0.160.0` (CDN, import map — no install)
- Vanilla JavaScript, no framework
- No build tooling; the source *is* the deliverable

---

## License

Released under the MIT License. See [LICENSE](LICENSE).

---

## Notes & disclaimers

Material breakdowns and dimensions are researched approximations intended to convey scale and proportion. They are not authoritative engineering data. Where a real object's mass is dominated by one material (a rocket by propellant, a building by concrete), the model reflects that, sometimes at the expense of finer detail.
