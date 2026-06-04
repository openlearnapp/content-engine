# Versuch 3 — Plan: Durchführung Lightning + Audio

Gehört zu `specs/003-lightning-schaerfe-mit-audio.md`.

## Setup

- ComfyUI 0.21.1 auf Apple Silicon / MPS
- Neu installiert: AnimateDiff Lightning 8-Step (ByteDance), DreamShaper 8 (Lykon),
  ControlNet Tile (lllyasviel), 4×-UltraSharp (uwg), Motion-LoRA zoom-in (guoyww)
- Workflow: `workflows/04-text-zu-video-lightning.json`
- Audio: ElevenLabs-API (Voice „OPA"), Schlüssel aus lokaler `.env.local`

## Schritte

1. `workflows/04-text-zu-video-lightning.json` anlegen — DreamShaper + Lightning +
   Context-Window, 32 Frames @ 768×432, 8 Steps, CFG 1,8, Sampler euler /
   Scheduler sgm_uniform, beta_schedule `lcm`.
2. Runner-Skript:
   - Narration-Text → ElevenLabs-API (Voice OPA, `eleven_multilingual_v2`) → `audio.mp3`
   - Workflow per `POST /prompt` an ComfyUI → Clip in `~/ComfyUI/output/`
   - `ffmpeg -i video.mp4 -i audio.mp3 -c:v copy -c:a aac out.mp4`
3. Beleg-Dateien (Video, Audio, Final) nach `results/003-lightning-narrated/` kopieren.
4. Auswertung in `analysis/003-lightning-narrated.md`:
   was probiert / was geklappt / was nicht / Vergleich gegen Versuch 1.

## Risiken

- Erste Kombi aus Lightning + Context-Window auf MPS — kann beim ersten Lauf zicken
  (Anomalien, OOM), dann Auflösung oder Frame-Zahl reduzieren.
- ElevenLabs-API-Antwort variiert in Länge; ggf. Narration verkürzen, damit Video-
  und Audio-Länge zusammenpassen.
- DreamShaper ist creativeml-openrail-m lizenziert — nur intern verwenden.
