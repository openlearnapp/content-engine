# Versuch 4 — Spec: Profi-Stack-Foundation

Datum: 2026-06-03

## Frage

Bisher wurde ComfyUI auf Einstiegs-Niveau benutzt: ein Standard-Modell, ein KSampler,
fertig. Welche Profi-Werkzeuge fehlen tatsächlich, um aus der Engine das herauszuholen,
was erfahrene Creator damit machen — und lassen sich alle Bausteine auf 24 GB Mac MPS
installieren und nutzen?

## Hintergrund

Recherche im Juni 2026 hat einen klaren Profi-Stack identifiziert, der die bisherige
Setup-Lücke schließt:

- **Hyper-SD** (ByteDance) als Distillation — schlägt das bislang genutzte Lightning bei
  Qualität pro Step.
- **ToonYou** als SD-1.5-Cel-Look-Modell — von allen Basis-Modellen am nächsten an
  flacher Vektor-Illustration.
- **DoctorDiffusion Vector-Art-XL LoRA** — engster Vektor-Look-Approximator auf
  HuggingFace mit klarer Lizenz (CC0-Training).
- **FLUX Schnell GGUF Q4** — quantisiertes Flux-Modell, auf 24 GB unified memory lauffähig
  über den city96/ComfyUI-GGUF-Loader.
- **rgthree-comfy, Inspire-Pack, Impact-Pack (+Subpack), Crystools, GGUF-Loader,
  efficiency-nodes** — die Custom-Node-Pakete, die Profis als Standard fahren.

Alternative DMD2 wurde geprüft und verworfen: Hyper-SD wird community-weit bevorzugt
(siehe sandner.art-Vergleich, juni 2026).

## Erfolgskriterien

1. Alle 10 Modelle und 7 Custom-Node-Pakete ohne Fehler installiert.
2. ComfyUI-Server startet sauber neu, keine Import-Errors in den neuen Paketen.
3. Alle neuen Modelle in den jeweiligen Loader-Dropdowns sichtbar (`/object_info`-API).
4. Python-Abhängigkeiten ohne Konflikte (Mac-Fix: `onnxruntime` statt `-gpu`).

## Umfang

Installation und Smoke-Test. **Kein Render** — der eigentliche Qualitäts-Vergleich mit
dem neuen Stack ist Inhalt von Versuch 5 (SVG-getriebener Kurzgesagt-Test).

## Nicht Teil dieses Versuchs

- SDXL-spezifische ControlNet-Modelle (Depth, Canny, Lineart) — folgen in Versuch 5
- LoRA-Training für Maskottchen-Konsistenz — eigener Versuch
- Cloud-Anbindung an fal.ai für Hero-Shots — eigener Versuch
