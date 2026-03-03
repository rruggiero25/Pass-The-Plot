# 🎭 Pass The Plot

> A local multiplayer party game where stories spiral gloriously out of control.

Built with ❤️ by **Absoluteam**

---

## What is Pass The Plot?

Pass The Plot is a party game inspired by the telephone game mechanic of [Gartic Phone](https://garticphone.com/). Players take turns writing a story and drawing it — passing the chain around until nobody knows how it started. The final reveal is always chaotic, always hilarious.

No accounts. No internet. Just devices, creativity, and chaos.

---

## How to Play

1. **One player hosts** a lobby — a 4-digit code is generated automatically.
2. **Other players join** by entering the code on their own device (local Wi-Fi or Bluetooth required).
3. Once everyone is in, the **host starts the game**.
4. Each round alternates between two phases:
   - ✍️ **Write** — You receive a prompt (or the last drawing) and write a continuation.
   - 🎨 **Draw** — You receive a sentence and illustrate it on the canvas.
5. The chain passes to a different player each round — no one sees the full story as it grows.
6. When all rounds are complete, the **Reveal** shows every chain from start to finish.
7. Watch how your original prompt turned into something completely unrecognizable. 😄

### Tips
- A timer can be set by the host — when it runs out, your answer is auto-submitted.
- You need at least **3 players** to start a game.
- Use the **Share** button on the Reveal screen to save or send your favourite chains.

---

## Tech Stack

| Area | Technology |
|---|---|
| Platform | iOS 17+ |
| Language | Swift 5.9 |
| UI Framework | SwiftUI |
| Multiplayer | MultipeerConnectivity (peer-to-peer, no server) |
| Drawing | PencilKit |
| State Machine | GameplayKit (`GKStateMachine`) |
| Font | OpenDyslexic |

---

## Architecture

Pass The Plot follows a **state machine driven flow** powered by `GKStateMachine`. Every screen transition is an explicit state, making the app flow predictable and easy to extend.

```
WelcomeState
    ├── HostingState → LobbyState
    └── CodeEntryState → ConnectingState → LobbyState
                                                │
                                         HostSettingsState
                                         WaitingForHostSettingsState
                                                │
                                          CountdownState
                                                │
                                           GameState  ←──────────────────┐
                                          /        \                      │
                                    WriteView    DrawView                 │
                                          \        /                      │
                                       (next round) ──────────────────── ┘
                                                │
                                           RevealState
```

**Networking** is handled entirely by `MultipeerManager`, which wraps `MCSession` and communicates via a `Codable` message enum (`MPMessage`). The host acts as the game coordinator — distributing prompts, collecting answers, building story chains, and triggering round transitions.

**Game data** never leaves the local network. All story chains are built and stored on the host device, then broadcast to all players at reveal time.

---

## Project Structure

```
Lofi/
├── App/
│   ├── LofiApp.swift              # Entry point, root view, state wiring
│   ├── AppModel.swift             # Shared observable state
│   └── AppFlowStateMachine.swift  # GKStateMachine flow controller
├── Views/
│   ├── Game/
│   │   ├── GameView.swift         # Routes between Write and Draw phases
│   │   ├── WriteAnswerView.swift  # Text input round
│   │   └── DrawView.swift         # PencilKit drawing round
│   ├── Reveal/
│   │   └── RevealView.swift       # Final chain reveal screen
│   ├── Lobby/
│   │   └── LobbyView.swift
│   ├── Home/
│   │   └── HomeEntryView.swift
│   └── Join/
│       ├── JoinCodeView.swift
│       └── FourDigitCodeField.swift
├── Networking/
│   ├── MultipeerManager.swift     # All peer-to-peer logic
│   └── MPMessage.swift            # Codable message protocol
├── Models/
│   ├── AppFlowStateMachine.swift
│   ├── StoryModels.swift          # StorySnippet, RoundAssignment
│   ├── GameSettings.swift
│   └── Player.swift
└── Design/
    ├── DesignTokens.swift         # Colors, fonts, spacing constants
    ├── HardShadowComponents.swift
    └── GridBackground.swift
```

---

## Requirements

- Xcode 16+
- iOS 17.0+
- Devices must be on the same local Wi-Fi network or have Bluetooth enabled

---

## Installation

1. Clone the repository:
   ```bash
   git clone https://github.com/absoluteam/pass-the-plot.git
   ```
2. Open `Lofi.xcodeproj` in Xcode.
3. Select your target device or simulator.
4. Build and run (`⌘ + R`).

> No external dependencies or package manager setup required.

---

## Privacy

Pass The Plot collects no personal data and requires no account. All communication is local and encrypted. See [PRIVACY_POLICY.md](./PRIVACY_POLICY.md) for full details.

---

## Made by

**Absoluteam** — *Apple Developer Academy*

---

*Pass The Plot — because no story survives contact with your friends.*
