# Versuch 7 — Spec: Hybrid-Wow-Video (LTX-Video + 3D-Figur)

Datum: 2026-06-17

## Frage

Lässt sich auf der vorhandenen Hardware (Mac, 24 GB, MPS) ein kurzes, vertontes
„Wow"-Video im Kurzgesagt-Geist produzieren — prachtvoll, mehrszenig, mit
konsistenter Figur — **vollständig lokal**, ohne Cloud?

Ausgangspunkt war der Wunsch, dafür **Wan 2.2** als Bewegungs-Engine zu nutzen.
Diese Spec prüft beides: ob Wan 2.2 lokal taugt, und — falls nicht — ob der
bestehende Stack ein gleichwertiges Ergebnis liefert.

## Erfolgskriterien

1. Wan 2.2 5B lokal verifizieren (läuft / läuft nicht, mit Begründung).
2. Mindestens ein **echtes, herunterladbares MP4** als Ergebnis im Repo
   (nicht nur HTML-Demo) — als „view raw" zugänglich.
3. Mehrszeniges Stück (≥ 4 Szenen) mit bewegten Bildern + ElevenLabs-Voiceover,
   synchron geschnitten.
4. Eine **konsistente Figur** mit Tiefe/Licht (nicht flach), wiederverwendbar.
5. Reproduzierbare, benannte Workflows in `workflows/`.

## Umfang

- Video-Engine evaluieren: Wan 2.2 5B (Ziel) vs. LTX-Video 2B (Fallback).
- Figur: SVG-Bibliothek → ControlNet → SDXL (3D-Look, Charakter-Konsistenz).
- Vertonung: ElevenLabs (OPA-Stimme), pro Satz, Schnitt synchron.
- Montage: ffmpeg (Zeitlupe, Ein-/Ausblendungen, Vignette).

## Nicht Teil dieses Versuchs

- Cloud-GPU-Setup (separat recherchiert, siehe analysis — als Option für Wan/lange Videos).
- Musik/SFX-Ebene und On-Screen-Text (für V2 vorgemerkt).
- Workshop-YAML-Integration.
