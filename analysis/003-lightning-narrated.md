# Versuch 3 — Auswertung: Lightning + DreamShaper + ElevenLabs-Vertonung

Gehört zu `specs/003` und `plans/003`. Datum: 2026-05-24.

## Frage

Lässt sich durch Umstieg auf AnimateDiff Lightning + DreamShaper 8 ein deutlich
schärferes Video erzeugen als mit dem AnimateDiff-v3-Base aus Versuch 1 — und
gleichzeitig die Vertonung sauber per ElevenLabs integrieren?

## Was probiert

End-to-End-Pipeline in einem Skript:

1. **Audio:** ElevenLabs-API (`eleven_multilingual_v2`, Voice „OPA") für die Narration
   „Willkommen in der Linux-Werkstatt. Heute geht's los." → MP3 (50 KB, 3,2 s).
2. **Video:** `workflows/04-text-zu-video-lightning.json` per ComfyUI-API:
   DreamShaper 8 + AnimateDiff Lightning 8-Step + Context-Window
   (`ADE_StandardUniformContextOptions`, length 16, overlap 4).
3. **Mux:** `ffmpeg -c:v copy -c:a aac -shortest` → finales MP4.

### Render-Daten

| Parameter | Wert |
|---|---|
| Checkpoint | DreamShaper 8 (Lykon) |
| Motion-Module | AnimateDiff Lightning 8-Step (ByteDance) |
| beta_schedule | `lcm` (Pflicht für Lightning) |
| Sampler / Scheduler | `euler` / `sgm_uniform` |
| Steps · CFG | 8 · 1,8 |
| Auflösung | 768×432 (16:9) |
| Frames | 32 angefordert, 25 im Output (Context-Window-Trim) |
| Frame-Rate | 8 fps → 3,125 s Clip |
| Render-Zeit | 400 s (~6,7 Min) auf Mac M-Series / MPS |

## Was geklappt hat

- **Schärfe-Sprung ist eindeutig.** Versuch 1 lieferte ein abstraktes
  blau-industrielles Muster; Versuch 3 zeigt einen erkennbaren cozy Workspace mit
  Monitor (grünes Terminal), Schreibtischlampen, Stiften, Büchern. Vergleich:
  `results/003-lightning-narrated/vorher-nachher.png`.
- **Temporale Stabilität bleibt erhalten.** Über die 25 Frames bleibt die Szene
  konsistent, leichte Kamera-Bewegung, kein Flackern.
- **Pipeline ist End-to-End funktional.** Ein Skript erzeugt Audio, rendert Video
  und muxt — ohne manuellen Eingriff. Das ist die Vorstufe zur Content-Fabrik aus
  der Roadmap (Versuch 5).
- **ElevenLabs-Voice OPA integriert** — Audio sitzt im finalen MP4, Länge passt
  mit dem Video (3,1 s vs. 3,2 s, `-shortest` cuttet sauber).
- **Render-Zeit machbar** auf 24 GB Mac MPS: ~6,7 Min für einen vertonten Clip.

## Was nicht geklappt hat

- **Frame-Trim:** 32 angefordert, 25 herausgekommen — Context-Window stride/overlap
  trimmt am Ende. Kein Render-Fehler, nur ein Längen-Hinweis (3,1 s statt 4 s).
- **Länge bleibt kurz.** 3 s sind für eine echte Lektion zu knapp; das löst ein
  späterer Versuch über längeres Context-Window oder Mehrfach-Clips + Schnitt.
- **Detail im Hintergrund** noch weicher als beim Standbild — der Tile-ControlNet-
  Pass aus dem Recherche-Plan ist hier bewusst noch nicht drin (Versuch 4).

## Erkenntnis

**Der Umstieg auf Lightning + DreamShaper ist der erste echte Qualitäts-Sprung
des Projekts.** Der frühere Eindruck „AnimateDiff ist nur für Atmosphäre, nicht für
konkrete Szenen" stimmt mit dem Base-Modul — mit Lightning + stilisiertem Checkpoint
liefert es konkrete, lesbare Szenen.

Die End-to-End-Vertonung ist das zweite große Stück: Audio kommt nicht mehr per
Hand-Composite, sondern ist im Pipeline-Lauf integriert.

Damit ist die Basis für die Content-Fabrik gelegt: ein Workflow pro Aufgabe,
ein Skript verkettet sie, ein MP4 fällt hinten raus.

## Belege

`results/003-lightning-narrated/`
- `final-vertont.mp4` — das vertonte Endprodukt (3,1 s, 370 KB) — **das Hauptergebnis**
- `video-ohne-audio.mp4` — derselbe Clip ohne Audio (für Vergleich)
- `narration-opa.mp3` — die ElevenLabs-OPA-Stimme allein
- `vorher-nachher.png` — Vergleich Versuch 1 (links) gegen Versuch 3 (rechts)

## Pipeline (Architektur)

```
narration-text  ──►  ElevenLabs-API  ──►  audio.mp3
                                                │
prompt-text  ──►  ComfyUI-API (workflow 04)  ──►  video.mp4
                                                │
                                       ffmpeg mux  ──►  final.mp4
```

Workflow versioniert in `workflows/04-text-zu-video-lightning.json`. Audio-Zugang
über ENV-Variablen (`ELEVENLABS_API_KEY`, `ELEVENLABS_VOICE_ID_OPA`) außerhalb des Repos.

## Offene Punkte

- ControlNet-Tile-Detail-Pass für noch mehr Schärfe (→ Versuch 4).
- 4×-UltraSharp-Upscale auf 2K als Final-Pass (→ Versuch 4).
- Längere Clips über ausgedehntes Context-Window oder Mehrfach-Segmente (→ Versuch 5).
- Workflow für Audio-Mux direkt in ComfyUI (per `VHS_LoadAudio` + `VHS_VideoCombine`),
  um den externen `ffmpeg`-Schritt zu sparen.
