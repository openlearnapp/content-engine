# Versuch 5 — Auswertung: Kurzgesagt-Wow-Short (Erstwurf)

Gehoert zu `specs/005` und `plans/005`. Datum: 2026-06-04.

## Frage

Laesst sich aus dem vorhandenen Stack ein 30-Sek-Short produzieren, der die
Kurzgesagt-Disziplin erreicht — gemessen an Pacing, Uebergaengen, Voiceover und
Cinema-Look?

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

- **Inhaltsbild gegen Kurzgesagt:** Reza-Urteil „Video koennte besser werden, noch
  besser erklaert werden". SVG-Assets aus der Cinematic-Bibliothek sind solide,
  aber an entscheidenden Hero-Momenten fehlt der „kurzgesagt-typische" Charakter-
  Punch. Charakter-Konsistenz ist nicht gesichert (kein IPAdapter im Spiel).
- **Kein SFX-Layer:** Whooshes/Klicks/Ambient-Pad pro Uebergang fehlen — pures
  Voiceover ohne Sound-Design.
- **Profi-Stack ungenutzt:** Versuch 4 hat Hyper-SD/ToonYou/FLUX/ControlNet-Union
  bereitgestellt — keiner davon wurde in diesem Erstwurf produktiv eingesetzt.
- **Keine ComfyUI-Hero-Frames:** SVGs allein sind 80% des Wegs, aber 1-2 KI-
  generierte Atmosphaere-Frames (z. B. ein Pingu-Hero) wuerden den Wow-Punkt
  setzen, an dem Kurzgesagt seine Charaktere zelebriert.

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

Der Erstwurf beweist NICHT: dass dies *Kurzgesagt-Niveau* ist. Dafuer muss der
Profi-Stack aus Versuch 4 jetzt produktiv eingesetzt werden — speziell:

1. **SVG-zu-KI-Pipeline** (Versuch 6): Lineart-ControlNet auf Cinematic-SVG plus
   ToonYou-Kolorierung — Hero-Frames die wie Kurzgesagt-Frames wirken statt wie
   referenzierte SVG-Embeddings.
2. **Charakter-Konsistenz** (Versuch 7): IPAdapter-Plus + Pingu-Referenz-Bild
   ueber alle Szenen, damit Pingu *derselbe* Pingu bleibt — nicht ein anderes
   SVG-Asset pro Szene.
3. **SFX-Layer**: Freesound-CC0 Whooshes, Klicks, Ambient-Pad — pro Uebergang
   gemixt.
4. **Kurzgesagt-Leitfaden anwenden**: siehe `reference_kurzgesagt_leitfaden.md` in
   privater Memory — Pacing-Regeln, Komposition-Hierarchie, Charakter-Patterns.

## Belege

`results/005-kurzgesagt-wow-short/`
- `demo.html` — vollstaendige Komposition
- `voiceover.mp3` — ElevenLabs-Erstgenerierung

## Offene Punkte (Naechste-Session-Agenda)

- Hero-Frames in ComfyUI rendern (SVG → ControlNet-Union-SDXL → ToonYou + Hyper-
  SDXL + DD-Vector), 5 Stueck, je pro Beat. PNG in `results/005-` ablegen,
  Demo-HTML einbinden statt SVG.
- Pingu durchgaengig konsistent: IPAdapter mit `pingu-cinematic.svg` als Referenz.
- SFX-Layer 4 Spuren: Whoosh-Low (Pan), Cloud-Swoosh (Wolken), Laser-Zip (Line),
  Mechanical-Click + Reveal (Iris). Plus Ambient-Pad fuer ganzes Stueck.
- Kurzgesagt-Leitfaden anwenden: Komposition-Hierarchie pro Beat, Charakter-
  Silhouetten-Test (lesbar bei Thumbnail-Groesse), Color-Discipline pro Szene.
