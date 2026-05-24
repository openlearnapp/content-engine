# Versuch 2 — Spec: Feste Workflow-Bibliothek

Datum: 2026-05-20

## Frage

Lässt sich mit einem **festen Satz benannter Workflows** verlässlich und
reproduzierbar Workshop-Bildmaterial erzeugen — statt jeden Render von Hand neu
aufzusetzen?

## Hintergrund

Versuch 1 hat gezeigt, dass einzelne Renders funktionieren, aber jeder Workflow
wurde dafür einzeln zusammengebaut. Für eine echte Automatisierung braucht es
wiederverwendbare Workflows: ein Werkzeug pro Aufgabe, fest eingestellt, nur der
Prompt wird beim Aufruf gesetzt.

## Umfang

Drei Workflows anlegen und je einen Produktions-Durchlauf fahren:

- `01-hintergrund-plate` — atmosphärischer Hintergrund (SDXL)
- `02-szenen-bild` — Szenen-Illustration mit Motiv (SDXL)
- `03-text-zu-video` — bewegter Clip (AnimateDiff, aus Versuch 1 übernommen)

## Erfolgskriterien

1. Jeder Workflow läuft per ComfyUI-API ohne Handarbeit durch.
2. Die Ausgabe ist brauchbar (scharf, zum Thema passend).
3. Derselbe Workflow lässt sich unverändert mit neuem Prompt erneut aufrufen.

## Nicht Teil dieses Versuchs

- Bild-zu-Video mit LTX-Video (→ Versuch 4)
- Verkettung zu einer End-to-End-Pipeline (→ Versuch 5)
