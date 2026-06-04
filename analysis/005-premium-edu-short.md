# Versuch 5 — Auswertung: Premium-Edu-Wow-Short (Erstwurf)

Gehoert zu `specs/005` und `plans/005`. Datum: 2026-06-04.

## Frage

Laesst sich aus dem vorhandenen Stack ein 30-Sek-Short produzieren, der das
Top-Tier-Niveau professioneller Edu-Animationen erreicht — gemessen an Pacing,
Uebergaengen, Voiceover und Cinema-Look?

## Was probiert

Erster vollstaendiger End-to-End-Lauf ohne neue Modell-Render:

- 5 Story-Beats aus der `strom-flow-10min`-Lektion auf 30 Sek kondensiert.
- HTML/CSS/JS-Komposition (self-contained, kein Build-Tool), eingebettete SVGs aus
  der Cinematic-Bibliothek (19 verschiedene Assets).
- ElevenLabs-Opa-Voiceover (26,8 Sek, Stability 45, Similarity 75, Style 15,
  Speaker Boost) — direkt von Python aus der API geholt und als MP3 in `results/`.
- 4 unterschiedliche Uebergangs-Typen: Camera-Pan-Down, Wolken-Wipe (Curtain-SVG
  faehrt durchs Bild), Line-Wipe (cyan/magenta-Leuchtlinie mit Clip-Path),
  Iris-In (Kreis-Clip schliesst/oeffnet).
- Cinema-Look: Letterbox + Vignette + animierter Grain + Cyan-Lens-Flare +
  Dolly-In je Szene + 3-Ebenen-Parallax + 40 farbige floatende Partikel.

## Was geklappt hat

- **Voiceover-Qualitaet:** Reza-Urteil „sehr gut, reizend, spannend, entspannt".
  ElevenLabs-Settings sitzen.
- **Asset-Pipeline:** Alle 19 SVG-Pfade aufloesen ueber HTTP-Server vom Repo-Root.
  Keine Build-Step noetig.
- **Uebergangs-Choreographie:** Alle 4 Typen funktionieren, sind visuell klar
  unterscheidbar, sitzen technisch sauber.
- **Schnelligkeit der Iteration:** Spec → Plan → Demo + Audio in unter einer Stunde.

## Was nicht ausreicht

- **Inhaltsbild gegen Top-Tier-Niveau:** Reza-Urteil „Video koennte besser werden,
  noch besser erklaert werden". SVG-Assets aus der Cinematic-Bibliothek sind solide,
  aber an entscheidenden Hero-Momenten fehlt der Charakter-Punch. Figuren wirken
  flach — ohne 3D-Effekt, ohne Licht/Schatten, ohne echte Tiefen-Schattierung.
- **Kein SFX-Layer:** Whooshes/Klicks/Ambient-Pad pro Uebergang fehlen — pures
  Voiceover ohne Sound-Design.
- **Profi-Stack ungenutzt:** Versuch 4 hat Hyper-SD/ToonYou/FLUX/ControlNet-Union
  bereitgestellt — keiner davon wurde in diesem Erstwurf produktiv eingesetzt.
- **Keine ComfyUI-Hero-Frames:** SVGs allein sind 80% des Wegs, aber 1-2 KI-
  generierte 3D-Atmosphaere-Frames (mit echtem Licht/Schatten) wuerden den Wow-
  Punkt setzen, an dem die Figuren wirklich „greifbar" werden.

## Render-/Build-Daten

| Asset | Groesse | Quelle |
|---|---|---|
| `demo.html` | ~24 KB | self-contained, kein Build |
| `voiceover.mp3` | 430 KB | ElevenLabs API, 26,8 Sek |
| 19 SVGs (referenziert) | aus `lernvideo-asset-system/asset-bibliothek/cinematic/` | nur via Pfad |
| Lokaler Server | `python3 -m http.server 8765` vom Repo-Root | Port 8765 |

## Erkenntnis

Der Erstwurf beweist: **die Komposition funktioniert** — Stimme + Choreographie
+ Cinema-Look sind technisch erreichbar in unter einer Stunde.

Der Erstwurf beweist NICHT: dass dies *Top-Tier-Niveau* ist. Dafuer muss der
Profi-Stack aus Versuch 4 jetzt produktiv eingesetzt werden — speziell:

1. **3D-Figuren mit Licht/Schatten** (Versuch 6): Charaktere bekommen echtes
   Volumen durch Rim-Light, Drop-Shadows, Gradienten-Tiefe, plastische Materialien.
   Statt flacher SVGs: ToonYou + ControlNet-Union + Hires-Fix-Pipeline rendert
   Hero-Frames mit Tiefe.
2. **Charakter-Konsistenz**: IPAdapter-Plus + Pingu-Referenz-Bild ueber alle
   Szenen, damit Pingu *derselbe* Pingu bleibt — nicht ein anderes SVG-Asset
   pro Szene.
3. **SFX-Layer**: Freesound-CC0 Whooshes, Klicks, Ambient-Pad — pro Uebergang
   gemixt.
4. **Output-Format-Standard**: alle Versuche als HTML-Browser-Demo zugaenglich,
   nicht als MP4-Download. Konsistenz ueber alle Versuche.

## Belege

`results/005-premium-edu-short/`
- `demo.html` — vollstaendige Komposition
- `voiceover.mp3` — ElevenLabs-Erstgenerierung

## Offene Punkte (Naechste-Session-Agenda)

- Hero-Frames in ComfyUI rendern (SVG → ControlNet-Union-SDXL → ToonYou + Hyper-
  SDXL + DD-Vector), 5 Stueck, je pro Beat. PNG in `results/005-` ablegen,
  Demo-HTML einbinden statt SVG.
- Pingu durchgaengig konsistent: IPAdapter mit `pingu-cinematic.svg` als Referenz.
- SFX-Layer 4 Spuren: Whoosh-Low (Pan), Cloud-Swoosh (Wolken), Laser-Zip (Line),
  Mechanical-Click + Reveal (Iris). Plus Ambient-Pad fuer ganzes Stueck.
- Premium-Edu-Leitfaden anwenden: 2.5D-Parallax-Tiefe statt flacher Embeddings,
  Charakter-Silhouetten-Test bei Thumbnail-Groesse, Color-Discipline pro Szene
  (3 Farben max).
