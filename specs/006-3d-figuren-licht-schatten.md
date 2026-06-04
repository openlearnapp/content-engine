# Versuch 6 — Spec: 3D-Figuren mit Licht und Schatten

Datum: 2026-06-04

## Frage

Wie hebt sich unser Workshop-Short auf Top-Tier-Edu-Niveau, indem wir aus
flachen SVG-Embeddings echte plastische 3D-Figuren mit Licht, Schatten und
Materialtiefe machen — und alle Versuche im einheitlichen HTML-Browser-Demo-
Format zugänglich machen?

## Hintergrund

Versuch 5 (Premium-Edu-Wow-Short) lieferte funktionierende Pacing-, Übergangs-
und Voiceover-Choreographie, aber:

- Figuren wirken flach (SVG-Embeddings ohne Tiefe)
- keine echte Schattierung, kein Volumen, keine Materialwahrnehmung
- Charakter wechselt zwischen Szenen (verschiedene SVG-Assets)
- Profi-Stack aus Versuch 4 (Hyper-SD, ToonYou, ControlNet-Union, IPAdapter)
  blieb ungenutzt
- Format-Inkonsistenz: V1-V3 zeigen MP4s im Pages-Index, V5 ist standalone HTML

Reza-Feedback (2026-06-04): „echte bessere Komponenten mit mehr 3D-Effekt für
Figuren und Elemente, mehr mit Licht und Schattierung auch die Echtheit mancher
Elemente deutlich darstellen anstatt nur einfache Elemente".

## Erfolgskriterien

1. **5 plastische Hero-Frames pro Story-Beat**, in ComfyUI gerendert:
   - SDXL + Hyper-SDXL-LoRA (8 Steps) + DD-Vector-LoRA (Style)
   - ControlNet-Union-SDXL mit Lineart-Maske aus Cinematic-SVG (Komposition-Lock)
   - IPAdapter-Plus-SDXL mit `pingu-cinematic.svg` als Referenz (Konsistenz)
   - Hires-Fix-Pass (1024→1920, Tile-ControlNet, 0,4 Denoise)
   - FaceDetailer-Pass (Pingu-Gesicht scharfstellen)
   - Sichtbare plastische Eigenschaften: Rim-Light, Drop-Shadow, Material-Gradienten,
     Volumenkurven, Tiefenstaffelung
2. **Charakter-Konsistenz:** Pingu erkennbar derselbe in allen 5 Frames (Schnabel-
   Form, Augen-Position, Schal-Farbe, Körper-Silhouette).
3. **Demo-Update:** `demo.html` aus V5 referenziert die neuen PNG-Hero-Frames
   statt SVG-Embeddings — eine Code-Zeile pro Beat geändert.
4. **SFX-Layer aus V5 nachgeholt:** 4 Übergangs-SFX (Whoosh/Cloud/Laser/Click) +
   Ambient-Pad. ffmpeg-Mux mit Voiceover.
5. **Format-Konsistenz:** Pages-`index.html` aktualisiert — Karten verlinken auf
   demo.html pro Versuch (Embedded-iframe oder eigene Seite), nicht mehr nur
   eingebettete MP4-Cards.

## Umfang

- ComfyUI-Workflow `05-svg-zu-illustration-plastisch.json` als wiederverwendbare
  Vorlage in `workflows/`
- 5 Renders in `results/006-3d-figuren-licht-schatten/heros/`
- Aktualisierte `results/006-3d-figuren-licht-schatten/demo.html` (mit
  Verweis auf die neuen Hero-PNGs + SFX)
- ElevenLabs-Voiceover bleibt aus V5 wiederverwendbar
- SFX-Spuren als CC0-Downloads in `results/006-.../sfx/`
- Pages-`index.html` mit V4-Card + V5-Card + V6-Card erweitert

## Nicht Teil dieses Versuchs

- LoRA-Training auf Pingu (eigenständige Maschinen-Last, eigener Versuch)
- Mehrsprachige Voiceover-Varianten
- AnimateDiff-Lightning pro Szene (wenn die Hero-Frames stark genug sind, brauchen
  wir es nicht — Idle-Motion via CSS reicht)
- Workshop-YAML-Integration

## Erfolgsmessung

- **Visuell:** Hero-Frame im Vergleich zur SVG-Version klar tiefer, plastischer,
  greifbarer. Drop-Shadow + Rim-Light deutlich sichtbar.
- **Konsistenz:** 5 Frames hintereinander zeigen denselben Pingu — Schnabel-Form
  identisch, Schal in derselben Farbe, Augen-Position passend.
- **Format:** Pages-`index.html` zeigt einheitliche Versuch-Cards mit Klick-Link
  auf jeweilige Browser-Demo.
- **Reza-Test:** „Top-Tier-Niveau erreicht oder übertroffen?" — JA oder ein
  konkreter Fix-Punkt.

## Risiken

- **Render-Zeit:** SDXL + ControlNet-Union + IPAdapter + Hires-Fix + FaceDetailer
  auf 24 GB Mac MPS → erwartet 4-6 Min pro Hero-Frame. 5 Frames → 25-30 Min.
  Falls >40 Min: fal.ai-Cloud-Option als Fallback.
- **IPAdapter-Konsistenz auf SVG-Lineart-Input:** muss verifiziert werden — IPAdapter
  ist primär für Foto-Referenzen trainiert, nicht für Vektor-Lineart. Plan-B:
  Standbild aus `pingu-cinematic.svg` einmalig als PNG rendern, dann als Foto-
  Referenz nutzen.
- **Pages-Format-Umstellung:** existierende V1-V3-Cards mit inline-MP4s müssten
  überdacht werden. Konservativer Ansatz: V4+V5+V6 als demo.html-Links, V1-V3
  unverändert.
