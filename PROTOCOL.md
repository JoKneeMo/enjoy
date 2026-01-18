# 🔌 ENJOY PROTOCOL

> **The layer that transforms enjoy from game to platform.**

```
╔═══════════════════════════════════════════════════════════════════════════╗
║                                                                           ║
║   "We are not building a game.                                            ║
║    We are building the foundation on which others will build."            ║
║                                                                           ║
╚═══════════════════════════════════════════════════════════════════════════╝
```

---

## 🌐 What is the enjoy Protocol?

An **open standard** that allows:
- 🎮 **Other games** to connect to enjoy
- 🛠️ **Tools and bots** to interact with the state
- 🎨 **Artists** to create assets that live in the game
- 🏗️ **Builders** to build on our foundations

---

## 🔗 How It Works

### 1. State API (Read-Only)
Any repo can read enjoy's state:

```bash
# Current game state
curl https://raw.githubusercontent.com/fabriziosalmi/enjoy/main/state.json

# Player list
curl https://raw.githubusercontent.com/fabriziosalmi/enjoy/main/game/players/

# Current level
curl https://raw.githubusercontent.com/fabriziosalmi/enjoy/main/levels/
```

### 2. Webhook Integration
Your workflows can listen to enjoy events:

```yaml
# In your repo
name: React to enjoy
on:
  repository_dispatch:
    types: [enjoy-level-up, enjoy-achievement, enjoy-event]

jobs:
  react:
    runs-on: ubuntu-latest
    steps:
      - name: Do something when enjoy levels up
        run: echo "enjoy reached level ${{ github.event.client_payload.level }}"
```

### 3. Cross-Repo Karma
Other projects can contribute karma to enjoy:

```yaml
# In your repo - contribute karma when someone does something good
- name: Send karma to enjoy
  uses: peter-evans/repository-dispatch@v2
  with:
    token: ${{ secrets.ENJOY_TOKEN }}
    repository: fabriziosalmi/enjoy
    event-type: external-karma
    client-payload: '{"player": "${{ github.actor }}", "amount": 5, "reason": "helped in my-project"}'
```

---

## 🎨 For Artists

### Contribute Generative Art

1. Create SVG that reacts to game state:
```svg
<!-- Your SVG can read enjoy variables -->
<svg data-enjoy-level="LEVEL" data-enjoy-karma="KARMA">
  <!-- Art that changes based on level -->
</svg>
```

2. Propose via Issue with label `art`
3. If approved, it becomes part of the game!

### Decentralized Gallery
Your art can live in YOUR repo but be visible in enjoy:

```json
// game/art/external.json
{
  "galleries": [
    {
      "artist": "@your_username",
      "repo": "your_username/enjoy-art",
      "pieces": ["piece1.svg", "piece2.svg"]
    }
  ]
}
```

---

## 🛠️ For Hackers/Builders

### Build Tools That Interact

**Possible project examples:**

| Project | Description |
|---------|-------------|
| `enjoy-cli` | CLI to play from terminal |
| `enjoy-dashboard` | Real-time state dashboard |
| `enjoy-music` | Generate music from game state |
| `enjoy-3d` | 3D board visualization |
| `enjoy-mobile` | Mobile app for notifications |
| `enjoy-discord` | Discord bot that announces events |
| `enjoy-telegram` | Telegram bot to play |
| `enjoy-vscode` | VS Code extension |

### State Schema

```typescript
interface EnjoyState {
  level: number;           // 1-100
  phase: string;           // foundation|complexity|metamorphosis|consciousness|transcendence
  totalKarma: number;      // Global karma
  totalPRs: number;        // Total PRs
  players: {
    [username: string]: {
      karma: number;
      streak: number;
      achievements: string[];
      lastActive: string;
    }
  };
  board: any[];            // Game contents
  lastHeartbeat: string;   // Last pulse
}
```

---

## 🌍 Satellite Projects

These projects can "orbit" around enjoy:

### Tier 1: Official
Maintained by enjoy core community:
- `enjoy-docs` - Extended documentation
- `enjoy-art` - Official gallery
- `enjoy-translations` - All language translations

### Tier 2: Community
Maintained by the community:
- Tools, bots, visualizations
- Games using the enjoy protocol
- Integrations with other platforms

### Tier 3: Experimental
- Creative forks
- Game mods
- Alternative rule versions

---

## 📜 Protocol License

**MIT** - Do what you want, but:
1. Do not be evil
2. Give credit where due
3. Share improvements

---

## 🤝 How to Propose Protocol Extensions

1. Open an Issue with labels `enhancement` + `protocol`
2. Describe the proposed extension
3. Community discussion
4. If approved, becomes part of the standard

---

## 🔮 Protocol Roadmap

### v0.1 (Now)
- [x] Publicly readable state
- [x] Webhooks for basic events
- [x] External art linkable

### v0.2 (Next)
- [ ] Official REST API
- [ ] OAuth for cross-repo actions
- [ ] Standard for generative art

### v0.3 (Future)
- [ ] Federation between enjoy instances
- [ ] Cross-chain karma (other "enjoys" on GitLab, etc.)
- [ ] Official SDK (JS/Python/Go)

### v1.0 (Dream)
- [ ] enjoy becomes a protocol, not just a repo
- [ ] Anyone can host an enjoy instance
- [ ] Interconnected universe of collaborative games

---

```
╔═══════════════════════════════════════════════════════════════════════════╗
║                                                                           ║
║   "A protocol is not a product.                                           ║
║    It is a promise."                                                      ║
║                                                                           ║
║   "We promise that anyone can build on enjoy.                             ║
║    We promise it will remain open forever.                                ║
║    We promise the game belongs to everyone."                              ║
║                                                                           ║
╚═══════════════════════════════════════════════════════════════════════════╝
```

---

## 🚀 Start Now

1. **Read** the [state.json](state.json)
2. **Study** the [workflows](.github/workflows/)
3. **Propose** a satellite project
4. **Build** something incredible

**The protocol is open. The platform is yours.**

*What will you build?* 🔌💜
