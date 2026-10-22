# AR Solar System — Marker-based WebAR

An augmented reality app that places an animated, interactive solar system on top of a printed marker. It runs in a phone browser (no app install) using **A-Frame** (3D/WebXR framework) and **AR.js** (marker tracking). The assignment allows "tools of your choice"; AR.js is open-source and in the same family as ARToolKit (link 8 in the brief).

## Features
- Detects the **Hiro marker** through the phone camera and anchors 3D content to it
- Sun + 6 planets orbiting at different speeds, with orbit rings, labels and Saturn's rings
- **Tap any planet** to see a fact card
- On-screen status: "marker found / lost"

## 1. Get the marker
Open and print (or display on a second screen):
https://raw.githubusercontent.com/AR-js-org/AR.js/master/data/images/hiro.png
Keep the black border visible; matte paper works best (less glare).

## 2. Host it (camera needs HTTPS)
Browsers only allow camera access on HTTPS (or `localhost`).

**Option A — GitHub Pages (recommended for submission)**
1. Create a public repo, e.g. `ar-solar-system`, and upload `index.html`.
2. Settings → Pages → Source: `main` branch, `/ (root)` → Save.
3. After a minute, open `https://<your-username>.github.io/ar-solar-system/` on your phone.

**Option B — Netlify Drop**
Drag the project folder onto https://app.netlify.com/drop and you get an HTTPS link instantly.

**Option C — Test on laptop webcam**
```bash
npx http-server .     # then open http://localhost:8080
```
Hold the printed marker in front of the webcam.

Tip: put the hosted URL in a QR code on your demo poster so the examiner can scan it.

## 3. How it works (for the report/viva)
1. **Camera feed**: AR.js grabs the webcam stream and renders it as the background.
2. **Marker detection**: each frame is converted to grayscale and thresholded; AR.js finds square black borders and matches the inner pattern against the Hiro template.
3. **Pose estimation**: from the 4 corners of the detected square, it computes the marker's position and orientation (a 4×4 transformation matrix) relative to the camera, using camera calibration parameters.
4. **Rendering**: A-Frame (built on Three.js / WebGL) applies that matrix to the `<a-marker>` entity, so all 3D children appear fixed to the paper.
5. **Interaction**: a raycaster from the tap position checks for intersections with `.clickable` objects.

**AR type:** marker-based (vs. markerless/SLAM, location-based, projection-based).

## 4. Suggested report outline
1. Introduction to AR and types of AR
2. Problem statement & objectives (interactive astronomy learning aid)
3. Tools & technologies: HTML, JavaScript, A-Frame, AR.js, WebGL, GitHub Pages
4. System architecture: Camera → Marker detection → Pose estimation → 3D rendering → User interaction
5. Implementation (key code snippets: `<a-marker>`, planet generation loop, click handler)
6. Screenshots (marker not found, solar system on marker, fact card)
7. Results & limitations (needs good lighting, marker must stay in view)
8. Future scope: custom image targets (MindAR), markerless placement (WebXR), real 3D models (.glb), audio narration
9. References

## 5. Team split (2–4 members)
- **Member 1**: 3D scene and animations
- **Member 2**: interaction (tap facts, UI hints)
- **Member 3**: hosting, testing on multiple phones, QR/demo setup
- **Member 4**: report, architecture diagram, presentation

## 6. Likely viva questions
- Difference between AR, VR and MR?
- Why is HTTPS required? (browser security for camera access)
- How does the app know where to place the objects? (marker detection + pose estimation)
- Marker-based vs markerless AR — pros and cons?
- What is A-Frame's entity–component system?

## Customising
- Change the marker: use `preset="kanji"`, or make your own with the AR.js Marker Training tool and use `type="pattern" url="pattern-yourmarker.patt"`.
- Swap the topic: replace the planets with any `.glb` model via `<a-gltf-model src="model.glb">` (free models at Sketchfab/Poly Pizza) — e.g. a CPU, a DNA strand, a campus building.
- Speeds/sizes: edit the `PLANETS` array in `index.html`.

## Troubleshooting
| Problem | Fix |
|---|---|
| Black screen / no camera prompt | You're on `http://` — use the HTTPS link |
| Marker not detected | More light, avoid glare, keep whole marker + border in frame |
| Objects jitter | Hold steady; print the marker larger (≥ 10 cm) |
| Works on Android not iPhone | Use Safari on iOS and allow camera access |
