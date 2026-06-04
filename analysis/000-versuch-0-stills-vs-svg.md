# Versuch 0 — Auswertung: ComfyUI-Stills vs. animierte SVG

*Rückblickende Auswertung der Arbeit, die vor dem Lab schon entstanden ist. Bildet die
Ausgangslage für alle weiteren Versuche.*

Datum: 2026-05-20

## Frage

Mit welchem Weg lassen sich Workshop-Intro-Videos (~15–25 Sek pro Lektion)
produzieren — KI-Standbilder aus ComfyUI oder handgebaute animierte SVG-Grafiken?

## Aufbau

Für den Linux-Workshop wurden 20 Videos zu 10 Themen produziert, jedes Thema in
zwei Varianten:

- **ComfyUI-Pfad** — SDXL-Standbild + Ken-Burns-Kamerafahrt + deutscher KI-Sprecher.
  10 Videos, `workshop-linux-grundlagen/videos/comfyui/`.
- **SVG-Pfad** — animierte Inline-SVG-Grafik + Cinema-Overlay + derselbe KI-Sprecher.
  10 Videos, `workshop-linux-grundlagen/videos/svgs/`.

Beide nutzen denselben Sprecher (Edge TTS, Killian Neural, deutsch) und denselben
Cinema-Look (Letterbox, Vignette, Korn). Pipeline-Skripte und Doku liegen in
`workshop-linux-grundlagen/videos/pipeline/` und `videos/RECIPES.md`.

## Was geklappt hat

- Die Pipeline läuft durchgängig: Text → Audio → Bild bzw. SVG → Composite → MP4,
  und sie ist batchfähig (alle 10 Themen am Stück).
- **SVG-Pfad:** gestochen scharf, exakte Diagramme und Text, kleine Dateien (~2 MB),
  schnell, vollständig steuerbar. Strukturell nah am Premium-Edu-Prinzip (Vektor, Code).
- **ComfyUI-Pfad:** atmosphärische Tiefe und Stimmung, die der SVG-Pfad nicht liefert.
  Gut für Storytelling-Tableaus und „Wow"-Momente.
- Der KI-Sprecher (Edge TTS Killian) klingt menschlich und ist kostenlos.

## Was nicht geklappt hat

- Der ComfyUI-Pfad zeigt nur ein **Standbild mit Zoom** — die Bewegung ist
  vorgetäuscht, kein echtes Video. Das ist die größte Lücke.
- SDXL allein erzeugt **keinen einheitlichen Stil** über die 10 Videos und keine
  wiedererkennbaren Figuren über mehrere Bilder.
- Der SVG-Pfad ist **handgebaut** — jede Lektion ist Handarbeit, das skaliert nicht
  automatisch.
- 24 GB RAM begrenzen die Modellwahl (siehe `README.md`).

## Erkenntnis und Empfehlung

Beide Pfade haben je eine klare Stärke — Atmosphäre (ComfyUI) gegen Präzision (SVG).
Der Weg nach vorn ist ein **Hybrid**: ComfyUI als Asset-Fabrik (Stil, Atmosphäre,
Masse) plus SVG/Code für die präzise Animation. Was komplett fehlt, ist **echtes
KI-bewegtes Video** — das ist der Inhalt von Versuch 1.

## Belege

- 20 Videos lokal: `workshop-linux-grundlagen/videos/comfyui/` und `/svgs/`
- Cloud-Ablage für große Dateien: noch festzulegen

## Offene Punkte

- Lohnt sich der handgebaute SVG-Pfad langfristig, oder soll alles über KI-Video laufen?
- Stil-Richtung festlegen — fotorealistisch/cinematisch oder flache Illustration.
