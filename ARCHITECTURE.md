# Architecture — Wat Hoor Ik?

## Single-file deployment

Het hele project is **één `index.html`** (~80 KB, 1386 regels JS). Geen build-step, geen bundler, geen framework. Vanilla HTML/CSS/JS.

## Bestanden

```
WatHoorIk/
├── index.html       # Single-page-app (alle code + UI)
├── README.md        # Publieke documentatie
├── CLAUDE.md        # Agent-instructies + deploy-procedure
├── ARCHITECTURE.md  # Dit bestand
├── LICENSE          # AGPL-3.0
└── prompts/         # Sessie-MD's (resume-haken)
```

## Componenten in index.html

### HTML-secties (6 tabs)
1. **Wizard-test** — primaire flow met reactietijd-meting
2. **Referentie** — manual ISO 8253-1 PTA (11 freqs)
3. **Speciaal** — manual research-spectrum (21 freqs, 10Hz-20kHz)
4. **Audiogram & rapport** — visualisatie + PDF/JSON export
5. **Spraak-vervorming** — 4 modi (audiogram-EQ / single-shift / multi-band / auto-comfort)
6. **Info** — uitleg + disclaimers

### Audio-pipeline

```
Wizard:
  user → wizardStart → wizardNextTrial → wizardPlayTone(freq, ear, level)
    → OscillatorNode → GainNode (envelope 50ms in/out) → StereoPannerNode → destination

Manual:
  user clicks cell → startCell → testStartTone (continuous sine, level=10%)
    user adjusts level → testSetLevel (setTargetAtTime smooth)
    user clicks "ik hoor het" → recordHearManual

Speech (offline):
  recorded blob → AudioContext.decodeAudioData → OfflineAudioContext
    → BufferSource → filter-chain (4 modes) → render → WAV encoder → Blob → download
```

### State (localStorage `hearhorse_v3`)

```js
state = {
  thresholds: {},              // manual: "L_1000": 35
  wizard: {
    mode: 'headphones',        // 'headphones' | 'speakers'
    trials: {},                // per cell array of trial-objects
    thresholds: {},            // computed thresholds
    avgRT: {},                 // computed avg reaction time
    partial: {},               // cells with incomplete data
    catchTrials: 0,
    catchPressed: 0,
    options: { flashOn, askConfirm, maxDurationMin }
  },
  manualFreqSet: 'reference',  // 'reference' | 'special'
  speechMode: 'audiogram',
  bands: [...],                // multi-band editor entries
}
```

### Constants

- `FREQS = [125, 250, 500, 1000, 2000, 4000, 6000, 8000]` — wizard 8 freqs
- `REFERENCE_FREQS` — 11 ISO 8253-1 PTA
- `SPECIAL_FREQS` — 21 freqs 10Hz-20kHz
- `ALL_FREQS = union(SPECIAL_FREQS, REFERENCE_FREQS)` — voor audiogram/rapport (union)
- `EARS = ['L', 'R']` — wizard standaard
- `MANUAL_EARS = ['L', 'R', 'B']` — manual 3 rijen

## Wizard state-machine

```
IDLE → START → BUILD_QUEUE
  → loop:
    PICK_RANDOM_CELL → catch-trial?  
      yes → silent trial
      no  → schedule tone after random 1-6 sec delay
    PLAY_TONE → wait for press OR timeout 6 sec
      pressed within 3000ms → HIT → adjust level -10
      timeout + askConfirm yes → HIT (border)
      timeout + askConfirm no or postAnswer no → MISS → adjust level +10
    2 consecutive hits at same level → CONVERGED → threshold = level
    14 trials reached → ABORT → threshold = lastHitLevel || level
  → all cells done → FINISH
```

## Audio safety

- Max gain in `levelToGain`: 0.5 absolute (1.0 zou volledige clipping zijn)
- Mapping `levelToGain(pct)` = `0.5 * 10^((-60 + pct/100*60)/20)` dB → -60 dB @ 0% tot 0 dB @ 100%
- Wizard tone-burst: 1 sec met 50ms in/out envelope (anti-click)

## Relaties met andere projecten

| Project | Relatie |
|---|---|
| **iCt Horse** | Maker/credit. Deploy-account hetzelfde (Hostinger). Geen code-deling. |
| **noisecanceling** (`iCt_Horse/edu/noisecanceling/`) | Patroon-referentie voor toekomstige v0.0.7-Pollack (api.php + signaling) |
| **Meta_WatHoorIk** | Submaster (PRIVATE) — projectbeheer, STATUS, codename-roadmap |

## Toekomst

- **v0.0.7-Pollack:** `api.php` toevoegen (file-based JSON sessions, polling 600ms, sessie-code XXXX-YYY) + nieuwe tab "Verbinding" + tester/kandidaat rolverdeling
- **v0.0.8-Davis:** AudioWorklet voor live pitch-shift (phase-vocoder, geen tempo-verandering)
- **v0.0.9-Wever:** masking-ruis op contralateraal oor bij L/R-asymmetrie
