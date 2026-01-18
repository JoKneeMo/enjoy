# 🔧 HACKER'S GUIDE

> Per chi vuole capire COME funziona, non solo COSA fa.

```
    ╔═══════════════════════════════════════════════════════════════╗
    ║  "The best way to understand something is to break it."      ║
    ║  "The best way to fix it is to build it together."           ║
    ╚═══════════════════════════════════════════════════════════════╝
```

---

## 🧠 Architettura Mentale

```
┌─────────────────────────────────────────────────────────────────┐
│                        ENJOY ARCHITECTURE                        │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│   ┌─────────┐    PR    ┌─────────────┐    ┌──────────────┐     │
│   │  Human  │ ───────▶ │  Validator  │ ──▶│  state.json  │     │
│   │ (You!)  │          │  Workflow   │    │  (The Brain) │     │
│   └─────────┘          └─────────────┘    └──────────────┘     │
│        │                     │                   │               │
│        │                     ▼                   ▼               │
│        │            ┌─────────────┐      ┌─────────────┐        │
│        │            │   Labels    │      │   Karma     │        │
│        │            │ auto-merge  │      │   Engine    │        │
│        │            │   invalid   │      │  (+1/-1/×2) │        │
│        │            └─────────────┘      └─────────────┘        │
│        │                                        │               │
│        │              ┌─────────────────────────┘               │
│        │              ▼                                         │
│        │       ┌─────────────┐                                  │
│        └──────▶│  Heartbeat  │◀──── cron: 6 hours              │
│                │  Workflow   │                                  │
│                │  (Pulse)    │                                  │
│                └─────────────┘                                  │
│                      │                                          │
│                      ▼                                          │
│            ┌─────────────────┐                                  │
│            │  Generated Art  │                                  │
│            │  SVG/MD/Stats   │                                  │
│            └─────────────────┘                                  │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

---

## 🔍 I File Che Contano

### `state.json` - Il Cervello
```json
{
  "level": 1,              // Livello attuale del gioco
  "phase": "foundation",   // Fase corrente (1-5)
  "totalKarma": 0,         // Karma globale accumulato
  "totalPRs": 0,           // Numero totale di PR
  "players": {},           // Mappa player -> stats
  "board": [],             // Contenuti del gioco
  "achievements": [],      // Achievement sbloccati
  "lastUpdate": "..."      // Timestamp
}
```

**⚠️ IMPORTANTE:** `state.json` ha un **lock di concorrenza**.
Solo un workflow alla volta può modificarlo.

### `levels.json` - Le Regole
```json
{
  "1": {
    "name": "Genesis",
    "phase": "foundation",
    "unlockKarma": 0,
    "rules": {
      "allowedExtensions": [".txt"],
      "maxFileSize": 100,
      "contentPattern": "^[a-zA-Z]{2,20}$"
    }
  }
  // ... 100 livelli
}
```

### `engine/` - Il Motore TypeScript
```
engine/
├── src/
│   ├── validator.ts      # Valida i PR
│   ├── karma-engine.ts   # Calcola karma
│   ├── time-system.ts    # Multiplier temporali
│   ├── board-manager.ts  # Gestisce il board
│   └── level-engine.ts   # Progressione livelli
├── dist/                 # Build compilato
└── package.json
```

---

## 🎯 Entry Points per Contribuire

### 🟢 FACILE - Primi Passi

| Cosa | File | Descrizione |
|------|------|-------------|
| Nuovo livello | `levels.json` | Aggiungi livello 101+ |
| Nuovo achievement | `achievements.json` | Nuova medaglia |
| Fix typo | `*.md` | Correzioni testo |
| Traduzione | `docs/` | Nuova lingua |

### 🟡 MEDIO - Per Chi Conosce JS/TS

| Cosa | File | Descrizione |
|------|------|-------------|
| Nuova regola validazione | `engine/src/rules/` | Nuovo controllo PR |
| Nuova arte generativa | `.github/workflows/` | Nuovo SVG generator |
| Nuovo bot behavior | `engine/src/` | Nuova logica |
| Performance | `engine/` | Ottimizzazioni |

### 🔴 AVANZATO - Architetti

| Cosa | File | Descrizione |
|------|------|-------------|
| Nuovo sistema di gioco | `engine/` + `workflows/` | Feature complessa |
| Nuovo tipo di PR | `validator.ts` | Estensione parser |
| Meta-gioco | `*` | Gioco che modifica il gioco |

---

## 🧪 Come Testare Localmente

```bash
# Clone
git clone https://github.com/fabriziosalmi/enjoy.git
cd enjoy

# Setup engine
cd engine
npm install
npm run build

# Esegui tests
npm test

# Valida un PR finto
npm run validate -- --mock --content="TESTWORD"

# Simula karma
npm run karma -- --player="testuser" --action="valid_pr"
```

---

## 🔐 Sicurezza & Limiti

### Cosa PUOI fare:
- ✅ Fork e sperimentare
- ✅ Proporre nuove regole via Issue
- ✅ Creare arte generativa
- ✅ Suggerire ottimizzazioni
- ✅ Rompere cose nel TUO fork

### Cosa NON fare:
- ❌ Exploit del rate limiting
- ❌ Spam di PR invalidi
- ❌ Tentativi di code injection
- ❌ Abuso delle GitHub Actions
- ❌ Manipolazione diretta di state.json

### Rate Limits Rispettati:
- Max 1 PR ogni 5 minuti per player
- Max 100 API calls per workflow
- Workflow timeout: 10 minuti
- Concurrency: 1 per state modification

---

## 💡 Idee per Hacker Creativi

### 1. **Sistema di Alleanze**
Players che collaborano ottengono bonus.
→ Modifica `karma-engine.ts`

### 2. **Seasonal Events**
Eventi speciali in date specifiche.
→ Nuovo workflow con cron

### 3. **Generative Music**
Converti lo stato in MIDI/audio.
→ Nuovo generator workflow

### 4. **3D Visualization**
Three.js viewer del board.
→ GitHub Pages component

### 5. **AI Narrator**
Bot che racconta la storia in tempo reale.
→ Workflow + LLM API

### 6. **Cross-Repo Play**
Altri repo possono "connettersi" a enjoy.
→ Webhook system

---

## 🤝 Come Proporre Cambiamenti Grossi

1. **Apri una Discussion** nella categoria "Ideas"
2. **Descrivi** cosa vuoi fare e perché
3. **Aspetta feedback** dalla community
4. **Se approvato**, apri una Issue "RFC: [nome]"
5. **Implementa** in un branch
6. **PR con tests** e documentazione
7. **Review** collaborativa
8. **Merge** 🎉

---

## 📊 Metriche che Monitoriamo

```
┌────────────────────────────────────────┐
│           HEALTH INDICATORS            │
├────────────────────────────────────────┤
│ Workflow Success Rate    > 95%         │
│ Avg PR Processing Time   < 2 min       │
│ state.json Lock Failures < 1%          │
│ Player Return Rate       > 40%         │
│ Community Response Time  < 24h         │
└────────────────────────────────────────┘
```

---

## 🛠️ Tools Consigliati

- **VS Code** con GitHub Copilot (ovvio 😉)
- **act** - Testa GitHub Actions localmente
- **jq** - Manipola JSON da CLI
- **gh** - GitHub CLI per tutto

```bash
# Installa act per test locali
brew install act

# Esegui workflow localmente
act -j validate-pr --secret GITHUB_TOKEN=$GITHUB_TOKEN
```

---

## 📚 Deep Dives

- [IMPLEMENTATION.md](IMPLEMENTATION.md) - Dettagli tecnici
- [LEVELS_ROADMAP.md](LEVELS_ROADMAP.md) - I 100 livelli
- [GAMEPLAY.md](GAMEPLAY.md) - Meccaniche di gioco
- [engine/README.md](engine/README.md) - Docs motore

---

```
╔════════════════════════════════════════════════════════════════╗
║                                                                ║
║   "First, solve the problem. Then, write the code."           ║
║                                        - John Johnson          ║
║                                                                ║
║   "Ma se il problema è che non c'è abbastanza gioco           ║
║    nel mondo... allora scrivi il gioco."                       ║
║                                        - enjoy philosophy      ║
║                                                                ║
╚════════════════════════════════════════════════════════════════╝
```

---

**Ora sai come funziona. Ora puoi migliorarlo.**

*Welcome to the source.* 🔧💜
