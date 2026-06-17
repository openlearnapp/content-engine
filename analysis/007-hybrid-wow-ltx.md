# Versuch 7 — Auswertung: Hybrid-Wow-Video (LTX-Video + 3D-Figur)

Gehört zu `specs/007` und `plans/007`. Datum: 2026-06-17.

## Frage

Lässt sich lokal (Mac, 24 GB, MPS) ein vertontes, mehrszeniges Wow-Video bauen —
und taugt Wan 2.2 dafür als Engine?

## Was probiert — und das zentrale Ergebnis zu Wan 2.2

**Wan 2.2 5B läuft auf diesem Apple-Silicon/MPS-Setup NICHT.** Das wurde
erschöpfend nachgewiesen, nicht vermutet:

- Fehlende Wan-2.2-VAE nachgeladen (das 5B braucht die 48-Kanal-/16-VAE, nicht die
  2.1-VAE). Erst danach lief die Pipeline überhaupt durch.
- Erste Renders: erst Grau (korrupte VAE-Datei — Download vorzeitig beendet), nach
  sauberem Neuladen dann **buntes Rauschen** statt Bild.
- Eingrenzung mit isolierten Tests:
  - **VAE**: Round-Trip eines echten Bildes rekonstruiert perfekt → VAE ok.
  - **Modell**: alle 825 Tensoren ohne NaN/Inf, plausible Stats → Gewichte ok.
  - **Text-Encoder**: gültige Embeddings (kein NaN) → Konditionierung ok.
  - **Sigma-Kurve**: erreicht sauber 0 → Scheduler ok.
- Trotzdem rauscht das Sampling — **unabhängig** von Auflösung (512/768/832/960),
  Sampler (euler/uni_pc/dpmpp), Scheduler (simple/beta), Shift (5/8),
  **Präzision (fp16/bf16/fp32 — bit-ähnlich identisch falsch)** und
  **Attention-Backend (sub-quadratic/split/pytorch-SDPA)**, sowohl bei t2v als auch i2v.
- ComfyUI-Update **0.21.1 → 0.25.0** (PyTorch 2.12) getestet: **behebt es nicht**.
  Danach sauber auf 0.21.1 zurückgerollt.

**Schluss:** Der Modell-Forward liefert auf MPS deterministisch falsche Vorhersagen —
ein ComfyUI/PyTorch-MPS-Bug für die Wan-DiT-Architektur, keine Konfigurationsfrage.
Auf NVIDIA/CUDA (lokal mit passender GPU oder Cloud) würde dasselbe Modell laufen.

## Was geklappt hat (der lieferbare Weg)

- **LTX-Video 2B** läuft lokal hervorragend (andere Architektur als Wan):
  - Text-zu-Video: 5 kinoreife Clips (Nebel, Sternengeburt, Ozean, Wald,
    Sonnenuntergang) in 3–9 min @ 704×480.
  - Bild-zu-Video: animiert ein Standbild, Figur bleibt erkennbar.
  - Wichtig: LTX braucht einen **separaten T5-Encoder** (`type ltxv`) — der
    Checkpoint enthält keinen (sonst „clip input is invalid: None").
- **SVG → ControlNet → SDXL** verwandelt die flache Pingu-SVG in eine glänzende
  **3D-Figur mit Rim-Light, Schatten und Tiefe** — Form bleibt durch das
  Lineart-ControlNet konsistent. Das ist das Versuch-6-Ziel („3D-Figuren") auf
  funktionierendem Weg.
- **Endprodukt:** `wow-du-bist-das-universum.mp4` — 29,5 s, 1280×720, 5 Szenen,
  ElevenLabs-OPA-Voiceover, Zeitlupe + Ein-/Ausblendung + Vignette. Erstes echtes,
  herunterladbares MP4 dieses Repos jenseits der frühen Clips.

## Was (noch) nicht ausreicht

- LTX-i2v verzerrt den Hintergrund bei starker Bewegung (2B-Grenze) — Figur gut,
  Umgebung wabert.
- Kein On-Screen-Text (lokaler ffmpeg-Build ohne `drawtext`/freetype) und keine
  Musik/SFX — für V2.
- Wan-Qualität (14B, mehrminütig, höchste Konsistenz) bleibt der Cloud vorbehalten.

## Render-/Build-Daten

| Asset | Größe | Quelle |
|---|---|---|
| `wow-du-bist-das-universum.mp4` | ~6,1 MB | 5 Szenen + Voiceover, ffmpeg |
| `figur-pingu-3d.png` | ~1,0 MB | SVG → ControlNet → SDXL, 1024² |
| `figur-svg-vorlage.png` | ~190 KB | rsvg-convert der Pingu-SVG (ControlNet-Input) |
| `szene-sternengeburt.png` / `szene-pingu-sterne.png` | ~0,5–0,7 MB | Einzelframes |
| `voiceover-opa.m4a` | ~0,7 MB | ElevenLabs OPA, 5 Sätze, 30,4 s |

## Erkenntnis & nächste Schritte

1. **Lokaler Standard für bewegtes Video ist jetzt LTX-Video 2B**, nicht Wan.
   Workflows fest hinterlegt: `05-bild-zu-video`, `06-text-zu-video-ltx`,
   `07-svg-zu-3d-figur`.
2. **Cloud-Option für Wan/lange Videos** recherchiert: stundenweise GPU-Miete
   (RunPod/Vast) + dauerhafte Festplatte ist günstiger als Dauer-Miete; Colab Pro
   als einfachster Einstieg. (Keine 24/7-Miete nötig.)
3. **V2-Agenda:** Musik/SFX, On-Screen-Text (ffmpeg mit freetype), längere/ruhigere
   Szenen, Figur über mehrere Szenen konsistent halten (IPAdapter).
