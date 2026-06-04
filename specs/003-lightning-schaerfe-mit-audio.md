# Versuch 3 — Spec: Schärfe-Sprung + Vertonung

Datum: 2026-05-24

## Frage

Lässt sich durch den Umstieg auf **AnimateDiff Lightning + DreamShaper 8** ein
deutlich schärferes Workshop-Intro-Video erzeugen als mit dem AnimateDiff-v3-Base aus
Versuch 1 — und gleichzeitig die Vertonung sauber per ElevenLabs integrieren?

## Hintergrund

- Versuch 1 hat gezeigt, dass das AnimateDiff-v3-Modul auf nacktem SD 1.5 zwar stabile
  Bewegung liefert, aber das Bild **weich und detailarm** macht. Bekannte Ursache: das
  Motion-Module verschmiert räumliches Detail über die Frames.
- Recherche-Ergebnis: **AnimateDiff Lightning (ByteDance)** ist eine 8-Step-Distillation
  des Motion-Modules. Weniger Denoising-Durchläufe = weniger Detail-Smearing.
- **DreamShaper 8 (Lykon)** ist der etablierte stilisierte SD-1.5-Checkpoint, der für
  AnimateDiff-Lightning offiziell empfohlen wird — hält Detail besser als nacktes SD 1.5.
- **Context-Window** (`ADE_StandardUniformContextOptions`) erlaubt Clips länger als
  16 Frames durch ein gleitendes Fenster — wir generieren 32 Frames.
- **ElevenLabs**-Schlüssel ist verfügbar; Voice „OPA" als warmer Erzähler.

## Erfolgskriterien

1. Render läuft auf dem Mac (24 GB MPS) ohne Out-of-Memory durch.
2. Output ist erkennbar **schärfer und detailreicher** als Versuch 1 — direkter
   Vergleich gegen das vorhandene Versuch-1-Video.
3. Audio (ElevenLabs OPA-Voice) ist sauber in das MP4 gemuxt.
4. Das Ergebnis ist als Workshop-Intro-Clip nutzbar (kein Test-Wegwerf-Output).

## Umfang

- Ein 4-Sekunden-Clip (32 Frames @ 8 fps), 768×432 (16:9).
- Eine Szene: „cozy developer workspace at night with glowing terminal".
- Eine deutsche Narration (~3–4 Sekunden, OPA-Voice).
- End-to-End: ein Skript erzeugt Audio, ein Workflow erzeugt Video, `ffmpeg` muxt.

## Nicht Teil dieses Versuchs

- ControlNet-Tile-Detail-Pass (→ Versuch 4)
- 4×-UltraSharp-Upscale im Workflow (→ Versuch 4)
- Bild-zu-Video mit LTX (→ Versuch 5)
