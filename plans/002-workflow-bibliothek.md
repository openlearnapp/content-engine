# Versuch 2 — Plan: Durchführung Workflow-Bibliothek

Gehört zu `specs/002-workflow-bibliothek.md`.

## Setup

- ComfyUI 0.21.1, Server auf `http://127.0.0.1:8188`
- Modelle: SDXL Base 1.0 (Bild), SD 1.5 + AnimateDiff v3 (Video)

## Schritte

1. Die drei Workflows als JSON in `workflows/` anlegen (API-Format).
2. Jeden Workflow per `POST /prompt` mit einem Produktions-Prompt aufrufen.
3. Für `01` mehrere Hintergründe zu Workshop-Themen erzeugen.
4. Ergebnisse aus `~/ComfyUI/output/` sichten — Schärfe, Themenbezug, Brauchbarkeit.
5. Brauchbare Belege nach `results/002-workflow-bibliothek/` kopieren.
6. Auswertung in `analysis/002-versuch-2-workflow-bibliothek.md`.

## Belege sammeln

- Erzeugte Bilder als Vorschau in `results/002-workflow-bibliothek/`
- Die verwendeten Workflow-Dateien sind bereits in `workflows/` versioniert

## Risiken

- SDXL braucht auf dem Mac ~1,5 Min pro Bild — Batch entsprechend einplanen.
- Das nackte SDXL-Basis-Modell liefert evtl. nicht die finale Ziel-Qualität;
  das ist eine erwartete Erkenntnis und führt zu Versuch 3 (stärkerer Checkpoint).
