# pwnagotchi_plugins

A collection of plugins for [Pwnagotchi](https://github.com/jayofelony/pwnagotchi).

---

## 📦 Available Plugins

### 🔹 Qpwn

**Qpwn** is a Pwnagotchi plugin that uses Q-learning to adapt recon and attack strategies in real time. It tracks access points and clients, assigns scores, and selects recon modes based on environmental data and performance outcomes.

#### How It Works

- **State**: `(blind_epochs, total_aps, total_clients, hour_bucket)`
- **Actions**: Recon configs like `(recon_time, recon_sleep, timeout)`
- **Reward**: `handshakes * 5 - blind_epochs * 2`
- **Q-table** stored at `/etc/pwnagotchi/qpwn_brain.json`
- **Memory** of APs/clients stored at `/etc/pwnagotchi/qpwn.json`

#### Attack Logic

- Clients are scored based on signal, activity, success
- Attacks are delayed, retried, or skipped based on:
  - Cooldowns
  - Previous success
  - Attempt count
  - Whitelist
- Sends broadcast deauths to idle APs

#### Recon Modes

| Label    | Time | Sleep | Timeout |
|----------|------|-------|---------|
| BLITZ    | 5    | 2     | 10      |
| ASSAULT  | 10   | 5     | 15      |
| SIEGE    | 15   | 10    | 20      |
| BERSERK  | 3    | 1     | 8       |

#### Features

- Q-learning with decay and exploration
- Channel optimization
- Persistent Q-table and AP memory
- Web UI
- Live stats and learning graph in browser
- Display recon mode + handshake count on UI
- Supports MAC/SSID whitelist

#### Why Qpwn May Perform Differently

Unlike fixed-logic plugins, Qpwn doesn’t rely on static attack rules or preset tuning thresholds. It uses reinforcement learning to discover the best recon and attack parameters based on actual results. Over time, it optimizes for high handshake yield and low blind time through learned decision-making, not hardcoded conditions. This can lead to more adaptive and efficient behavior, especially in environments with fluctuating AP/client density.


#### Config

```toml
main.plugins.qpwn.enabled = true
```



---

