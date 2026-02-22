# 🌐 StellarConduit

> **Payments that work everywhere. Even where the internet doesn't.**

StellarConduit is an open-source, offline-first payment network built on the [Stellar blockchain](https://stellar.org). It enables peer-to-peer financial transactions in environments with no internet connectivity — propagating signed transactions across a Bluetooth and WiFi-Direct mesh network, and settling them on Stellar when any node in the mesh reaches connectivity.

We are building the financial infrastructure layer for the **2.9 billion people** who live in low-connectivity regions — rural communities, conflict zones, disaster relief areas, and underserved populations who cannot access traditional or internet-dependent financial systems.

---

## ⚡ The Problem

Every blockchain payment application built today assumes one thing: internet access.

That assumption excludes billions of people.

- A farmer in rural Nigeria cannot send money to his family 3 villages away because there is no signal.
- A refugee in a displacement camp cannot receive aid because the nearest cell tower is destroyed.
- A disaster relief worker cannot disburse emergency funds because the infrastructure is down.
- A market trader in a remote town cannot accept digital payment because data is too expensive.

Traditional fintech has not solved this. Mobile money requires GSM. Bank apps require internet. Even most crypto wallets fail without a node connection.

**StellarConduit solves this at the protocol level.**

---

## 💡 How It Works

StellarConduit creates a **local mesh network** between nearby devices using Bluetooth Low Energy and WiFi-Direct. Transactions are cryptographically signed on the sender's device — completely offline — and propagated peer-to-peer through the mesh using a gossip protocol until they reach a **relay node**: any device in the mesh that has internet access.

The relay node batches the queued transactions and settles them on the Stellar network. Every step of the relay chain is cryptographically proven and auditable.
```

[Sender Device] — BLE/WiFi-Direct → [Peer Device] → [Peer Device] → [Relay Node] — Stellar Network → [Settlement]
```

### Key Properties

- **Truly offline** — transactions are signed and propagated without any internet connection on the sender's device
- **Trustless relay** — relay nodes cannot tamper with transactions; they are pre-signed by the sender
- **Double-spend resistant** — conflict resolution protocol handles split mesh clusters and race conditions
- **Auditable** — every relay hop is logged with cryptographic proof of propagation chain
- **Stellar-native** — built on Stellar's fast, low-fee, energy-efficient blockchain
- **Open protocol** — anyone can run a relay node, build a client, or extend the protocol

---

## 🏗️ Architecture Overview

StellarConduit is composed of six core layers that work together:

### 1. Mesh Layer
The mesh layer handles device discovery, peer-to-peer connection establishment, and message routing between devices. It uses Bluetooth Low Energy for discovery and initial handshake, and WiFi-Direct for higher-bandwidth data transfer when available. The mesh is self-organizing — devices automatically discover peers, establish connections, and route messages toward relay nodes using a topology-aware routing algorithm.

### 2. Gossip Protocol
Once a transaction is signed offline, it enters the gossip network. Each device that receives a transaction envelope forwards it to all known peers that have not yet seen it. The gossip protocol is designed for extremely low-bandwidth environments — message sizes are minimized, redundant transmissions are deduplicated using bloom filters, and propagation is prioritized toward peers closest to known relay nodes.

### 3. Offline Transaction Engine
The offline transaction engine handles the creation, signing, and queuing of Stellar transaction envelopes without any network connection. It manages the transaction queue with priority ordering (emergency payments first), handles sequence number reservation to prevent conflicts, and stores signed envelopes durably on-device until settlement confirmation is received.

### 4. Conflict Resolution Engine
In a mesh network, the same transaction may propagate through multiple paths simultaneously, and network partitions can create split clusters where the same funds are spent in two places. The conflict resolution engine implements a deterministic algorithm for resolving double-spend attempts in offline clusters, using timestamps, cryptographic proofs, and a consensus mechanism among relay nodes to determine which transaction is valid.

### 5. Relay Node
A relay node is any device in the mesh that has internet connectivity. It receives batched transaction envelopes from the mesh, verifies their cryptographic integrity, submits them to the Stellar network, and propagates settlement confirmations back through the mesh. Relay nodes earn a small protocol fee for their service, creating an economic incentive to run relay infrastructure in underserved areas.

### 6. Soroban Smart Contracts
The on-chain layer of StellarConduit is built on Stellar's Soroban smart contract platform. Contracts handle relay node registration and staking, fee distribution, dispute resolution for contested transactions, and the protocol treasury.

---

## 📦 Repositories

This organization is structured as a monorepo-of-repos. Each repository is a self-contained module of the StellarConduit protocol:

| Repository | Description | Language | Status |
|---|---|---|---|
| [`stellarconduit-core`](https://github.com/StellarConduit/stellarconduit-core) | Mesh networking engine, gossip protocol, and peer discovery | Rust | 🚧 In Development |
| [`stellarconduit-mobile`](https://github.com/StellarConduit/stellarconduit-mobile) | Android and iOS wallet application | React Native / Kotlin | 🚧 In Development |
| [`stellarconduit-relay-node`](https://github.com/StellarConduit/stellarconduit-relay-node) | Relay node software for bridging mesh to Stellar | Rust / Node.js | 🚧 In Development |
| [`stellarconduit-sync-engine`](https://github.com/StellarConduit/stellarconduit-sync-engine) | Offline transaction queue, conflict resolution, and settlement | Rust | 🚧 In Development |
| [`stellarconduit-contracts`](https://github.com/StellarConduit/stellarconduit-contracts) | Soroban smart contracts for relay staking and fee distribution | Rust (Soroban) | 🚧 In Development |
| [`stellarconduit-backend`](https://github.com/StellarConduit/stellarconduit-backend) | API layer, settlement coordination, monitoring, and analytics | Node.js / TypeScript | 🚧 In Development |
| [`stellarconduit-docs`](https://github.com/StellarConduit/stellarconduit-docs) | Protocol documentation, architecture specs, whitepapers, and RFCs | Markdown | 🚧 In Development |

---

## 🗺️ Roadmap

### Phase 1 — Foundation `(Current)`
- [ ] Protocol specification and architecture documentation
- [ ] Core gossip protocol implementation
- [ ] Bluetooth Low Energy device discovery module
- [ ] Offline transaction envelope design and signing
- [ ] Relay node MVP with Stellar testnet integration
- [ ] Basic mobile wallet (Android first)
- [ ] Local mesh network simulator for development and testing

### Phase 2 — Mesh Intelligence
- [ ] WiFi-Direct integration alongside BLE
- [ ] Topology-aware routing algorithm
- [ ] Conflict resolution engine for split clusters
- [ ] Transaction queue with priority ordering
- [ ] Relay node Docker packaging and easy deployment
- [ ] Relay node economic model and fee distribution contracts
- [ ] End-to-end encryption of all mesh messages

### Phase 3 — Hardening & Scale
- [ ] Formal security audit of the gossip protocol
- [ ] Double-spend attack simulation and stress testing
- [ ] Multi-hop relay chain with cryptographic proof of relay
- [ ] iOS mobile wallet
- [ ] Relay node monitoring dashboard
- [ ] Community relay node map and directory

### Phase 4 — Ecosystem
- [ ] StellarConduit SDK for third-party app developers
- [ ] Hardware relay node reference design (Raspberry Pi based)
- [ ] Integration with Stellar anchor network for fiat on/off ramp
- [ ] Grants program for relay node operators in underserved regions
- [ ] Pilot deployment partnerships in target regions

---

## 🤝 Contributing

StellarConduit is built entirely by the open-source community. We welcome contributors of all skill levels — whether you write code, design systems, write documentation, or just want to help test.

### Where to Start

1. Read our [Contributing Guide](https://github.com/StellarConduit/stellarconduit-docs/blob/main/guides/contributing.md)
2. Browse issues labeled [`good first issue`](https://github.com/issues?q=org%3AStellarConduit+label%3A%22good+first+issue%22) across all repositories
3. Join the discussion in our community channels
4. Read the [Architecture Overview](https://github.com/StellarConduit/stellarconduit-docs/blob/main/architecture/overview.md) to understand how everything fits together

### Contribution Areas

We need help across many disciplines:

- **Protocol Engineering** — gossip protocol, conflict resolution, mesh topology
- **Mobile Development** — React Native, Android (Kotlin), iOS (Swift)
- **Rust Development** — core engine, relay node, sync engine
- **Smart Contracts** — Soroban/Rust contract development
- **Cryptography** — transaction signing, relay chain proofs, security review
- **DevOps** — Docker, CI/CD, relay node infrastructure
- **Technical Writing** — specs, RFCs, guides, API documentation
- **UI/UX Design** — mobile wallet design, onboarding flows
- **Research** — gossip protocols, mesh networking, offline-first systems
- **Testing** — unit tests, integration tests, network simulation

### Our RFC Process

Major protocol decisions go through our RFC (Request for Comments) process. Anyone can propose an RFC. RFCs are discussed openly in the [`stellarconduit-docs`](https://github.com/StellarConduit/stellarconduit-docs) repository before being accepted or rejected. This ensures the protocol is designed transparently with community input.

---

## 🛠️ Tech Stack

| Layer | Technology |
|---|---|
| Smart Contracts | Rust, Soroban SDK |
| Core Engine | Rust |
| Relay Node | Rust, Node.js, TypeScript |
| Mobile | React Native, Kotlin (Android), Swift (iOS) |
| Backend API | Node.js, TypeScript, PostgreSQL |
| Mesh Communication | Bluetooth Low Energy, WiFi-Direct |
| Blockchain | Stellar Network |
| Testing | Jest, Rust test framework, custom mesh simulator |
| Infrastructure | Docker, GitHub Actions |

---

## 📖 Documentation

All protocol documentation lives in [`stellarconduit-docs`](https://github.com/StellarConduit/stellarconduit-docs).

Key documents to start with:

- [Protocol Whitepaper](https://github.com/StellarConduit/stellarconduit-docs/blob/main/whitepaper/stellarconduit-whitepaper.md)
- [Architecture Overview](https://github.com/StellarConduit/stellarconduit-docs/blob/main/architecture/overview.md)
- [Transaction Lifecycle](https://github.com/StellarConduit/stellarconduit-docs/blob/main/architecture/transaction-lifecycle.md)
- [Gossip Protocol Spec](https://github.com/StellarConduit/stellarconduit-docs/blob/main/specs/gossip-protocol-spec.md)
- [Offline Transaction Envelope Spec](https://github.com/StellarConduit/stellarconduit-docs/blob/main/specs/offline-transaction-envelope-spec.md)
- [Running a Relay Node](https://github.com/StellarConduit/stellarconduit-docs/blob/main/guides/running-a-relay-node.md)
- [Local Development Setup](https://github.com/StellarConduit/stellarconduit-docs/blob/main/guides/local-development.md)

---

## 🌍 Why Stellar

We chose Stellar because it is the most aligned blockchain for this use case:

- **Speed** — 3-5 second finality means settlement is near-instant when connectivity is restored
- **Cost** — transaction fees are fractions of a cent, viable for micro-payments in low-income contexts
- **Soroban** — Stellar's smart contract platform gives us the programmability we need without EVM complexity
- **Mission alignment** — Stellar's core mission is financial inclusion and cross-border payments for underserved populations, which is exactly who StellarConduit is built for
- **Anchor network** — Stellar's existing anchor ecosystem means fiat on/off ramps already exist in many of our target markets

---

## 📜 License

StellarConduit is open-source software licensed under the [Apache 2.0 License](LICENSE). You are free to use, modify, and distribute this software. Contributions are welcome under the same license.

---

## 🔒 Security

If you discover a security vulnerability in any StellarConduit repository, please do **not** open a public issue. Instead, send a responsible disclosure to **security@stellarconduit.org**. We take security seriously and will respond within 48 hours.

---

## 🌱 Community

StellarConduit is more than a protocol. It is a community of engineers, researchers, and advocates who believe financial access is a human right.

- **GitHub Discussions** — technical questions, ideas, and proposals
- **Twitter/X** — [@StellarConduit](https://twitter.com/StellarConduit)
- **Discord** — [Join our server](#) *(coming soon)*
- **Telegram** — [Dm on Telegram](t.me/mrwicks00) 

---

<div align="center">

**Built in public. For the world. By the community.**

*If you believe payments should work everywhere, star this repo and join us.*

⭐ **Star us on GitHub** • 🍴 **Fork a repo** • 🐛 **Open an issue** • 📖 **Improve the docs**

</div>
