# ☀️ SunD — Live UV & Vitamin D Estimator

A single-file web app that estimates UV index **from your phone's actual sensors** — not internet weather data — and tracks estimated vitamin D production during a sun session.

## How it works (v3)

- **Android (Chrome):** reads the real **ambient light sensor (lux)** when available. If not, it uses the camera with **manual (locked) exposure** where supported — auto-exposure is disabled so brightness readings actually track the scene. Where exposure lock isn't available, it meters the camera's own chosen ISO × shutter speed (∝ scene brightness), which still responds to real light.
- **iPhone (Safari):** iOS exposes neither the light sensor nor exposure controls to websites, so the **front camera's** sky brightness is used with fixed brightness bands. Cruder, but live.
- **Front camera by default** — hold the phone normally and the selfie cam faces the sky while you read the screen. A **Switch camera** button flips front/back.
- Fitzpatrick skin type (I–VI) + skin-exposure slider drive the vitamin D model: **1,000 IU ≈ ¼ MED over 25% of skin**. The burn bar shows progress toward one full minimal erythemal dose.
- Built-in **Diagnostics** panel logs exactly which sensors your browser allowed — open it if nothing happens.

## Deploy to GitHub Pages

1. Create a new repository (e.g. `sund`).
2. Upload `index.html` (and this README) to the repo root.
3. Go to **Settings → Pages → Build and deployment → Deploy from a branch**, pick `main` / `(root)`, save.
4. Open `https://<your-username>.github.io/<repo-name>/` on your phone.

> ⚠️ The app **must** be served over HTTPS (GitHub Pages does this automatically). Opening `index.html` directly from a file will not work — browsers block camera/sensor/location access on non-HTTPS pages.

## Usage

1. Open the site on your phone, tap **Enable sensors & measure**, accept the prompts.
2. Go outdoors or to an **open** window (glass blocks the UV-B that makes vitamin D!) and point the camera/sensor at the sky.
3. Pick your skin type, set how much skin is exposed, and tap **Start session**.
4. Stop well before the burn bar fills.

## Limitations

- Expect ±2–3 UV index of error — this is a guide, not a measurement or medical advice.
- Indoors behind glass, real UV is ~zero even when bright; no brightness method can detect glass.
- Light sensor & exposure lock require **Chrome on Android**. On iPhone, expect coarser brightness bands.
- Demo mode (`?demo=1` in the URL) simulates a clear sky for UI testing; sensors are ignored in demo mode.

100% client-side. No data leaves the phone.
