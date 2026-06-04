# Versuch 5 — Plan: Kurzgesagt-Wow-Short

Gehört zu `specs/005-kurzgesagt-wow-short.md`.

## Story-Beats (75 Sek total)

| # | Beat | Dauer | Hero-SVGs | Übergang zu nächster |
|---|---|---|---|---|
| 1 | Hook: „Was passiert wenn du Enter drückst?" | 0:00-0:13 | menschen/mensch-tippend, flow/pulse-electric | **Camera-Pan-Down** zu Tastatur-Querschnitt |
| 2 | Strom flutet in den PC | 0:13-0:28 | flow/strom-quelle, flow/elektron-stream, hardware/motherboard-topdown | **Wolken-Wipe** (Curtain-Bottom rauf) |
| 3 | CPU + RAM erwachen | 0:28-0:45 | hardware/cpu-detail-cores, hardware/ram-stick-3d, tech-kern/cpu-chip-3d | **Line-Wipe** nach rechts |
| 4 | Daten fließen durch Kernel | 0:45-1:02 | tech-kern/daten-strahl, tech-kern/code-stream, tech-kern/terminal-glas | **Iris-In** auf Pingu |
| 5 | Pingu erklärt's: „und so läuft alles" | 1:02-1:15 | menschen/pingu-cinematic, menschen/pingu-winkend | **Fade to logo** |

## Voiceover-Skript (für ElevenLabs)

```text
Was passiert eigentlich, <break time="0.4s"/> wenn du Enter drückst?

<break time="0.6s"/>
Strom flutet durch dein Netzteil — Milliarden Elektronen pro Sekunde.

<break time="0.5s"/>
Die CPU wacht auf. Der RAM lädt Befehle. <break time="0.3s"/>Alles in Mikrosekunden.

<break time="0.5s"/>
Linux dirigiert das Ganze. Daten fließen durch den Kernel, <break time="0.3s"/>genau dorthin wo sie hin sollen.

<break time="0.6s"/>
Und so <emphasis>läuft</emphasis> alles. <break time="0.4s"/>Jeder Tastendruck. <break time="0.3s"/>Jede Sekunde.
```

Stability 45, Similarity 75, Style 15, Speaker Boost ON, Voice: ELEVENLABS_VOICE_ID_OPA.

## Übergangs-Choreographie (4 Typen)

1. **Camera-Pan-Down** (Beat 1→2): Hero-SVG scrollt nach unten aus dem Viewport,
   gleichzeitig fadet Beat 2 von unten ein. Dauer 0,8s, Easing `cubic-bezier(0.7, 0, 0.3, 1)`.
2. **Wolken-Wipe** (Beat 2→3): `wolken/wolke-curtain-bottom.svg` skaliert von unten
   auf 200% hoch, schluckt komplett, dann Beat 3 fadet drunter ein und Wolke fährt weiter
   nach oben raus. Dauer 1,2s.
3. **Line-Wipe** (Beat 3→4): Vertikale Linie wandert von 0% auf 100% nach rechts. Links
   der Linie altes Bild (clipped), rechts neues Bild kommt rein. Dauer 1,0s.
4. **Iris-In** (Beat 4→5): Kreismaske öffnet sich von Pingu-Position aus radial.
   Dauer 0,9s.

## Audio-Layer

- **Layer 1 — Voiceover** (ElevenLabs MP3, mono, normalized -3 dB)
- **Layer 2 — Ambient-Pad** (synthwave-pad, loop, -18 dB unter Voice)
- **Layer 3 — SFX pro Übergang:**
  - Camera-Pan-Down → whoosh-low
  - Wolken-Wipe → cloud-swoosh
  - Line-Wipe → laser-zip
  - Iris-In → mechanical-click + reveal
- **Layer 4 — Per-Scene-Atmo:** elektrisches Knistern (Beat 1-2), Tasten-Klicks (Beat 4)

## Schritte

1. Spec + Plan (DONE).
2. SVG-Inventur — alle benötigten 25-30 Assets verifiziert vorhanden.
3. HTML/CSS-Komposition aufsetzen (Cinema-Wrapper aus strom-flow-10min als Basis).
4. 5 Szenen-Container mit Hero-SVG-Embedding bauen.
5. 4 Übergangs-CSS-Animationen + JS-Choreographie.
6. ElevenLabs-Skript durch externes Python-Skript jagen → `voiceover.mp3`.
7. SFX von Freesound (CC0) holen → `sfx/*.mp3`.
8. HTML-`<audio>`-Elemente mit Timing-Events koppeln (`requestAnimationFrame`-Sync).
9. Lokal im Browser testen, Reza zeigen, Feedback einarbeiten.
10. Wenn OK: Belege in `results/005-` ablegen, Commit + PR.

## Risiken

- **ElevenLabs-API-Latenz:** 75 Sek Text → ~15 Sek Generierung. Cache MP3 lokal.
- **Audio-Sync-Drift:** HTML5 `<audio>` ist nicht frame-exakt. Akzeptabel für Demo,
  bei finalem MP4-Export via `ffmpeg` neu mixen.
- **ComfyUI-Modelle nicht rechtzeitig:** dann Stretch-Goal (ComfyUI-Atmosphäre-Layer)
  weglassen — Demo läuft trotzdem voll.
- **SVG-Performance:** 5-7 SVGs gleichzeitig im DOM ist ok, mehr in einer Szene =
  reduziert auf 1-2 inline + Rest als `<img>`.

## Belege

`results/005-kurzgesagt-wow-short/`
- `demo.html` (final composition)
- `voiceover.mp3`
- `sfx/*.mp3`
- `screenshots/scene-01..05.png` (für Auswertung)
