# Content Engine

Werkzeug-Repository für die automatisierte Produktion von Workshop-Inhalten —
Hintergründe, Szenen-Bilder, Vorlagen und Videos — mit ComfyUI und KI-Modellen,
vollständig lokal.

## Ziel

Aus einem definierten Eingabe-Material (Thema, Text) automatisiert hochwertige
Bild- und Video-Inhalte für Workshops erzeugen. Kern ist eine Sammlung
**wiederverwendbarer, fest definierter Workflows** — jeder Workflow hat genau eine
Aufgabe und ist per Skript ansteuerbar.

## Aufbau

| Ordner | Inhalt |
|---|---|
| `workflows/` | Wiederverwendbare ComfyUI-Workflows (JSON) — je ein Workflow pro Aufgabe |
| `specs/` | Was ein Versuch testet, welche Frage er beantwortet |
| `plans/` | Durchführung eines Versuchs — Schritte, Setup, Modelle |
| `analysis/` | Auswertung — Ergebnis, was funktioniert, was nicht, offene Punkte |
| `results/` | Belege: Screenshots, kurze Clips, Workflow-Dateien |

## Workflow-Strategie

Statt für jedes Video alles neu zu bauen, gibt es einen **festen Satz benannter
Workflows**. Jeder erledigt einen Schritt und liefert ein definiertes Ergebnis.
Beim Aufruf wird nur der Text-Prompt gesetzt — alle übrigen Einstellungen sind fix
hinterlegt, damit die Qualität reproduzierbar bleibt.

| Workflow | Aufgabe | Ausgabe |
|---|---|---|
| `01-hintergrund-plate` | Atmosphärische Hintergründe / Backplates | hochauflösendes Standbild |
| `02-szenen-bild` | Vollständige Szenen-Illustration mit Motiv | hochauflösendes Standbild |
| `03-text-zu-video` | Text → bewegtes Video | kurzer Video-Clip |
| `04-text-zu-video-lightning` | Text → schärferes Video (Lightning-Distillation) | kurzer Video-Clip |
| `05-svg-zu-illustration` | SVG-Vorlage → kolorierte KI-Illustration *(geplant, ControlNet-Union)* | Standbild |
| `06-bild-zu-video` | Standbild → bewegter Clip *(geplant, LTX-Video / AnimateDiff-Lightning)* | Clip aus Standbild |

Details und Aufruf: `workflows/README.md`. Die Workflows lassen sich verketten zu
einer durchgehenden Automatisierung: Text → Bilder → Bewegung → Vertonung → Video.

## Werkzeug-Stand (lokal, Apple Silicon, 24 GB RAM, MPS)

| Bereich | Installiert |
|---|---|
| Engine | ComfyUI 0.21.1 |
| Bild-Modelle | SDXL Base 1.0, SD 1.5, ToonYou Beta6, DreamShaper 8, FLUX Schnell (GGUF Q4) |
| Video-Modelle | AnimateDiff v3, AnimateDiff Lightning (4-step), LTX-Video 2B |
| Speed-LoRAs | Hyper-SDXL 8-step, Hyper-SD15 8-step |
| Style-LoRAs | DoctorDiffusion Vector-Art-XL |
| Motion-LoRAs | Pan-Left/Right, Zoom-In/Out, Tilt-Up/Down |
| Steuerung | ControlNet Tile (SD1.5), ControlNet Union (SDXL, Promax), IPAdapter Plus (SDXL + SD1.5) |
| Vision-Encoder | CLIP-ViT-H-14 (LAION-2B) |
| Detail-Pass | Impact-Pack FaceDetailer, SAM-HQ, YOLOv8-Face |
| Upscale | 4× UltraSharp |
| Custom-Nodes | rgthree, Inspire-Pack, Impact-Pack (+Subpack), Crystools, GGUF-Loader, efficiency-nodes, ControlNet-Aux, IPAdapter-Plus, AnimateDiff-Evolved, VideoHelperSuite, Frame-Interpolation, Manager |
| Sprecher | ElevenLabs (Cloud-API, mehrsprachig) · Edge TTS (lokal, Fallback) |

Alle Bild-/Video-/Workflow-Werkzeuge laufen vollständig lokal — Inhalte verlassen
den Rechner nur für ElevenLabs-Voiceover (verschlüsselt zu deren API).
Grenze: sehr große Video-Modelle (LTX 2.3 22B, FLUX Dev BF16, Wan, HunyuanVideo)
brauchen mehr Speicher als 24 GB RAM bieten.

## Methode

Ein Versuch = eine Nummer = `specs/NNN` + `plans/NNN` + `analysis/NNN`.
Kleine Belege liegen in `results/`, große Renders außerhalb des Repos.

## Roadmap

- **Versuch 0** — Auswertung vorhandener Test-Videos (Stills vs. SVG) · erledigt
- **Versuch 1** — Erstes KI-bewegtes Video (AnimateDiff) · erledigt
- **Versuch 2** — Feste Workflow-Bibliothek + erste Produktions-Durchläufe · erledigt
- **Versuch 3** — Schärfe-Sprung (Lightning + DreamShaper) + ElevenLabs-Vertonung · erledigt
- **Versuch 4** — Profi-Stack-Foundation: Hyper-SD, ToonYou, DD-Vector, FLUX-GGUF, 7 Custom Nodes · erledigt
- **Versuch 5** — Kurzgesagt-Wow-Short: 30-Sek-Komposition, 4 Übergangs-Typen, ElevenLabs-Voiceover · in Arbeit
- **Versuch 6** — SVG-getriebener Kurzgesagt-Workflow (Lineart-ControlNet + ToonYou koloriert SVG-Vorlagen)
- **Versuch 7** — Charakter-Konsistenz: Pingu/Linus per IPAdapter + späteres LoRA-Training
- **Versuch 8** — End-to-End-Fabrik: Lektions-Text → fertiges narriertes Workshop-Video

## Status

Stand 2026-06-04. Werkzeuge ausgebaut auf Profi-Stack (FLUX-GGUF, Hyper-SD,
ControlNet-Union, IPAdapter-Plus, AnimateDiff-Lightning, 6 Motion-LoRAs).
Versuche 0–4 ausgewertet und gemerged. Versuch 5 als Iterations-Branch offen:
30-Sek-Wow-Short läuft lokal mit ElevenLabs-Voiceover und 4 unterschiedlichen
Übergangstypen — wird in der nächsten Session auf Kurzgesagt-Niveau geschärft.
