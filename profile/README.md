# P2Pool Starter Stack

Open-source tools for running your own Monero mining setup on hardware you control.

[![Pithead release](https://img.shields.io/github/v/release/p2pool-starter-stack/pithead?sort=semver&display_name=tag&label=Pithead&color=F26822&logo=github)](https://github.com/p2pool-starter-stack/pithead/releases/latest)
[![RigForge release](https://img.shields.io/github/v/release/p2pool-starter-stack/rigforge?sort=semver&display_name=tag&label=RigForge&color=ff5236&logo=github)](https://github.com/p2pool-starter-stack/rigforge/releases/latest)

Website: [p2pool-starter-stack.github.io](https://p2pool-starter-stack.github.io/)

| Project | What it does |
|---|---|
| [Pithead](https://github.com/p2pool-starter-stack/pithead) | Runs a Monero node, [P2Pool](https://github.com/SChernykh/p2pool), Tor, a single mining endpoint for your workers, and a web dashboard, as a Docker Compose stack on Ubuntu Server 24.04 LTS. |
| [RigForge](https://github.com/p2pool-starter-stack/rigforge) | Compiles upstream XMRig from source on an Ubuntu or Debian machine, tunes the system for RandomX, and runs the miner as a service. Works with Pithead or any RandomX Stratum pool. |

## Why run it

- **No pool operator.** P2Pool has no pool operator and charges no pool fee. Rewards are paid
  directly to your own wallet.
- **Your own node.** Pithead runs its own Monero node, or uses one you run on another machine.
- **Tor-first networking.** The stack's outbound runtime traffic goes over Tor by default, and
  inbound peers reach P2Pool and the local nodes through onion addresses, so you do not forward
  ports.
- **One endpoint for your rigs.** Point existing XMRig rigs at the stack on port `3333`, with no
  wallet address in the miner config, and follow hashrate and per-worker stats in the dashboard.

## Before you start

- **Tor-first does not make mining anonymous.** Workers reach the stack over plain stratum on
  your LAN. A node you run on another machine is dialled directly. Install and update downloads
  reveal your IP address to the download host. The optional clearnet initial sync, P2Pool clearnet peering
  and XvB without Tor expose your IP while they are on. P2Pool payout addresses are public, so
  [use a dedicated mining wallet](https://github.com/SChernykh/p2pool/releases/tag/v4.18.1). The
  [privacy guide](https://github.com/p2pool-starter-stack/pithead/blob/main/docs/privacy.md) lists
  every connection.
- **Payouts vary.** P2Pool pays when the pool finds blocks, so payouts swing with mining luck. The
  dashboard's earnings figures are estimates, not guarantees.
- **No pool fee is not the same as no costs.** RigForge's XMRig donation defaults to 1%, XMRig's
  own upstream default, and goes to the XMRig project; set `"DONATION": 0` before setup to turn it
  off (on an already-built worker, lowering it needs a rebuild). With XvB
  enabled, Pithead donates part of your hashrate to the XMRvsBeast pool to hold a raffle tier.
  Electricity and hardware are your own costs; once you set an electricity price, the dashboard
  can show power cost and net figures from each rig's measured or manually estimated power draw.
- **Tari and XvB.** Pithead can also merge-mine Tari on the same work and switch hashrate for the
  XMRvsBeast raffle. Each has its own configuration, a local Tari node needs its own disk and
  memory, and Tari blocks are found less often than P2Pool's. The latest Pithead release pins Tari 5.3.1;
  Pithead's changelog on `develop` notes that Tari's 6.0 hard fork at block 350,000 leaves older
  nodes off the canonical chain, and that update is not released yet. See
  [configuration](https://github.com/p2pool-starter-stack/pithead/blob/main/docs/configuration.md)
  and [hardware requirements](https://github.com/p2pool-starter-stack/pithead/blob/main/docs/hardware.md).
- **Setup comes before the sync.** `./pithead setup` checks dependencies, asks for your payout
  addresses, provisions Tor and offers to start the stack. P2Pool and the worker endpoint then wait
  until the nodes finish their initial blockchain sync. RigForge needs root to install packages and
  tune the system, and on Linux a reboot to apply HugePages.
- **Wallets in workers.** A worker pointed at Pithead needs no wallet address. For a public pool,
  you set your Monero wallet as the pool user in RigForge's config.
- **Browser control is opt-in.** Editing configuration, retuning rigs and one-click upgrades from
  the dashboard stay off until you set `dashboard.control.enabled`, and every change sits behind
  the dashboard login. Retuning a rig also needs a RigForge worker with its token-authenticated
  control port configured.
- **Platforms.** Pithead supports Ubuntu Server 24.04 LTS as its host; other systems are not
  supported. RigForge targets Ubuntu and Debian; its macOS support is deprecated as of 2026-09-14
  and untested.
- **Performance figures.** RigForge's
  [benchmarks](https://github.com/p2pool-starter-stack/rigforge/blob/develop/docs/benchmarks.md)
  compare its tuning with stock XMRig on two named CPUs. They measure CPU-package power, not wall
  power, and the comparison with a hand-tuned worker used a different XMRig version. Results vary
  with CPU, RAM and kernel.

## Start here

- Run the stack: [Pithead getting started](https://github.com/p2pool-starter-stack/pithead/blob/main/docs/getting-started.md).
- Already have XMRig rigs: [connect them](https://github.com/p2pool-starter-stack/pithead/blob/main/docs/workers.md).
- Provision a tuned worker: [RigForge](https://github.com/p2pool-starter-stack/rigforge).

Pithead OS, a bootable appliance image, is in development on Pithead's
[`develop`](https://github.com/p2pool-starter-stack/pithead/tree/develop) branch and is not
released yet. The badges above show the latest published releases. Issues and pull requests are
welcome.

## License

Pithead's and RigForge's own code is MIT-licensed. Third-party components keep their own
licenses: Pithead ships P2Pool and xmrig-proxy (GPLv3) unmodified as separate containers, and
RigForge compiles upstream XMRig (GPLv3) on your machine. See the license sections of
[Pithead](https://github.com/p2pool-starter-stack/pithead#-license) and
[RigForge](https://github.com/p2pool-starter-stack/rigforge#-license).

## Donate

If these projects saved you time and you'd like to support the work, donations to this XMR wallet
are appreciated:

```
486aGn4qhH1MkaASjnEWMDN7stD1SVtPF5fvihmjffeBE5ACL1u1jU95KxiqmoiaPZMexi4R4W11MLXut66XWVVF8wjAE5R
```
