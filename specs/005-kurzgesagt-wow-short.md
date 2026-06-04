# Versuch 5 — Spec: Kurzgesagt-Wow-Short

Datum: 2026-06-04

## Frage

Lässt sich aus dem vorhandenen Stack (139 SVGs Cinematic-Bibliothek + ElevenLabs +
ComfyUI-Profi-Stack aus Versuch 4) ein 75-Sek-Short produzieren, der den Kurzgesagt-
Standard erreicht oder übertrifft — gemessen an Pacing, Übergangs-Choreographie, Audio-
Qualität und konsistenter Bildsprache?

## Hintergrund

Die 10-Min-Lektion `strom-flow-10min` liefert das narrative Gerüst und die Asset-
Auswahl. Für einen Short muss die Story auf 5 Hero-Beats kondensiert werden, mit
expliziter Übergangs-Choreographie statt simpler Cross-Fade.

## Erfolgskriterien

1. 75-Sek-Video, 5 Szenen à 12-18 Sek, alle vier Übergangs-Typen mindestens einmal
   eingesetzt (Camera-Pan, Wolken-Wipe, Line-Wipe, Iris/Zoom).
2. ElevenLabs-Voiceover mit SSML-Pausen, Stability 45, Similarity 75.
3. SFX-Layer für jeden Übergang (Whoosh, Klick, Surr) plus Ambient-Pad.
4. Cinema-Look komplett: Letterbox, Vignette, Grain, Lens-Flares, Dolly-In.
5. Subjective Wow-Test: schlägt unsere Strom-Flow-10min in der ersten Minute.

## Umfang

- HTML/CSS/JS-Komposition (self-contained, kein Build-Tool, kein Remotion)
- 5 Hero-Szenen aus SVG-Bibliothek + ggf. ComfyUI-Atmosphäre-Layer
- Audio: ElevenLabs Opa-Voice + CC0-SFX
- Lokales Browser-Demo, kein Render-Pipeline-Zwang

## Nicht Teil dieses Versuchs

- MP4-Export (wenn HTML-Demo überzeugt, in Versuch 6)
- Mehrsprachige Voiceover-Varianten
- Workshop-YAML-Integration
- ComfyUI-Lightning-Animation pro Szene (Optional-Stretch, nur falls Downloads rechtzeitig)
