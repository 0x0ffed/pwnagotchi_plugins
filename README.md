# pwnagotchi_plugins

A collection of plugins for [Pwnagotchi](https://github.com/jayofelony/pwnagotchi).

---





# Qpwn ![Qpwn banner](https://img.shields.io/badge/status-stable-green?style=flat-square)  

**Learns. Scores. Pwns.**  
A self-adapting Q-learning plugin that intelligently attacks clients based on past performance. Unlike brute-force methods, Qpwn evolves its strategy over time to maximize handshakes per attempt.


---

## What It Does

- Tracks APs and clients in memory
- Scores clients based on signal, activity, and past success
- Filters out weak clients automatically using percentile thresholds
- Learns the best recon mode over time using Q-learning
- Updates recon strategy for each AP based on handshake success
- Deauths idle APs with no clients to trigger probe traffic
- Picks the best Wi-Fi channel to focus on each epoch
- Exposes a web UI dashboard with live stats and learning graphs

## How It Works

Every epoch:

1. Updates scores for every tracked client  
2. Learns how good the current strategy was based on handshake delta  
3. Adjusts Q-table values for current state and strategy  
4. Picks the next recon mode using either learned preference or random choice  
5. Skips clients that don't meet the score threshold  
6. Broadcast deauths APs with no clients  
7. Picks the next best channel based on AP/client/handshake weight  
8. Attacks only clients that are worth attacking  

It filters clients based on score. The score comes from signal strength, time since last seen, success history, and number of attempts. If a client scores below the 60th percentile, it is skipped.

The Q-learning engine runs globally and also per AP. It uses blind count, number of APs, number of clients, and time bucket (hour) as state. The actions are preset recon profiles. Every recon choice is adjusted over time based on how many handshakes it leads to.

## Why It’s Better

Most plugins attack everything they see. This wastes time, heats up the Pi, and floods the air with junk. Qpwn filters targets, avoids dead clients, and focuses only on what gives results. It adapts instead of guessing.

Over time, it hits fewer targets but captures more handshakes.


```toml
main.plugins.qpwn.enabled = true
```



---

