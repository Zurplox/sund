# ☀️ SunD — Live UV & Vitamin D Estimator

A single-file web app that estimates UV index **from your phone's actual sensors** — not internet weather data — and tracks estimated vitamin D production during a sun session.

## How it works (v9)

**Cross-device standardization.** Every sensor path is converted to the same unit: **% of clear-sky brightness for the sun's exact current position** (computed locally from time + location). Per-device differences are handled by:

- **Exposure lock** (Chrome/Android where supported): manual exposure + lowest ISO, so pixel brightness linearly tracks real brightness (gamma-decoded).
- **Incident-meter equation** `E = C·N²/(t·ISO)` when ISO/shutter are readable: an actual lux estimate, with the **aperture N read from a photo's EXIF** (best-effort, Android) instead of a guess.
- **Brightest-quartile sky sampling**: the mean of the brightest 25% of pixels, so a hand or frame edge doesn't skew the reading.
- **EMA smoothing** (α = 0.35): the UV number doesn't jump sample-to-sample.
- **Platform-split corrections:** Android locks exposure (or meters ISO×shutter); iOS expands its compressed preview band for the live view and uses photo EXIF metering as the true reading.
- **📸 Photo meter (the accurate iOS path):** snap a photo of the sky — SunD reads ISO, shutter speed and aperture straight from the photo's EXIF and applies the incident-meter equation `E = C·N²/(t·ISO)` with **platform-tuned constants** (Android C≈1200, iOS C≈200 — phone AE exposure targets differ ~5× in bright sun, so one textbook constant fits neither), making iPhone/iPad and Android readings directly comparable. iOS live video stays available as a rough guide only (Apple HDR tone-maps the preview).

**Sensor priority:** real ambient light sensor (lux, Chrome/Android) → camera with locked exposure → camera with ISO/shutter metering → 📸 photo EXIF meter (best path on iOS) → camera AE luma (iOS rough guide).

**Session features:** Fitzpatrick skin-type selector with real skin-tone buttons (I–VI), skin-exposure slider, live vitamin D accumulation (Holick's rule: ¼ MED over ¼ body ≈ 1,000 IU), burn-time progress bar, **screen wake lock**, and automatic camera restart + exposure re-lock after screen unlock. Built-in **Diagnostics** panel logs every sensor event and error.

## Deploy to GitHub Pages

1. Create a new repository (e.g. `sund`).
2. Upload `index.html` (and this README) to the repo root.
3. Go to **Settings → Pages → Build and deployment → Deploy from a branch**, pick `main` / `(root)`, save.
4. Open `https://<your-username>.github.io/<repo-name>/` on your phone.

> ⚠️ The app **must** be served over HTTPS (GitHub Pages does this automatically). Opening `index.html` directly from a file will not work — browsers block camera/sensor/location access on non-HTTPS pages.

## Usage

1. Open the site on your phone, tap **Enable sensors & measure**, accept the prompts.
2. **iPhone/iPad:** tap **📸 Photo meter** and snap the sky for an exact reading — the live camera turns off and the session timer keeps running on the photo's measured value until you retake it (static mode).
3. Go outdoors or to an **open** window (glass blocks the UV-B that makes vitamin D!) and point the camera/sensor at the sky.
4. Pick your skin type, set how much skin is exposed, and tap **Start session**.
5. Stop well before the burn bar fills.

## References

- UV index → erythemal irradiance: UVI × 25 mW/m² (WHO/WMO; ISO/CIE 17166)
- MED by Fitzpatrick type: ~200/250/300/450/600/1000 J/m² (I–VI, typical solar-simulator values)
- Vitamin D: Holick's rule — ¼ MED over ¼ body ≈ 1,000 IU (PubMed 20398766); 1 MED full body ≈ 10,000–25,000 IU (Holick 2011; Med J Aust 2011)
- Incident-meter equation: E = C·N²/(t·ISO), C ≈ 250–340 (standard photographic metering)

## Limitations

- Expect ±2–3 UV index of error — a guide, not a measurement or medical advice.
- Indoors behind glass, real UV is ~zero even when bright; no brightness method can detect glass.
- Light sensor & exposure lock require **Chrome on Android**. On iPhone/iPad, use the **📸 Photo meter** for real readings; live video is a rough guide there.
- Demo mode (`?demo=1` in the URL) simulates a clear sky for UI testing; sensors are ignored in demo mode.

100% client-side. No data leaves the phone.
