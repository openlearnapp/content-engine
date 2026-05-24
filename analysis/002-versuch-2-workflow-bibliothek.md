# Versuch 2 — Auswertung: Feste Workflow-Bibliothek

Gehört zu `specs/002` und `plans/002`. Datum: 2026-05-20.

## Frage

Lässt sich mit einem festen Satz benannter Workflows verlässlich und reproduzierbar
Workshop-Bildmaterial erzeugen?

## Was probiert

Drei Workflows als JSON in `workflows/` angelegt und per ComfyUI-API aufgerufen —
ohne Handarbeit, beim Aufruf wurde nur der Text-Prompt gesetzt:

| Workflow | Lauf | Render-Zeit | Ergebnis |
|---|---|---|---|
| `01-hintergrund-plate` | Terminal-Workspace | 108 s | dunkler Workspace mit leuchtendem Bildschirm — brauchbar |
| `01-hintergrund-plate` | Datenfluss (abstrakt) | 102 s | starke abstrakte Daten-Landschaft, navy/cyan/magenta |
| `01-hintergrund-plate` | Computer am Schreibtisch | 102 s | warme, ruhige Schreibtisch-Szene — brauchbar |
| `03-text-zu-video` | Terminal-Szene | 228 s | bewegter Clip, atmosphärisch-abstrakt |

Alle Bild-Renders SDXL, 1344×768, 30–32 Steps. Video: SD 1.5 + AnimateDiff, 512×512, 16 Frames.

## Was geklappt hat

- **Das Workflow-Bibliothek-Konzept funktioniert.** Jeder Workflow liegt als JSON
  vor, wird per API aufgerufen, ist ohne Änderung wiederverwendbar — nur der Prompt
  wechselt.
- **Workflow `01` liefert gute Hintergründe.** Alle drei Bilder sind scharf,
  zusammenhängend und thematisch passend. Direkt als Video-Hintergründe verwendbar.
- Reproduzierbar: gleicher Workflow + neuer Prompt = neues Bild, gleiche Qualität.
- Render-Zeit ~1,7 Min pro Hintergrund auf dem Mac — gut für Batch-Produktion.

## Was nicht geklappt hat

- **Workflow `03` (Video) bleibt atmosphärisch-abstrakt.** Der Clip ist ein
  bewegtes Tech-Motiv, aber keine konkrete, lesbare Szene. Das bestätigt die
  Erkenntnis aus Versuch 1: AnimateDiff ist stark bei Stimmung, schwach bei Präzision.
- Abstrakte Prompts (Datenfluss) gelingen deutlich besser als konkrete Szenen.

## Erkenntnis

Die **Bild-Workflows sind produktionsreif** — Hintergründe lassen sich ab sofort
verlässlich und im Batch erzeugen. Der **Video-Workflow funktioniert technisch**,
braucht aber noch Qualitätsarbeit. Die Workflow-Bibliothek als Strategie ist
bestätigt: feste, benannte Workflows statt Handarbeit pro Render.

Nächster Hebel (Versuch 3): stärkerer Bild-Checkpoint und ControlNet, um auch dem
Video-Pfad konkrete, scharfe Szenen beizubringen.

## Belege

`results/002-workflow-bibliothek/`
- `hintergrund-terminal.png`, `hintergrund-datenfluss.png`, `hintergrund-computer.png`
- `text-zu-video-terminal.mp4`
- Die Workflows selbst sind in `workflows/` versioniert.

## Offene Punkte

- Video-Workflow: konkrete Szenen statt Abstraktion — über stärkeren Checkpoint und
  ControlNet angehen.
- Workflow `04` (Bild → Video mit LTX-Video) ergänzen.
- Vertonung anbinden, um aus Bild + Bewegung + Stimme ein fertiges Video zu verketten.
