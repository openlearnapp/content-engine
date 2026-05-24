# Versuch 1 — Auswertung: Erstes echtes KI-bewegtes Video mit AnimateDiff

Gehört zu `specs/001` und `plans/001`. Datum: 2026-05-20.

## Frage

Kann AnimateDiff auf dem Mac ein echtes bewegtes Erklär-Video erzeugen?

## Was probiert

Drei Renders auf dem Mac (Apple Silicon, MPS, 24 GB RAM), alle 512×512, 20 Steps,
CFG 7, Sampler euler/normal:

| Lauf | Setup | Render-Zeit | Ergebnis |
|---|---|---|---|
| 1a (erster Versuch) | AnimateDiff Gen1 · `beta_schedule: autoselect` · **kein Adapter** | 234 s | abstraktes orange-blaues Farbmuster — kein erkennbares Bild |
| Kontrolle | SD 1.5 allein, ein Standbild, **kein** AnimateDiff | 12 s | scharfer, sauberer Server-Raum |
| 1b (Fix) | AnimateDiff Gen1 · **v3-Adapter-LoRA (Stärke 1,0)** · `beta_schedule: sqrt_linear (AnimateDiff)` | 222 s | erkennbare Server-Raum-Szene mit echter Bewegung |

## Was geklappt hat

- **AnimateDiff läuft auf dem Mac** und erzeugt echtes bewegtes Video — kein
  vorgetäuschter Zoom mehr wie in Versuch 0.
- **Die Bewegung ist zeitlich stabil.** Über alle 16 Frames bleibt die Szene
  zusammenhängend, kein wildes Flackern oder Morphing (geprüft per Frame-Kontaktbogen).
- **Render-Zeit ist machbar:** ~3,7 Min für einen 2-Sekunden-Clip auf dem Mac.
- Die Kontrolle beweist: Basis-Modell und MPS funktionieren einwandfrei — der Fehler
  in Lauf 1a lag allein an der AnimateDiff-Konfiguration.

## Was nicht geklappt hat

- **Ohne den v3-Adapter-LoRA ist das v3-Motion-Module unbrauchbar** (Lauf 1a). Das
  v3-Modul wurde zusammen mit diesem Adapter trainiert — er ist Pflicht, nicht optional.
- **Schärfe/Detail bleiben hinter dem Standbild zurück.** Der AnimateDiff-Clip wirkt
  weicher und „unruhiger" als die Kontrolle. Das Motion-Module kostet räumliche
  Bildschärfe — bekannter Nebeneffekt.
- **Clip-Länge nur 2 Sekunden** (16 Frames). Für längere Clips braucht es
  Kontext-Fenster-Optionen (Sliding Window) — noch nicht getestet.
- Die Stil-Treue reicht noch nicht an präzise Erklär-Grafik heran.

## Erkenntnis

AnimateDiff funktioniert auf dem Mac und liefert echte, stabile Bewegung — der
Durchbruch gegenüber Versuch 0. Aber: Für **präzise Erklär-Inhalte** (scharfe
Diagramme, Text) ist der SVG-Pfad weiterhin überlegen. AnimateDiffs Stärke liegt bei
**atmosphärischen, bewegten Hintergründen und Stimmungs-Szenen**.

Das bestätigt die Hybrid-Richtung: KI-Video für Atmosphäre, SVG/Code für Präzision.

Größter nächster Hebel: ein stärkerer SD-1.5-Checkpoint (Community-Modell statt
nacktes Basis-Modell) und ControlNet zur Komposition-Steuerung.

## Belege

Im Repo (klein genug, kein Cloud-Upload nötig): `results/001-animatediff/`
- `animatediff-v3-funktioniert.mp4` — das geglückte Ergebnis (Lauf 1b)
- `animatediff-erster-versuch-fehler.mp4` — der Fehlversuch (Lauf 1a)
- `kontrolle-sd15-standbild.png` — der Kontroll-Render
- `workflow-animatediff-v3.json` — der funktionierende Workflow zum Nachbauen

## Offene Punkte

- Stärkeren SD-1.5-Checkpoint prüfen (Community-Modell mit Illustrations- oder
  Cinematic-Stil) für besseren Look.
- v3-Motion-Module mit Adapter gegen das ältere, robustere v2-Modul vergleichen.
- Schärfe zurückholen: Hires-Fix, Upscale-Durchlauf oder höhere Auflösung testen.
- Geeignete Frame-Zahl / Auflösung fürs Erklärvideo-Format auf Mac-Hardware ermitteln.
