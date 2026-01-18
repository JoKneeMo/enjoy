# 🔧 HACKER'S GUIDE

> For those who want to understand HOW it works, not just WHAT it does.

```
╔═══════════════════════════════════════════════════════════════╗
║  "The best way to understand something is to break it."       ║
║  "The best way to fix it is to build it together."            ║
╚═══════════════════════════════════════════════════════════════╝
```

---

## 🧠 Mental Architecture

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

## 🔍 The Files That Matter

### `state.json` - The Brain
```json
{
  "level": 1,              // Current game level
  "phase": "foundation",   // Current phase (1-5)
  "totalKarma": 0,         // Accumulated global karma
  "totalPRs": 0,           // Total number of PRs
  "players": {},           // Player -> stats map
  "board": [],             // Game contents
  "achievements": [],      // Unlocked achievements
  "lastUpdate": "..."      // Timestamp
}
```

**⚠️ IMPORTANT:** `state.json` has a **concurrency lock**.
Only one workflow at a time can modify it.

### `levels/` - The Rules
100 JSON files, one per level:
```json
{
  "name": "Genesis",
  "phase": "foundation",
  "unlockKarma": 0,
  "rules": {
    "allowedExtensions": [".txt"],
    "maxFileSize": 100,
    "contentPattern": "^[a-zA-Z]{2,20}$"
  }
}
```

### `engine/` - The TypeScript Engine
```
engine/
├── src/
│   ├── validator.ts      # Validates PRs
│   ├── karma-engine.ts   # Calculates karma
│   ├── time-system.ts    # Time multipliers
│   ├── board-manager.ts  # Manages the board
│   └── level-engine.ts   # Level progression
├── dist/                 # Compiled build
└── package.json
```

---

## 🎯 Entry Points for Contributing

### 🟢 EASY - First Steps

| What | File | Description |
|------|------|-------------|
| New level | `levels/` | Add level 101+ |
| New achievement | `achievements.json` | New badge |
| Fix typo | `*.md` | Text corrections |
| Translation | `README.*.md` | New language |

### 🟡 MEDIUM - For JS/TS Developers

| What | File | Description |
|------|------|-------------|
| New validation rule | `engine/src/rules/` | New PR check |
| New generative art | `.github/workflows/` | New SVG generator |
| New bot behavior | `engine/src/` | New logic |
| Performance | `engine/` | Optimizations |

### 🔴 ADVANCED - Architects

| What | File | Description |
|------|------|-------------|
| New game system | `engine/` + `workflows/` | Complex feature |
| New PR type | `validator.ts` | Parser extension |
| Meta-game | `*` | Game that modifies the game |

---

## 🧪 How to Test Locally

```bash
# Clone
git clone https://github.com/fabriziosalmi/enjoy.git
cd enjoy

# Setup engine
cd engine
npm install
npm run build

# Run tests
npm test

# Validate a mock PR
npm run validate -- --mock --content="TESTWORD"

# Simulate karma
npm run karma -- --player="testuser" --action="valid_pr"
```

---

## 🔐 Security and Limits

### What you CAN do:
- ✅ Fork and experiment
- ✅ Propose new rules via Issue
- ✅ Create generative art
- ✅ Suggest optimizations
- ✅ Break things in YOUR fork

### What NOT to do:
- ❌ Rate limiting exploits
- ❌ Invalid PR spam
- ❌ Code injection attempts
- ❌ GitHub Actions abuse
- ❌ Direct state.json manipulation

### Rate Limits Respected:
- Max 1 PR every 5 minutes per player
- Max 100 API calls per workflow
- Workflow timeout: 10 minutes
- Concurrency: 1 for state modifications

---

## 💡 Ideas for Creative Hackers

### 1. **Alliance System**
Players who collaborate get bonuses.
→ Modify `karma-engine.ts`

### 2. **Seasonal Events**
Special events on specific dates.
→ New workflow with cron

### 3. **Generative Music**
Convert state to MIDI/audio.
→ New generator workflow

### 4. **3D Visualization**
Three.js board viewer.
→ GitHub Pages component

### 5. **AI Narrator**
Bot that tells the story in real-time.
→ Workflow + LLM API

### 6. **Cross-Repo Play**
Other repos can "connect" to enjoy.
→ Webhook system

---

## 🤝 How to Propose Big Changes

1. **Open a Discussion** in the "Ideas" category
2. **Describe** what you want to do and why
3. **Wait for feedback** from community
4. **If approved**, open an Issue "RFC: [name]"
5. **Implement** in a branch
6. **PR with tests** and documentation
7. **Collaborative review**
8. **Merge** 🎉

---

## 📊 Metrics We Monitor

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

## 🛠️ Recommended Tools

- **VS Code** with GitHub Copilot
- **act** - Test GitHub Actions locally
- **jq** - Manipulate JSON from CLI
- **gh** - GitHub CLI for everything

```bash
# Install act for local testing
brew install act

# Run workflow locally
act -j validate-pr --secret GITHUB_TOKEN=$GITHUB_TOKEN
```

---

## 📚 Deep Dives

- [IMPLEMENTATION.md](IMPLEMENTATION.md) - Technical details
- [LEVELS_ROADMAP.md](LEVELS_ROADMAP.md) - The 100 levels
- [GAMEPLAY.md](GAMEPLAY.md) - Game mechanics
- [engine/README.md](engine/README.md) - Engine docs

---

```
╔════════════════════════════════════════════════════════════════╗
║                                                                ║
║   "First, solve the problem. Then, write the code."           ║
║                                        - John Johnson          ║
║                                                                ║
║   "But if the problem is that there is not enough play        ║
║    in the world... then write the game."                       ║
║                                        - enjoy philosophy      ║
║                                                                ║
╚════════════════════════════════════════════════════════════════╝
```

---

**Now you know how it works. Now you can improve it.**

*Welcome to the source.* 🔧💜
