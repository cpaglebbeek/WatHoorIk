---
date: 2026-06-06
repo: iCt_Horse
status: done
resume: ""
---

**HISTORISCH** — origineel newp HearHorse onder iCt_Horse. Project verhuisd naar eigen repo WatHoorIk + domein wathoorik.nl op 2026-06-06. Resume-haken zijn overgenomen in `WatHoorIk/prompts/2026-06-06_newp_wathoorik_verhuis.md`.

---


## v0.0.3-Carhart LIVE 2026-06-06 18:45 UTC

Twee features:
1. **Speakers-mode toggle** in wizard-tab: 🎧 hoofdtelefoon (L+R apart, 16 cellen) vs 🔊 speakers (beide samen via key 'B', 8 cellen). Audiogram krijgt groene driehoek-marker voor B-data. Audio: `pan.value = 0` voor beide oren tegelijk.
2. **Stop & bewaar partial**: Stop-knop tekst aangepast naar "■ Stop & bewaar partial". Bij stop: cellen met ≥1 hit krijgen `cs.lastHitLevel` als drempel-schatting + `state.wizard.partial[key]=true`. Cellen met trials maar geen hit krijgen 100% (niet hoorbaar). Audiogram rendert partial-pad als dashed line, tabel toont 'partial'-pill, conclusie meldt aantal partial cellen.

Bestand: 69 KB / 1232 regels JS, parse OK.
JSON-schema bumped naar v3, accepteert v1+v2 op import.

## v0.0.2-Fletcher LIVE 2026-06-06 18:15 UTC

Bundle-release met 3 features tegelijk (1 grote deploy ipv 3 stappen):
- Wizard-test (Feature C): reactietijd-meting met random 1-6 sec delay + 10% catch-trials + Hughson-Westlake-light niveau-adjust + 2-hits-confirm + interleaved random
- Single pitch-shift (Feature A) + Multi-band editor (Feature E): semitonen-slider + user-toegevoegde banden boost/shift
- Auto-comfort (Feature D): wizard-data → comfortPenalty → multi-band profiel

Bestand: `iCt_Horse/HearHorse/index.html` 66 KB / 1177 regels JS — parse OK.
Deploy: rsync icthorse:~/domains/icthorse.nl/public_html/HearHorse/, HTTP 200.

Tab-volgorde gewijzigd: wizard wordt tab 1 (primair), manual wordt tab 2 (expert).
LocalStorage key bumped naar `hearhorse_v2` (v1 import blijft werken).
JSON-schema bumped naar `hearhorse-audiogram-v2`, accepteert v1 op import.


# 2026-06-06 — newp: HearHorse v0.0.1-Békésy

## Prompt
> newp "HearHorse". Sofware voor diagnose en mitigatie bij doofheid/slechthorendheid. leer als basis alles uit de repo noisecanceling. begin met een pagina op icthorse.nl/HearHorse. test die iemand zelf kan afnemen om te bepalen wat hij/zij wel of niet hoort qua tonen/frequentie op dat moment. het resultaat is een uitgebreid rapport. 2de feature: resultaat van test 1 wordt gebruikt om een in te spreken bericht te vervormen naar de frequentie waarin het gehoord werd

## Beslissingen (WhatIf 1 → akkoord)
- **Architectuur:** A — subpagina onder iCt_Horse (zoals `edu/noisecanceling/`), pad `iCt_Horse/HearHorse/`, URL `icthorse.nl/HearHorse/`. Geen eigen repo (kan later promoten naar B).
- **Tech-stack:** vanilla HTML/CSS/JS, **alles client-side**, geen backend. Web Audio API + jsPDF CDN. Geen LabSharing (single-user use-case).
- **Codenaam-thema:** cochlea-/audiometrie-pioniers. v0.0.1-**Békésy** (Nobelprijs 1961). Niet Helmholtz (overlap met noisecanceling).
- **PUBLIC** (erft iCt_Horse-zichtbaarheid).
- **Disclaimer-toon:** licht-medisch "indicatief, geen medisch hulpmiddel".
- **Test-procedure:** ascending method (start zacht, gebruiker zelf omhoog tot drempel). Hughson-Westlake voor latere fase.
- **8 testfrequenties:** 125, 250, 500, 1000, 2000, 4000, 6000, 8000 Hz × L/R = 16 metingen.

## Wat is gebouwd (skelet v0.0.1-Békésy)
- `iCt_Horse/HearHorse/index.html` (~800 regels, single-page-app, 4 tabs)
- `iCt_Horse/HearHorse/README.md`
- Geen backend (`api.php` niet nodig, alles client-side via Web Audio + localStorage)

### Tab 1 — Gehoortest
- Kalibratie 1 kHz referentietoon (OscillatorNode + GainNode @ 0.05)
- Test-grid 2×8 cellen klikbaar
- Per cel: OscillatorNode + StereoPannerNode (-1 L, +1 R) + GainNode
- Level-bar 0-100% met +5/-5 knoppen + keyboard shortcuts (↑↓ Enter Esc)
- Mapping `levelToGain(pct)`: log-curve van -60 dB tot 0 dB (max gain 0.5 voor veiligheid)
- "ik hoor het" → drempel opgeslagen, auto-advance naar volgende undone cel
- LocalStorage key `hearhorse_v1` met `thresholds`-object

### Tab 2 — Audiogram & rapport
- Canvas 900×420 met klinische audiogram-conventie:
  - X log-schaal (100 Hz - 10 kHz)
  - Y lineair (drempel 0-100%, lager boven = beter horen)
  - L = blauwe cirkel ○, R = rood kruis ×
- Tabel met pillen: normaal (≤20%) / licht (≤40%) / matig (≤60%) / ernstig (≤80%) / zwaar (>80%)
- Conclusie-generator: detecteert presbyacusis-patroon (HF-loss), asymmetrie L/R
- PDF-export via jsPDF (CDN lazy-load, zoals noisecanceling v0.0.16-Olson)
- JSON-export schema `hearhorse-audiogram-v1` + import met validatie

### Tab 3 — Spraak-vervorming (compenserende EQ)
- MediaRecorder WebM/Opus, max 30 sec auto-stop
- OfflineAudioContext + 8 BiquadFilterNodes (type 'peaking', Q=1.0) op testfrequenties
- Gain per band = `min(24, avg(L+R drempel%) × 0.24)` dB
- AudioBuffer → WAV PCM-16 encoder (in-line, 30 regels)
- Download als .wav

### Tab 4 — Info
- Wat HearHorse wel/niet is
- Tech-stack + codenaam-context
- Disclaimers herhaald

## Acties (gepland)
- [x] index.html skelet
- [x] README.md
- [x] Sessie-MD met resume-frontmatter (deze file)
- [x] Memory `project_hearhorse.md` + MEMORY.md index-regel
- [x] Lokale browser smoke-test (JS parse OK 592 regels, tags 128/128 gebalanceerd)
- [x] **Deploy naar Hostinger** `icthorse.nl/HearHorse/` — rsync via SSH-alias `icthorse`, 3 bestanden, HTTP 200 LIVE 17:52 UTC
- [ ] Z Fold 6 responsive check (test-grid is 9 kolommen — kan krap zijn op klein scherm)
- [ ] Eventueel Meta_Master STATUS.md / ECOSYSTEMS.md update (komt bij "over en uit")

## v0.0.2-Fletcher techniek-notes
- **Wizard state-machine:** `wiz.active/queue/cellState/currentTrial/toneStartedAt/pressTimer/awaitingAnswer`. Trials gegenereerd on-the-fly, niet vooraf, voor dynamische niveau-adjust per cel.
- **wizardPlayTone:** 1-sec sine met 50ms in/out-envelope (anti-click). Async met `setTimeout` voor non-blocking.
- **Reactietijd:** `performance.now()` - `toneStartedAt`. Geldig tussen 0-3000ms; daarbuiten = grensgeval.
- **Multi-band shift implementatie (modus C):** complex — per shift-band aparte OfflineAudioContext (3 contexts in cascade: bandpass-isolate → playbackRate-shift → mix in finalCtx). Audio mixing op t=0 met gain 0.5 om clipping te vermijden.
- **Auto-comfort algoritme:** `comfortPenalty = drempel/100 + max(0, (RT-800)/1500)`. Threshold 0.35 voor boost-band, +18dB max. Extra `shift -5 semis` op 6kHz als beide HF-banden (6k+8k) ≥14dB boost krijgen.

## Open punten (resume-haak)

### Fase 0 → Fase 1 mogelijke verbeteringen
1. **Kalibratie-verfijning** — nu 5% stappen, optie voor 1% voor fijnere drempel-bepaling
2. **Hughson-Westlake mode** — klinische standaard 10-up/5-down naast huidige ascending
3. **Test-tijd-randomisatie** — toon-onset niet voorspelbaar (anders gokt user op timing)
4. **Maskeer-ruis op niet-getest oor** — bij grote asymmetrie hoort beter oor mee → maskeren met smalle-band ruis
5. **Frequency-shift mitigatie (fase 2)** — voor severe high-freq loss: shift HF-content naar lager spectrum (vocodertechniek)
6. **Audiogram-vergelijking over tijd** — meerdere metingen in localStorage met datum, trend-grafiek
7. **Headphone-database** — bekende kalibratie-offsets voor populaire koptelefoons (Sony WH-1000XM, AirPods Pro, etc.)
8. **TTS-demo** — vooraf bepaald NL-zinnetje speakerlist via SpeechSynthesis door dezelfde EQ → snelle preview zonder opname

### Codenaam-roadmap
- v0.0.1-**Békésy** ✅ (cochlea-mechanica)
- v0.0.2-**Fletcher** — Fletcher-Munson equal-loudness curves toepassen op audiogram-weging
- v0.0.3-**Munson** — masking-ruis op contralaterale oor
- v0.1.0-**Wever** — frequency-shift mitigatie (Wever-Bray, fase-locking)
- v0.2.0-**Stevens** — ervaringspsycho-fysica, perceived loudness re-mapping

## Lessons learned uit noisecanceling-repo
- **jsPDF lazy-load via CDN** is robuust en compact (zelfde patroon als v0.0.16-Olson)
- **CSS-vars per-element-kleur** (v0.0.12-Cornu) overkill voor v0.0.1; later integreren als Configuratie-paneel komt
- **OfflineAudioContext + AudioBuffer→WAV encoder** patroon: noisecanceling doet dit niet, eigen kleine encoder geschreven (PCM-16, 35 regels)
- **StereoPanner per oor** = klinisch correct (geen multi-channel routing nodig)

## Volgende
- Smoke-test in browser
- Deploy
- Eventuele iteraties op basis van eigen test-ervaring (Christian + Joyce)
