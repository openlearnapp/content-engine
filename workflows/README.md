# Workflow-Bibliothek

Ein **fester Satz benannter Workflows**. Jeder erledigt genau eine Aufgabe und ist
per ComfyUI-API ansteuerbar. Beim Aufruf wird nur der Text-Prompt gesetzt — alle
übrigen Einstellungen (Modell, Auflösung, Sampler, Steps) sind fix hinterlegt,
damit die Qualität reproduzierbar bleibt.

## Die Workflows

| Datei | Aufgabe | Modell | Ausgabe |
|---|---|---|---|
| `01-hintergrund-plate.json` | Atmosphärischer Hintergrund / Backplate | SDXL | Standbild 1344×768 |
| `02-szenen-bild.json` | Vollständige Szenen-Illustration mit Motiv | SDXL | Standbild 1344×768 |
| `03-text-zu-video.json` | Text → bewegtes Video (Baseline) | SD 1.5 + AnimateDiff v3 | Clip, 16 Frames |
| `04-text-zu-video-lightning.json` | Text → bewegtes Video (schärfer, 32 Frames) | DreamShaper 8 + AnimateDiff Lightning | Clip, 32 Frames @ 8 fps |
| `05-bild-zu-video.json` | Standbild → Video *(geplant)* | LTX-Video 2B | Clip aus Standbild |

## Format

Die JSON-Dateien liegen im **ComfyUI-API-Format** (`{"node-id": {"class_type", "inputs"}}`).
Das ist das Format für die Automatisierung — es wird per HTTP an den ComfyUI-Server
geschickt, nicht von Hand in der Oberfläche geklickt.

## Aufruf

ComfyUI-Server muss laufen (`http://127.0.0.1:8188`). Ablauf eines Aufrufs:

1. Workflow-JSON laden.
2. Den Prompt setzen (siehe Feld `_prompt_setzen` in der jeweiligen Datei).
3. Per `POST /prompt` an den Server schicken.
4. Fertiges Ergebnis aus `~/ComfyUI/output/` abholen.

## Strategie

Die Workflows sind so geschnitten, dass sie sich **verketten** lassen:

```
Thema/Text
   ├─ 01 → Hintergrund-Plate
   ├─ 02 → Szenen-Bild
   └─ 03 → bewegtes Video
            └─ + Vertonung (Edge TTS) → fertiges Workshop-Video
```

Ziel ist, pro wiederkehrender Aufgabe **einen** festen Workflow zu haben, statt
jedes Mal neu zu bauen. So entsteht eine planbare Automatisierung der
Inhalts-Produktion.
