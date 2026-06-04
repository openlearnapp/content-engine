# Versuch 6 — Plan: 3D-Figuren mit Licht und Schatten

Gehört zu `specs/006-3d-figuren-licht-schatten.md`.

## Setup

- ComfyUI 0.21.1 läuft auf `:8188` (siehe `reference_comfyui_setup`)
- Profi-Stack aus V4 + V5-Downloads vollständig installiert (siehe
  `project_current_state` 2026-06-04, Abschnitt „Profi-Stack")
- Cinematic-SVG-Bibliothek in `lernvideo-asset-system/asset-bibliothek/cinematic/`

## Story-Beats wie in V5 (Wiederverwendung)

| # | Beat | Hero-Figur | SVG-Lineart-Quelle |
|---|---|---|---|
| 1 | Hook: Mensch tippt | mensch-tippend | menschen/mensch-tippend.svg |
| 2 | Strom flutet ins Motherboard | Stromquelle + Board | flow/strom-quelle.svg + hardware/motherboard-topdown.svg |
| 3 | CPU + RAM erwachen | cpu-chip + ram-stick | tech-kern/cpu-chip-3d.svg + hardware/ram-stick-3d.svg |
| 4 | Daten fließen durch Kernel | daten-portal + terminal | tech-kern/daten-portal.svg + tech-kern/terminal-glas.svg |
| 5 | Pingu winkt | pingu-winkend | menschen/pingu-winkend.svg |

## ComfyUI-Workflow (neuer: 05-svg-zu-illustration-plastisch.json)

```
SVG-Lineart-PNG (1024×1024)
  ↓
ControlNet-Union-SDXL (mode=lineart, strength=0.85)
  ↓
SDXL Base + Hyper-SDXL-8steps-CFG-lora (sampler=lcm, steps=8, cfg=2.0)
  + DD-vector-v2 LoRA (weight=0.6)
  + IPAdapter-Plus-SDXL mit pingu-cinematic-rendered.png (weight=0.7)
  ↓
KSampler (1024×1024 → Latent)
  ↓
VAE Decode → PNG-Out (1024×1024)
  ↓
Hires-Fix Pass:
  - Upscale 2× via 4xUltraSharp
  - Tile-ControlNet (strength=0.4, denoise=0.4)
  - Re-Sample → 2048×2048
  ↓
FaceDetailer Pass:
  - face_yolov8n detect
  - sam_hq_vit_b segment
  - SDXL re-render face region (denoise=0.35)
  ↓
Crop to 1920×1080 → Save als hero-NN.png
```

## Schritte

1. **SVG → Lineart-PNG-Pipeline:**
   ```bash
   # Aus jedem benötigten SVG eine 1024×1024 PNG-Lineart-Variante rendern
   for svg in mensch-tippend strom-quelle motherboard-topdown cpu-chip-3d \
              ram-stick-3d daten-portal terminal-glas pingu-winkend \
              pingu-cinematic; do
     rsvg-convert -w 1024 -h 1024 \
       lernvideo-asset-system/asset-bibliothek/cinematic/.../${svg}.svg \
       > /tmp/v6_lineart/${svg}.png
   done
   ```
2. **IPAdapter-Referenz erstellen:** `pingu-cinematic.svg` als 1024×1024 PNG mit
   farbiger Vorlage rendern (nicht nur Lineart). Liegt in
   `results/006-3d-figuren-licht-schatten/refs/pingu-reference.png`.
3. **Workflow-JSON bauen** und mit erstem Beat (Pingu winkend) testen. Wenn
   Output plastisch aussieht, weiter mit Batch.
4. **Batch alle 5 Beats:** Python-Script ruft ComfyUI-API mit Workflow-Vorlage
   + variablen Prompt + variabler Lineart-Input. Output → `results/006-.../heros/`.
5. **demo.html aus V5 kopieren** nach `results/006-3d-figuren-licht-schatten/demo.html`,
   SVG-Pfade durch Hero-PNG-Pfade ersetzen.
6. **SFX-Layer:**
   - Freesound CC0 holen: whoosh-low, cloud-swoosh, laser-zip, mechanical-click
   - Ambient-Pad: synthwave-loop (Freesound oder Suno später)
   - In demo.html: 4 `<audio>` Tags + Timing-Events
7. **Pages-`index.html` updaten:** V4-Card + V5-Card + V6-Card hinzufügen, jede
   linkt auf `results/NNN-.../demo.html` (kein iframe, schlicht ein Link).
8. **Lokal testen** via `python3 -m http.server 8765` vom Repo-Root.
9. **Reza zeigen → OK abwarten → PR erstellen.**

## Voiceover

ElevenLabs-MP3 aus V5 (`results/005-premium-edu-short/voiceover.mp3`) bleibt
1:1 wiederverwendbar — gleiche Story, gleicher Sprecher, gleiches Timing.
Nur Pfad anpassen in V6-demo.html.

## Erwartete Render-Daten

| Phase | Zeit | Output |
|---|---|---|
| 9× SVG → PNG-Konvertierung | <1 Min | /tmp/v6_lineart/*.png |
| 1× IPAdapter-Referenz | 2 Min | refs/pingu-reference.png |
| 5× Hero-Frame Basis-Render (1024) | 5×3 Min = 15 Min | /tmp/v6_render/heros/hero-NN.png |
| 5× Hires-Fix + FaceDetailer | 5×2 Min = 10 Min | results/006-.../heros/hero-NN.png |
| demo.html-Update + SFX-Layer | 20 Min | results/006-.../demo.html |
| Tests + Iteration | 30 Min | — |

**Gesamt:** ~80 Min ohne Iterationen, ~120 Min realistisch.

## Risiken

- **Render-Zeit-Drift:** Falls 24 GB RAM zu eng werden bei IPAdapter+Hires+
  FaceDetailer im selben Workflow, Splittung in zwei Workflows (basis + finish).
- **Pingu-Charakter-Drift:** Wenn IPAdapter-Konsistenz nicht reicht, Plan B:
  einmal einen Pingu-Hero in höchster Qualität rendern, dann img2img mit niedrigem
  Denoise (0.3) für die anderen 4 Beats — garantiert Konsistenz, kostet Variation.
- **SFX-Lizenz:** Freesound CC0-Filter immer aktivieren. Im Zweifelsfall Suno
  selbst generieren.
- **Pages-Refactor scope creep:** Pages-Index nur erweitern um V4/V5/V6, nicht
  komplett umbauen.

## Belege

`results/006-3d-figuren-licht-schatten/`
- `demo.html` — Komposition mit Hero-PNGs statt SVG-Embeddings
- `heros/hero-01..05.png` — die 5 plastischen Hero-Frames
- `refs/pingu-reference.png` — IPAdapter-Referenz für Konsistenz
- `sfx/*.mp3` — 5 SFX-Spuren
- `voiceover.mp3` — Kopie/Symlink von V5
- `vorher-nachher.png` — V5-SVG-Frame vs V6-Hero-Frame nebeneinander
