# P2Pool Starter Stack

Open-source tools for running your own Monero mining setup on hardware you control.

[![Pithead release](https://img.shields.io/github/v/release/p2pool-starter-stack/pithead?sort=semver&display_name=tag&label=Pithead&color=F26822&logo=github)](https://github.com/p2pool-starter-stack/pithead/releases/latest)
[![RigForge release](https://img.shields.io/github/v/release/p2pool-starter-stack/rigforge?sort=semver&display_name=tag&label=RigForge&color=ff5236&logo=github)](https://github.com/p2pool-starter-stack/rigforge/releases/latest)

Website: [p2pool-starter-stack.github.io](https://p2pool-starter-stack.github.io/)

| Project | What it does |
| --- | --- |
| [Pithead](https://github.com/p2pool-starter-stack/pithead) | Runs a Monero node, [P2Pool](https://github.com/SChernykh/p2pool), Tor, a single mining endpoint for your workers, and a web dashboard. Use the bootable Pithead OS appliance or the Docker Compose stack on Ubuntu Server 24.04 LTS. |
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
- **Appliance or DIY.** Pithead OS installs from USB and is configured in a browser; the Compose
  path runs the same stack and dashboard on an Ubuntu host you manage.
- **Recovery built in.** The appliance installs signed updates to a spare system slot and returns
  to the previous slot if the new one does not boot cleanly. Encrypted backup and restore, service
  diagnostics, and setup and recovery options for machines without a monitor are included.
  System rollback does not undo a one-way database migration; keep backups off the machine.

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
- **Tari and XvB are optional beta features.** New installs leave both off. Pithead can merge-mine
  Tari on the same work and switch hashrate for the XMRvsBeast raffle when you enable them. Each
  has its own configuration, a local Tari node needs its own disk and memory, and Tari blocks are
  found less often than P2Pool's. See
  [configuration](https://github.com/p2pool-starter-stack/pithead/blob/main/docs/configuration.md)
  and [hardware requirements](https://github.com/p2pool-starter-stack/pithead/blob/main/docs/hardware.md).
- **Choose the machine's role.** Pithead OS can be a coordinator, a coordinator that also mines on
  its own CPU, or a mining rig. The appliance uses a browser setup wizard; DIY uses `./pithead setup`.
  Mining starts after the required nodes finish their initial blockchain sync.
  RigForge needs root to install packages and tune the system, and on Linux a reboot to apply
  HugePages.
- **Wallets in workers.** A worker pointed at Pithead needs no wallet address. For a public pool,
  you set your Monero wallet as the pool user in RigForge's config.
- **Browser control is opt-in.** Editing configuration, retuning rigs and one-click upgrades from
  the dashboard stay off until you set `dashboard.control.enabled`, and every change sits behind
  the dashboard login. Remote rig control is beta and needs a RigForge control token; an appliance
  rig mines before adoption, but the dashboard cannot change it until you adopt it.
- **Platforms and hardware.** Pithead OS needs an x86-64 UEFI machine with wired Ethernet and a
  16 GB+ USB stick. A coordinator needs 16 GB+ RAM and an internal SSD; installation erases the
  selected disk. A worker-only rig can run from USB and has no chain-storage requirement. The DIY path needs
  Ubuntu Server 24.04 LTS. Disk needs depend on where the Monero and Tari nodes run; check the
  [hardware guide](https://github.com/p2pool-starter-stack/pithead/blob/main/docs/hardware.md).
  RigForge targets Ubuntu and Debian; its macOS support is deprecated as of 2026-09-14 and untested.
- **Performance figures.** RigForge's
  [benchmarks](https://github.com/p2pool-starter-stack/rigforge/blob/develop/docs/benchmarks.md)
  compare its tuning with stock XMRig on two named CPUs. They measure CPU-package power, not wall
  power, and the comparison with a hand-tuned worker used a different XMRig version. Results vary
  with CPU, RAM and kernel.

## Start here

- Install the appliance: [Pithead OS guide](https://github.com/p2pool-starter-stack/pithead/blob/main/docs/appliance.md).
- Run the Compose stack: [Pithead getting started](https://github.com/p2pool-starter-stack/pithead/blob/main/docs/getting-started.md).
- Already have XMRig rigs: [connect them](https://github.com/p2pool-starter-stack/pithead/blob/main/docs/workers.md).
- Provision a tuned worker: [RigForge](https://github.com/p2pool-starter-stack/rigforge).
- Check current limitations: [Pithead release notes and known issues](https://github.com/p2pool-starter-stack/pithead/releases/latest).

Both Pithead install paths run the same stack, dashboard, and configuration. The badges above show
the latest published releases. Issues and pull requests are welcome.

## License

Pithead's and RigForge's own code is MIT-licensed. Third-party components keep their own
licenses: Pithead ships P2Pool and xmrig-proxy (GPLv3) unmodified as separate containers, and
RigForge compiles upstream XMRig (GPLv3) on your machine. See the license sections of
[Pithead](https://github.com/p2pool-starter-stack/pithead#-license) and
[RigForge](https://github.com/p2pool-starter-stack/rigforge#-license).

## Donate

If these projects saved you time and you'd like to support the work, donations to this XMR wallet
are appreciated:

```text
486aGn4qhH1MkaASjnEWMDN7stD1SVtPF5fvihmjffeBE5ACL1u1jU95KxiqmoiaPZMexi4R4W11MLXut66XWVVF8wjAE5R
```
