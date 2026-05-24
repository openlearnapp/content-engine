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
| `04-bild-zu-video` | Standbild → bewegtes Video *(geplant, LTX-Video)* | Clip aus Standbild |

Details und Aufruf: `workflows/README.md`. Die Workflows lassen sich verketten zu
einer durchgehenden Automatisierung: Text → Bilder → Bewegung → Vertonung → Video.

## Werkzeug-Stand (lokal, Apple Silicon, 24 GB RAM, MPS)

| Bereich | Installiert |
|---|---|
| Engine | ComfyUI 0.21.1 |
| Bild | SDXL Base 1.0, SD 1.5 |
| Video | AnimateDiff (v3), LTX-Video 2B |
| Steuerung | ControlNet, ControlNet-Aux, IPAdapter |
| Pipeline | VideoHelperSuite, Frame-Interpolation |
| Sprecher | Edge TTS (deutsch, neural) |

Alle Werkzeuge laufen vollständig lokal — keine Inhalte verlassen den Rechner.
Grenze: sehr große Video-Modelle (13–14 Mrd. Parameter) brauchen mehr Speicher als
24 GB RAM bieten.

## Methode

Ein Versuch = eine Nummer = `specs/NNN` + `plans/NNN` + `analysis/NNN`.
Kleine Belege liegen in `results/`, große Renders außerhalb des Repos.

## Roadmap

- **Versuch 0** — Auswertung vorhandener Test-Videos (Stills vs. SVG) · erledigt
- **Versuch 1** — Erstes KI-bewegtes Video (AnimateDiff) · erledigt
- **Versuch 2** — Feste Workflow-Bibliothek + erste Produktions-Durchläufe · erledigt
- **Versuch 3** — Schärfe steigern: stärkerer Checkpoint + ControlNet
- **Versuch 4** — Bild-zu-Video mit LTX-Video
- **Versuch 5** — End-to-End: Eingabe-Text → fertiges vertontes Video

## Status

Stand 2026-05-20. Werkzeuge installiert, Workflow-Bibliothek aufgebaut und
produktiv getestet, Versuche 0 bis 2 ausgewertet.
