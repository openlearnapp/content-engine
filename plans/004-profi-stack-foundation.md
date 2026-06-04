# Versuch 4 — Plan: Durchführung Profi-Stack-Install

Gehört zu `specs/004-profi-stack-foundation.md`.

## Setup

- ComfyUI 0.21.1 auf Apple Silicon / MPS, 24 GB unified memory
- venv `~/ComfyUI/venv/`
- Server-Kommandos siehe `reference_comfyui_setup`

## Custom Nodes (git clone in `~/ComfyUI/custom_nodes/`)

| Repo | Zweck |
|---|---|
| `rgthree/rgthree-comfy` | Workflow-QoL: Mute/Reroute/Seed-Management |
| `ltdrdata/ComfyUI-Inspire-Pack` | LoRA-Block-Weight, A1111-Prompt-Parität, Caching |
| `ltdrdata/ComfyUI-Impact-Pack` | FaceDetailer + HandDetailer + SEGS-Pipeline |
| `ltdrdata/ComfyUI-Impact-Subpack` | Impact-Pack-Erweiterung (Ultralytics-Detection) |
| `crystian/ComfyUI-Crystools` | CPU/GPU/RAM-Monitor in der UI |
| `city96/ComfyUI-GGUF` | GGUF-Loader für quantisiertes FLUX |
| `jags111/efficiency-nodes-comfyui` | XY-Plots, effizienter KSampler |

## Modelle (10 Downloads, ~14 GB)

| Datei | Zielordner | Quelle | Größe |
|---|---|---|---|
| `Hyper-SDXL-8steps-CFG-lora.safetensors` | `models/loras/` | ByteDance/Hyper-SD | ~787 MB |
| `Hyper-SD15-8steps-CFG-lora.safetensors` | `models/loras/` | ByteDance/Hyper-SD | ~134 MB |
| `toonyou_beta6.safetensors` | `models/checkpoints/` | frankjoshua/toonyou_beta6 | ~2,1 GB |
| `DD-vector-v2.safetensors` | `models/loras/` | DoctorDiffusion | ~200 MB |
| `flux1-schnell-Q4_K_S.gguf` | `models/diffusion_models/` | city96/FLUX.1-schnell-gguf | ~6,8 GB |
| `t5-v1_1-xxl-encoder-Q4_K_S.gguf` | `models/text_encoders/` | city96/t5-v1_1-xxl-encoder-gguf | ~3 GB |
| `flux_ae.safetensors` | `models/vae/` | Comfy-Org/Lumina_Image_2.0_Repackaged | ~335 MB |
| `clip_l.safetensors` | `models/text_encoders/` | comfyanonymous/flux_text_encoders | ~246 MB |
| `face_yolov8n.pt` | `models/ultralytics/bbox/` | Bingsu/adetailer | ~6 MB |
| `sam_hq_vit_b.pth` | `models/sams/` | lkeab/hq-sam | ~362 MB |

## Schritte

1. Branch `versuch-4-profi-stack` auf `content-engine` von `main` abzweigen.
2. Sieben Custom Nodes klonen.
3. Pip-Requirements installieren — Mac-Fix: `onnxruntime-gpu` aus jeder
   `requirements.txt` filtern, `onnxruntime` separat sicherstellen.
4. Zehn Modelle parallel im Hintergrund herunterladen.
5. ComfyUI-Server neu starten (sauberer pkill, DB-Lock-Reset).
6. Via `/object_info` API prüfen: alle Loader sehen die neuen Modelle.
7. Auswertung in `analysis/004`.

## Risiken

- **FLUX-VAE-Quelle:** das offizielle BFL-Repo ist gated. Comfy-Org's
  Lumina-Image-2.0-Repackaged liefert einen technisch identischen FLUX-VAE — über
  Datei-Größe (~335 MB) und Lauf-Test verifizieren.
- **insightface** wird bewusst NICHT installiert — auf Apple-Silicon-arm64 zu fragil
  (siehe Memory). Charakter-Konsistenz später über IPAdapter Plus + LoRA-Training.
- Manche Custom Nodes laden langsam beim ersten Start; ggf. zweimal neu starten,
  bis alles initialisiert ist.

## Belege

`results/004-profi-stack/`
- Server-Log-Auszug (Custom-Nodes-Lade-Status)
- `object_info`-Snapshot vor und nach Install
