---
date: 2026-06-06
repo: WatHoorIk
status: open
resume: "verder met WatHoorIk — v0.0.7-Pollack: live 2-weg verbinding met tester/kandidaat rolverdeling (kandidaat krijgt alleen 'Ik hoor het'-knop, flits+bevestiging-modal voor kandidaat geforceerd UIT, tester ziet alle UI realtime). Backend api.php in WatHoorIk/, hergebruik noisecanceling-patroon, STUN-only optioneel."
---

# 2026-06-06 — newp WatHoorIk (verhuis van HearHorse onder iCt_Horse)

## Trigger
> ik heb een nieuw domain met website op hostinger. wathoorik.nl. verhuis deze inhoud daar naartoe en onthoud als nieuwe placeholder

## Beslissingen (WhatIf akkoord)
- **Architectuur A:** nieuwe PUBLIC repo `cpaglebbeek/WatHoorIk` (AGPL-3.0) + nieuwe PRIVATE submaster `Meta_WatHoorIk`
- **Branding:** publiek "Wat Hoor Ik?" (title/H1/footer/info), intern HearHorse-codenamen-tracker behouden (cochlea-/audiometrie-pioniers)
- **Oude URL `icthorse.nl/HearHorse/`:** 301 redirect naar wathoorik.nl via .htaccess
- **Memory:** rename `project_hearhorse.md` → `project_wathoorik.md`, MEMORY.md index updated
- **v0.0.7-Pollack:** uitstellen naar verse sessie (context-budget)

## Acties uitgevoerd
- [x] `gh repo create cpaglebbeek/WatHoorIk --public --license agpl-3.0`
- [x] Clone naar `~/Documents/Gemini_Projects/WatHoorIk`
- [x] Kopieer `iCt_Horse/HearHorse/*` → WatHoorIk/
- [x] Rebrand index.html (title, h1, sub, footer, info-tab heading + Bron)
- [x] Nieuwe README.md met "Wat Hoor Ik?" branding + GitHub-link + AGPL-3.0
- [x] CLAUDE.md (deploy-procedure, codenamen-thema)
- [x] ARCHITECTURE.md (single-file deployment, state-machine, componenten)
- [x] Sessie-MD (deze file)
- [ ] Rsync naar `wathoorik.nl/public_html/`
- [ ] Verify HTTPS 200 op wathoorik.nl
- [ ] `gh repo create cpaglebbeek/Meta_WatHoorIk --private`
- [ ] Meta_WatHoorIk basics
- [ ] .htaccess redirect bij icthorse.nl/HearHorse/
- [ ] Verwijder `iCt_Horse/HearHorse/` + commit
- [ ] Memory rename + MEMORY.md update
- [ ] Meta_Master PROJECTS.json + ECOSYSTEMS.md + STATUS.md
- [ ] Final OEU?

## Open punten (resume-haak)
- **v0.0.7-Pollack:** live 2-weg verbinding met tester/kandidaat rolverdeling
  - Backend `api.php` in WatHoorIk/, gebaseerd op noisecanceling-patroon
  - Sessie-code XXXX-YYY (alfabet zonder 0/O/1/I/L)
  - Nieuwe tab "Verbinding" met aanmaken/aansluiten + role-keuze
  - Tester ziet alle UI realtime, kandidaat alleen grote "Ik hoor het"-knop
  - Forceer voor kandidaat: `flashOn=false`, `askConfirm=false`
  - HTTP polling 600ms (geen WebRTC nodig voor remote test, kan optioneel later)
  - Latency-disclaimer: reactietijden inclusief netwerk-overhead
- v0.0.8-Davis: live AudioWorklet pitch-shift (phase-vocoder)
- v0.0.9-Wever: masking-ruis contralateraal bij L/R-asymmetrie

## Migratie-impact
- LocalStorage key blijft `hearhorse_v3` (backward-compat met bestaande users die test al gedaan hebben op icthorse.nl/HearHorse/)
- JSON-schema blijft `hearhorse-audiogram-v3`
- 301 redirect zorgt dat bestaande links blijven werken
