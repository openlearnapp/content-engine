# Versuch 1 — Plan: Durchführung AnimateDiff-Video

Gehört zu `specs/001-echtes-video-animatediff.md`.

## Setup

- ComfyUI 0.21.1 (`~/ComfyUI`), Apple Silicon / MPS
- Custom Nodes: ComfyUI-AnimateDiff-Evolved, ComfyUI-VideoHelperSuite
- Modelle: SD-1.5-Checkpoint, AnimateDiff Motion Module v3, v3-Adapter
- Sprecher (für späteren Vergleich): Edge TTS Killian, vorhanden

## Schritte

1. ComfyUI-Server mit den neuen Custom Nodes neu starten. Prüfen, dass
   AnimateDiff-Evolved und VideoHelperSuite ohne Fehler geladen werden.
2. Modelle am richtigen Ort ablegen: Checkpoint in `models/checkpoints/`,
   Motion Module in `models/animatediff_models/`, Adapter in `models/loras/`.
3. AnimateDiff-Workflow aufbauen: Text-zu-Video, 16 Frames, Auflösung 512×512
   oder 768×512 (Mac-tauglich klein anfangen).
4. Erste Render-Tests. Pro Lauf dokumentieren: Steps, CFG, Frames, Auflösung,
   Render-Zeit, RAM-Last.
5. Den Clip mit VideoHelperSuite als MP4 exportieren.
6. Optional: mit Frame-Interpolation auf flüssigere Bildrate glätten.
7. Ergebnis gegen das vorhandene SVG-Video derselben Lektion stellen
   (Schärfe, Bewegung, Stimmung, Aufwand).

## Belege sammeln

- Workflow als JSON nach `results/001-animatediff/`
- 2–3 Vorschau-Frames als PNG
- Den fertigen Clip in die Cloud, Link in die Analyse

## Auswertung

Ergebnis nach `analysis/001-versuch-1-animatediff.md` — Abschnitte:
was probiert, was geklappt, was nicht geklappt, offene Punkte.

## Risiken

- MPS unterstützt `torch.compile` nicht — Renders können langsam sein.
- Bei 24 GB RAM kann die Frame-Zahl oder Auflösung limitiert sein; im Zweifel
  kleiner anfangen und hochtasten.
