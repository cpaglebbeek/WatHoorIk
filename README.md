# Wat Hoor Ik?

**Self-administered gehoortest + compenserende spraak-vervorming.**

🌐 **LIVE:** https://wathoorik.nl/
📦 **GitHub:** https://github.com/cpaglebbeek/WatHoorIk
🏷️ **Versie:** v0.0.6-Bunch
📜 **Licentie:** AGPL-3.0

## Wat doet het?

### 1. Gehoortest (wizard)
- Random delay 1-6 sec voor elke toon
- Spatiebalk/knop drukken zodra je toon hoort → reactietijd gemeten
- 10% controle-trials zonder toon → betrouwbaarheidsrate
- Hughson-Westlake-light niveau-adjust (10% omlaag bij hit, 10% omhoog bij miss, 2× hit op zelfde niveau = drempel)
- Hoofdtelefoon (L en R apart, 16 cellen ~10 min) of speakers (beide samen, 8 cellen ~5 min)

### 2. Manual modes (expert)
- **Referentie:** ISO 8253-1 / ANSI S3.21 standaard PTA — 11 frequenties (125-8000 Hz octaaf + halve octaven)
- **Speciaal:** Volledig spectrum 10 Hz – 20 kHz (21 frequenties), incl. sub-bas 10-120 Hz en extended HF tot 20 kHz
- 3 rijen: Linker / Rechter / Beide oren
- 1%-precisie via slider + getal-input + ±-knoppen + presets (laag/middel/hoog/maximaal)
- Toon aan/uit toggle

### 3. Audiogram & rapport
- Klinische conventie (log-X freq, lineair-Y drempel)
- Wizard-markers: ○ links / × rechts / ▲ beide
- Manual-markers: ◇
- Partial-data (test gestopt voor convergentie): stippellijn
- RT-grafiek per frequentie
- PDF-export via jsPDF
- JSON i/o backward-compat (v1/v2/v3 schema)

### 4. Spraak-vervorming (4 modi)
| Modus | Werking |
|---|---|
| **Audiogram-EQ** | 8 BiquadFilter peakings op testfrequenties, gain proportioneel aan drempel (+24 dB cap) |
| **Single pitch-shift** | Semitonen-slider ±12 via `OfflineAudioContext.playbackRate` (tempo verandert mee) |
| **Multi-band editor** | Zelf banden toevoegen: boost (peaking dB) of shift (semitonen in bandpass) |
| **Auto-comfort** | Genereert multi-band profiel uit wizard-data: comfortPenalty = drempel + RT-penalty, boost slechte banden + extra omlaag-shift bij ernstige HF-loss |

## Disclaimer

> **Geen medisch hulpmiddel.** Wat Hoor Ik? is een indicatief, educatief instrument. Resultaten zijn afhankelijk van hoofdtelefoon/speakers, omgevingsgeluid en systeem-volume. Voor een klinische diagnose: raadpleeg een audicien of KNO-arts.

## Privacy

- **100% client-side** — geen backend, geen tracking, geen cookies
- LocalStorage key `hearhorse_v3` (historisch behouden voor backward-compat met eerdere versies onder icthorse.nl/HearHorse/)
- Audio-opnames blijven in je browser, niets gaat naar een server

## Tech-stack

- Vanilla HTML/CSS/JS, geen framework
- Web Audio API: `OscillatorNode`, `StereoPannerNode`, `BiquadFilterNode`, `OfflineAudioContext`, `MediaRecorder`
- jsPDF via CDN lazy-load
- Single-file deployment (~80 KB)

## Codenaam-thema (release-tracker)

Cochlea-/audiometrie-pioniers:
- v0.0.1-Békésy — Georg von Békésy (Nobelprijs 1961, cochlea-mechanica)
- v0.0.2-Fletcher — Harvey Fletcher (equal-loudness, multi-band psychoacoustics)
- v0.0.3-Carhart — Raymond Carhart (vader klinische audiologie, binauraal-summation)
- v0.0.4-Stevens — Stanley Smith Stevens (psychofysica, magnitude estimation)
- v0.0.5-Munson — Wilbert Munson (Fletcher-Munson + binauraal hearing)
- **v0.0.6-Bunch** — Cyril G. Bunch (eerste klinische audiogram 1923, PTA-conventies)
- v0.0.7-Pollack (gepland) — live 2-weg verbinding met tester/kandidaat rolverdeling
- v0.0.8-Davis — live AudioWorklet pitch-shift

## Maker

[iCt Horse](https://icthorse.nl/) — Wij verbinden mensen en systemen zodat technologie voor mensen werkt.

## Licentie

[AGPL-3.0](LICENSE)
