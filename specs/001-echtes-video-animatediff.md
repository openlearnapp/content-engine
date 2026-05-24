# Versuch 1 — Spec: Erstes echtes KI-bewegtes Video mit AnimateDiff

Datum: 2026-05-20

## Frage

Kann AnimateDiff auf dem Mac (24 GB RAM, MPS) ein **echtes bewegtes** Erklär-Video
erzeugen, das dem bisherigen SVG-Pfad und dem Stills-plus-Kamerafahrt-Pfad aus
Versuch 0 überlegen ist?

## Hintergrund

Versuch 0 hat gezeigt: Der ComfyUI-Pfad liefert bisher nur Standbilder mit
vorgetäuschter Bewegung. AnimateDiff erzeugt aus einem Text-Prompt eine echte
Bild-Folge mit generierter Bewegung. Es ist das Video-Verfahren, das auf
Apple-Silicon-Hardware am zuverlässigsten läuft (SD-1.5-basiert, moderate Größe).

## Hypothese

AnimateDiff liefert echte, flüssige Bewegung — aber vermutlich mit dem
Diffusion-typischen leichten Flackern. Für ruhige, präzise Erklär-Inhalte könnte der
SVG-Pfad weiterhin sauberer sein. AnimateDiff dürfte bei atmosphärischen,
storytelling-artigen Szenen punkten.

## Erfolgskriterien

1. Ein Clip von 5–10 Sekunden wird auf dem Mac in vertretbarer Zeit gerendert
   (Render-Zeit dokumentiert).
2. Die Bewegung ist flüssig, ohne störendes Flackern.
3. Der Stil passt zum Workshop-Look (Cinema, ruhig, klar).
4. Direkter Vergleich gegen das vorhandene SVG-Video derselben Lektion.

## Umfang

Genau eine Lektion, eine Szene — Thema „Was ist Linux". Kein End-to-End,
kein Batch. Ziel ist eine belastbare Antwort auf die Frage oben, nicht ein
fertiges Produkt.

## Nicht Teil dieses Versuchs

- Stil-Konsistenz über mehrere Bilder (→ Versuch 2)
- Figuren-Konsistenz (→ Versuch 3)
- Automatisierung / End-to-End-Pipeline (→ Versuch 5)
