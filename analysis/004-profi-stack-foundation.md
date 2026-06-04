# Versuch 4 — Auswertung: Profi-Stack-Foundation

Gehört zu `specs/004` und `plans/004`. Datum: 2026-06-03.

## Frage

Welche Profi-Werkzeuge fehlen tatsächlich in unserer ComfyUI-Setup, und lassen sich
alle Bausteine auf 24 GB Mac MPS sauber installieren und laden?

## Was probiert

7 Custom-Node-Pakete geklont und Python-Requirements installiert. 10 Modelle (~17 GB
nach finaler Größenmessung) von HuggingFace geladen und in die ComfyUI-Modell-Ordner
einsortiert. ComfyUI-Server neu gestartet und per `/object_info`-API verifiziert, dass
alle neuen Modelle in den jeweiligen Loadern auftauchen.

## Was geklappt hat

**Alle 7 Custom Nodes geladen, keine Import-Errors** (laut Server-Log):

| Pack | Lade-Zeit | sichtbare Nodes |
|---|---|---|
| rgthree-comfy | 0,0 s | 24 |
| Inspire-Pack | 0,1 s | 101 |
| Impact-Pack | 0,7 s | (Detailer-Pipeline) |
| Impact-Subpack | 0,4 s | (Ultralytics-Brücke) |
| Crystools | 2,6 s | 29 |
| GGUF-Loader | 0,0 s | UnetLoaderGGUF, DualCLIPLoaderGGUF u.a. |
| efficiency-nodes | 0,0 s | 4 |

**Alle 10 Modelle in den Loader-Dropdowns sichtbar** (verifiziert via API):

- `CheckpointLoaderSimple` enthält `toonyou_beta6.safetensors`
- `LoraLoader` enthält `Hyper-SDXL-8steps-CFG-lora`, `Hyper-SD15-8steps-CFG-lora`, `DD-vector-v2`
- `VAELoader` enthält `flux_ae.safetensors`
- `UnetLoaderGGUF` enthält `flux1-schnell-Q4_K_S.gguf`
- `DualCLIPLoaderGGUF` enthält `clip_l.safetensors` und `t5-v1_1-xxl-encoder-Q4_K_S.gguf`

**Python-Smoke-Test grün:** `gguf, sentencepiece, psutil, ultralytics, segment_anything, onnxruntime` alle importierbar.

**Mac-Fix erfolgreich angewendet:** `onnxruntime-gpu` aus den Requirements gefiltert,
stattdessen reguläres `onnxruntime` installiert. Kein Konflikt mit den CUDA-Erwartungen
einzelner Pakete.

## Was nicht geklappt hat

- **FLUX-GGUF und T5-GGUF erster Download silent abgebrochen.** Erster Lauf ohne
  `--fail` lieferte unvollständige Dateien (1,9 GB statt 6,8 GB; 469 MB statt 2,7 GB)
  ohne curl-Fehlercode. Re-Download mit `curl --fail -L --connect-timeout 30
  --max-time 3600` lief sauber durch. → **Lektion:** für >1-GB-HuggingFace-Downloads
  immer `--fail` setzen, Größe gegen Content-Length prüfen.
- **FLUX-VAE ist gated** im offiziellen Black-Forest-Labs-Repo. Workaround:
  `Comfy-Org/Lumina_Image_2.0_Repackaged` enthält denselben FLUX-VAE öffentlich
  (Lumina 2.0 ist FLUX-basiert) — 320 MB Datei verifiziert geladen.
- **`insightface`-basierte Konsistenz-Stacks (InstantID, PuLID-Flux)** bewusst NICHT
  installiert — auf Apple-Silicon-arm64 zu fragil (Issue cubiq/ComfyUI_IPAdapter_plus#493).
  Charakter-Konsistenz später über IPAdapter Plus + LoRA-Training.

## Render-/Install-Daten

| Datei | Soll | Ist | Status |
|---|---|---|---|
| `Hyper-SDXL-8steps-CFG-lora.safetensors` | ~787 MB | 751 MB | OK |
| `Hyper-SD15-8steps-CFG-lora.safetensors` | ~269 MB | 257 MB | OK |
| `toonyou_beta6.safetensors` | ~2,1 GB | 2,1 GB | OK |
| `DD-vector-v2.safetensors` | ~218 MB | 218 MB | OK |
| `flux1-schnell-Q4_K_S.gguf` | ~6,8 GB | 6,3 GB *(binär = 6,78 GB dezimal)* | OK |
| `t5-v1_1-xxl-encoder-Q4_K_S.gguf` | ~2,7 GB | 2,5 GB *(binär = 2,74 GB dezimal)* | OK |
| `flux_ae.safetensors` | ~335 MB | 320 MB | OK |
| `clip_l.safetensors` | ~246 MB | 235 MB | OK |
| `face_yolov8n.pt` | ~6 MB | 5,9 MB | OK |
| `sam_hq_vit_b.pth` | ~362 MB | 362 MB | OK |

Größen-Diskrepanzen erklären sich durch dezimal (HF) vs. binär (`ls -lh`) — alle Dateien
voll und ladbar.

## Erkenntnis

Die Setup-Lücke ist geschlossen. Vorher: ein Modell, ein KSampler, fertig. Jetzt steht
das Profi-Inventar bereit für die Workflow-Pattern aus
`reference_comfyui_full_power.md`: Hires-Fix, ControlNet-Stacking, IPAdapter-Konsistenz,
FaceDetailer-Pass, Hyper-SD-Distillation, GGUF-quantisiertes FLUX.

Das ist die Voraussetzung für den eigentlichen Qualitäts-Sprung in den nächsten Versuchen.

## Belege

`results/004-profi-stack/`
- `server-log-custom-nodes.txt` — Lade-Status aller Custom-Node-Pakete aus dem Server-Log
- `loader-snapshot.json` — Inhalt der Loader-Dropdowns nach Install (Beweis dass alle Modelle sichtbar sind)

## Offene Punkte

- **SDXL-spezifische ControlNet-Modelle** (Depth, Lineart, Canny SDXL-Versionen) — für
  den SVG-getriebenen Premium-Edu-Test (Versuch 5) zusätzlich nötig.
- **Erster Profi-Workflow** mit den neuen Bausteinen ist Inhalt von Versuch 5 — der
  eigentliche Stärken-Beweis kommt dort.
- **kohya_ss** für LoRA-Training des Maskottchens — eigener Versuch wenn benötigt.
