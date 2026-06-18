# Versuch 7 — Plan: Hybrid-Wow-Video (LTX-Video + 3D-Figur)

Gehört zu `specs/007`. Datum: 2026-06-17.

## Schritte

1. **Wan 2.2 5B lokal aufsetzen & testen**
   - Fehlende Wan-2.2-VAE (48-Kanal, /16) nachladen — das 5B braucht sie zwingend
     (nicht die 2.1-VAE der 14B-Modelle).
   - Nativer ComfyUI-Pfad: `UNETLoader` (5B) + `CLIPLoader` (umt5, type wan)
     + `Wan22ImageToVideoLatent` + `ModelSamplingSD3` + `KSampler` + `VAEDecode`.
   - Smoke-Render, Ergebnis prüfen.

2. **Falls Wan scheitert: systematisch eingrenzen**
   - VAE isoliert testen (Round-Trip echtes Bild), Modellgewichte scannen,
     Text-Encoder-Embeddings prüfen, Sigma-Kurve prüfen.
   - Variablen durchspielen: Auflösung, Sampler, Scheduler, Shift, Präzision
     (fp16/bf16/fp32), Attention-Backend (sub-quadratic/split/pytorch), t2v vs i2v.
   - ComfyUI-Update als möglichen Fix testen, danach zurückrollen.

3. **Fallback-Engine: LTX-Video 2B**
   - `workflows/06-text-zu-video-ltx.json` (Hintergründe) und
     `workflows/05-bild-zu-video.json` (Figur animieren).
   - Separater T5-Encoder (type `ltxv`) — Checkpoint enthält keinen.

4. **Konsistente 3D-Figur**
   - `workflows/07-svg-zu-3d-figur.json`: Pingu-SVG → `rsvg-convert` PNG →
     Lineart-ControlNet (SDXL Union Promax) → SDXL koloriert mit Tiefe/Licht.

5. **Vertonung**
   - 5-Satz-Narration „Du bist das Universum", ElevenLabs OPA
     (Stability 45 / Similarity 75 / Style 15), pro Satz eine MP3.

6. **Montage (ffmpeg)**
   - Jede Szene per `setpts` auf die Länge ihres Satzes (Zeitlupe),
     `minterpolate` für flüssige 25 fps, Ein-/Ausblendung je Szene.
   - Szenen + Voiceover concat, global Fade-in/-out + Vignette → MP4.

## Hardware-Notiz

- Render-Schwelle wie gehabt: lokal ok für kurze Clips; sehr große Modelle (Wan 14B,
  mehrminütige Videos) gehören in die Cloud.
