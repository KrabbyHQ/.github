<div align="center">
  <img src="https://raw.githubusercontent.com/KrabbyHQ/.github/main/assets/krabby-banner.png" alt="Krabby Banner" width="100%">
  
  <br />
  <br />
  
  <h1>Krabby.</h1>
  
  <p><strong>Blazing-fast developer tools for building high-performance real-time communication systems.</strong></p>
  
  <p>
    <a href="https://github.com/KrabbyHQ"><img src="https://img.shields.io/badge/Rust-000000?style=for-the-badge&logo=rust&logoColor=white" alt="Rust" /></a>
    <img src="https://img.shields.io/badge/License-MIT-blue?style=for-the-badge" alt="License" />
    <img src="https://img.shields.io/badge/Status-Pre--release-orange?style=for-the-badge" alt="Status" />
  </p>
</div>

---

## About Krabby

**Krabby** is a production-grade, open-source suite designed for engineering teams who need full control over their real-time communication infrastructure. Built from the ground up with **Rust**, Krabby provides the low-level primitives necessary to build multi-media platforms that rival the performance and reliability of industry giants like WhatsApp, Zoom, and Signal — without the constraints of proprietary vendor lock-in.

Whether you are building high-concurrency chat systems, sub-millisecond audio/video calling, or large-scale presence engines, Krabby offers a decoupled, microservice-oriented architecture that scales horizontally and executes with the safety and speed of Rust.

---

## Documentation

This section provides technical documentation that covers the entire Krabby ecosystem, detailing our architectural philosophy and the underlying protocols that power modern real-time systems.

### Table Of Content

1. [Design Philosophy](#design-philosophy)
2. [WebRTC Glossary (Important Terms to Know)](#webrtc-glossary-important-terms-to-know)
3. [Roadmap](#roadmap)
4. [Architectural Overview](#architectural-overview)
5. [Development & Open Source Standards](#development--open-source-standards)
6. [Getting Involved](#getting-involved)

### Design Philosophy

All Krabby services are intentionally built to be **highly modular and minimal**. This approach prevents bloat, eases developer onboarding, and enhances the flexibility/extensibility of the entire suite.

- **Modular Microservices:** We decouple Identity, Logic, Real-time Data, and Signaling into specialized Rust binaries.
- **Architectural Simplicity:** We consciously avoid overly complex backend patterns (like heavy Event-Driven Architectures) in the core primitives. This allows teams of all sizes to adapt the base easily.

> This further explains why all Krabby backend services are engineered to communicate with a single PostgreSQL database, providing a simple and stable foundation for teams that later wish to scale into more complex data architectures.

### WebRTC Glossary (Important Terms to Know)

Krabby uses **WebRTC (Web Real-Time Communication)** to enable real-time audio, video, and data exchange between clients. While much of the complexity is abstracted away, understanding the core terminology will help when integrating, debugging, or extending the system.

#### Peer Connection (`RTCPeerConnection`)

The **Peer Connection** is the core WebRTC interface responsible for managing communication between two peers. It handles ICE candidate gathering, media stream transmission, and encryption.

#### ICE (Interactive Connectivity Establishment)

**ICE** is the framework used to discover the best network path between two peers. Each peer gathers **ICE candidates** and exchanges them to test the most reliable route (P2P, STUN, or TURN).

#### NAT (Network Address Translation)

Most devices operate behind **NAT routers**. WebRTC uses **ICE, STUN, and TURN** to establish connections through these routers.

#### SDP (Session Description Protocol)

**SDP** is a standardized format used to describe a WebRTC session, including media types, codecs, encryption, and network information.

#### Offer/Answer Model

WebRTC negotiation follows an **Offer/Answer pattern** where one peer generates an SDP Offer and the other responds with an Answer to establish session parameters.

#### Signaling

The process of exchanging connection metadata (SDP and ICE candidates) before establishing a connection. In Krabby, this is handled by the **[`rtc_signalling`](https://github.com/KrabbyHQ/rtc_signalling) service**.

#### Data Channel (`RTCDataChannel`)

Enables peers to exchange arbitrary data (chat, files, state) directly over an encrypted, low-latency WebRTC connection.

#### SFU (Selective Forwarding Unit)

A server architecture for multi-participant calls that receives media streams and selectively forwards them to others, optimizing bandwidth and CPU.

#### STUN and TURN

**STUN** allows a client to discover its public IP, while **TURN** acts as a relay server when direct connectivity is restricted by firewalls.

---

### Roadmap

Once fully stable, Krabby will be available for use in three distinct modes:

1.  **Raw APIs:** For direct integrations by teams that want maximum control. You host the backend primitives; Krabby provides the code.
2.  **SDKs (Most Recommended):** High-level wrappers for Web, Mobile, and Desktop to optimize integration speed while maintaining self-hosted ownership.
3.  **Buck (by Krabby):** Krabby's SaaS solution. **Buck** allows teams to bypass self-hosting by subscribing to our enterprise cloud for managed APIs and premium SDKs.

---

### Architectural Overview

The Krabby project is structured as a suite of specialized services, each responsible for a specific domain of the communication lifecycle.

#### Identity & Access Management ([`chat__auth_server`](https://github.com/KrabbyHQ/chat__auth_server))

The gateway to the Krabby ecosystem. Handles user onboarding, JWT token management, and secure password hashing.

- **Tech Stack:** Axum, SQLx, PostgreSQL.

#### Business Logic & Persistence ([`chat__core_rest_api_server`](https://github.com/KrabbyHQ/chat__core_rest_api_server))

The primary engine for stateful operations (users, rooms, message history) with features like bookmarking, pinning, and archiving.

- **Tech Stack:** Axum, SQLx, AWS S3.

#### Real-time Streaming ([`chat__web_socket_server`](https://github.com/KrabbyHQ/chat__web_socket_server))

A high-concurrency stateful server for live data delivery, message broadcasting, and real-time presence.

- **Tech Stack:** Tokio (Async I/O), Axum WebSockets.

#### Call Negotiation ([`rtc_signalling`](https://github.com/KrabbyHQ/rtc_signalling))

The broker for peer-to-peer multimedia sessions, providing low-latency relay of SDPs and ICE candidates.

- **Focus:** Sub-millisecond signal delivery for rapid call establishment.

#### Multi-Platform Clients ([`demo_clients`](https://github.com/KrabbyHQ/demo_clients))

Reference implementations showcasing best-practice API integrations for Web (Next.js) and Mobile (React Native).

#### STUN/TURN Infrastructure

Krabby integrates seamlessly with the open-source **[Coturn](https://github.com/coturn/coturn)** project to provide production-grade relay services.

---

### Development & Open Source Standards

Krabby maintains world-class standards for maintainability, security, and scalability.

- **Standardized Commits:** We enforce [Conventional Commits](https://conventionalcommits.org) using `commitlint`.
- **Automated Quality Assurance:** Every project utilizes `Husky` git hooks for local formatting, linting, and static analysis.
- **Unified Configuration:** Services share a hierarchical configuration system (`base.toml` -> `env.toml` -> `local.toml`) with environment variable overrides.
- **Type Safety:** We maintain a centralized **[Data Modeling Reference](https://github.com/KrabbyHQ/demo_clients/blob/main/docs/reference/data_model/schema.sql)** to ensure consistency across the stack.

### Getting Involved

We welcome contributions ranging from core Rust optimizations to high-fidelity UI components.

1.  **Explore the Code:** Dive into the service directories linked above.
2.  **Follow the Standards:** Set up your environment with `bun` and `rustup` for automated checks.
3.  **Build the Future:** Help us define the next generation of real-time communication infrastructure.

<br />

<p align="center">
Built with precision and ❤️ by the <strong>Krabby</strong> Engineering Team.
</p>
