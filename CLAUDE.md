# CLAUDE.md — WatHoorIk

Publiek consumer-domein voor de "Wat Hoor Ik?"-app (zelftest gehoor + spraak-vervorming).

## Project

- **Werknaam:** WatHoorIk (intern, repo-naam, code-tracker)
- **Publiekstitel:** "Wat Hoor Ik?" (title, H1, README)
- **Versie-tracker:** HearHorse-codenamen (cochlea-/audiometrie-pioniers) — historisch consistent
- **URL:** https://wathoorik.nl/
- **GitHub:** cpaglebbeek/WatHoorIk (PUBLIC, AGPL-3.0)
- **Ecosysteem:** Meta_WatHoorIk (PRIVATE submaster)

## Regels

- Auto git commit + push na elke wijziging
- WhatIf-protocol vóór elke actie (Plan → Impact → Akkoord)
- Versiebeheer via codenamen-thema:
  - Cochlea-/audiometrie-pioniers: Békésy / Fletcher / Carhart / Stevens / Munson / Bunch / Pollack / Davis / Wever / Hood / Hallpike
  - Volgende: v0.0.7-Pollack (live verbinding met tester/kandidaat)
- Color-coded feature/bugfix protocol:
  - Groen +0.0.1 = minor code-fix zonder architectuur-impact
  - Oranje +0.1.0 = design-impact functioneel/technisch
  - Rood +1.0.0 = redesign / breaking change

## Deploy

**Host:** Hostinger (zelfde account als icthorse.nl)
**SSH-alias:** `icthorse` (IdentityFile `~/.ssh/mindbodynjoy_hostinger`, user `u753337840`)
**Doelpad:** `~/domains/wathoorik.nl/public_html/`

```bash
rsync -avz -e "ssh" \
  /Users/christian/Documents/Gemini_Projects/WatHoorIk/index.html \
  /Users/christian/Documents/Gemini_Projects/WatHoorIk/README.md \
  icthorse:~/domains/wathoorik.nl/public_html/
```

(Niet `prompts/`, `CLAUDE.md`, `ARCHITECTURE.md`, `LICENSE` mee — alleen runtime-bestanden.)

## Privacy

- 100% client-side — geen backend
- LocalStorage key `hearhorse_v3` (historisch behouden voor backward-compat met eerdere versies onder icthorse.nl/HearHorse/)
- Bij toekomstige v0.0.7-Pollack: er komt wel een `api.php` voor live verbinding (sessiecode + signaling), opt-in via specifieke tab

## "Over en uit"

Conform globale OEU-protocol:
1. Commit + push WatHoorIk
2. Commit + push Meta_WatHoorIk (status-update)
3. Meta_Master: STATUS.md update + RESUME.md regenereren
4. Memory sync + Gemini/Codex sync
