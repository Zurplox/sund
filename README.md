# ☀️ SunD — Live UV & Vitamin D Estimator

A single-file web app that estimates UV index **from your phone's actual sensors** — not internet weather data — and tracks estimated vitamin D production during a sun session.

## How it works

- **Android (Chrome):** reads the real ambient light sensor (lux) and compares it against the theoretical clear-sky brightness for the sun's exact position (computed locally from time + location — works offline after page load).
- **iPhone (Safari):** iOS doesn't expose the light sensor to websites, so the camera's view of the sky is used as a brightness proxy instead.
- Fitzpatrick skin type (I–VI) + skin-exposure slider drive the vitamin D model: **1,000 IU ≈ ¼ MED over 25% of skin**. The burn bar shows progress toward one full minimal erythemal dose.
- Built-in **Diagnostics** panel shows exactly which sensors your browser allowed — check it if nothing happens.

## Deploy to GitHub Pages

1. Create a new repository (e.g. `sund`).
2. Upload `index.html` (and this README) to the repo root.
3. Go to **Settings → Pages → Build and deployment → Deploy from a branch**, pick `main` / `(root)`, save.
4. Open `https://<your-username>.github.io/<repo-name>/` on your phone.

> ⚠️ The app **must** be served over HTTPS (GitHub Pages does this automatically). Opening `index.html` directly from a file will not work — browsers block camera/sensor/location access on non-HTTPS pages.

## Usage

1. Open the site on your phone, tap **Enable sensors & measure**, accept the prompts.
2. Go outdoors or to an **open** window (glass blocks the UV-B that makes vitamin D!) and point the sensor/camera at the sky.
3. Pick your skin type, set how much skin is exposed, and tap **Start session**.
4. Stop well before the burn bar fills.

## Limitations

- Expect ±2–3 UV index of error — this is a guide, not a measurement or medical advice.
- Indoors behind glass, real UV is ~zero even when bright; the app cannot detect glass.
- Demo mode (`?demo=1` in the URL) simulates a clear sky for UI testing; sensors are ignored in demo mode.

100% client-side. No data leaves the phone.
