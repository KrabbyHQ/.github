<div align="center">
  <img src="https://raw.githubusercontent.com/KrabbyHQ/.github/main/assets/krabby-banner.png" alt="Krabby Banner" width="100%">
  
  <br />
  <br />
  
  <h1>Krabby.</h1>
  
  <p><strong>Blazing-fast developer tools for building high-performance real-time communication systems.</strong></p>
  
  <p>
    <a href="https://github.com/KrabbyHQ"><img src="https://img.shields.io/badge/Rust-000000?style=for-the-badge&logo=rust&logoColor=white" alt="Rust" /></a>
    <img src="https://img.shields.io/badge/License-MIT-blue?style=for-the-badge" alt="License" />
    <img src="https://img.shields.io/badge/Status-Active-green?style=for-the-badge" alt="Status" />
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

1. [Design Philosophy](#-design-philosophy)

2. [WebRTC Glossary(Important Terms to Know)](#webrtc-glossary-important-terms-to-know)

3. [Roadmap](#-roadmap)

4. [Architectural Overview](#-architectural-overview)

5. [Development & Open Source Standards](#-development--open-source-standards)

6. [Getting Involved](#-getting-involved)

### Design Philosophy

All Krabby services are intentionally built to be **highly modular and minimal**. This approach prevents bloat, eases developer onboarding, and enhances the flexibility/extensibility of the entire suite.

*   **Modular Microservices:** We decouple Identity, Logic, Real-time Data, and Signaling into specialized Rust binaries.

*   **Architectural Simplicity:** We consciously avoid overly complex backend patterns(like heavy Event-Driven Architectures) in the core primitives. This allows teams of all sizes to adapt the base easily. 

> This further explains why all Krabby backend services are engineered to communicate with a single PostgreSQL database, providing a simple and stable foundation for teams that later wish to scale into more complex data architectures.

### WebRTC Glossary(Important Terms to Know)

Krabby uses **WebRTC(Web Real-Time Communication)** to enable real-time audio, video, and data exchange between clients. While much of the complexity is abstracted away, understanding the core terminology will help when integrating, debugging, or extending the system.

#### Peer Connection(`RTCPeerConnection`)

The **Peer Connection** is the core WebRTC interface responsible for managing communication between two peers.

It handles:

* ICE candidate gathering and connectivity checks
* Media stream transmission (audio/video)
* Data channels
* Encryption and security

Most WebRTC integrations revolve around configuring and managing a **`RTCPeerConnection`** instance.

#### ICE(Interactive Connectivity Establishment)

**ICE** is the framework used to discover the best network path between two peers.

Each peer gathers **ICE candidates** (possible connection routes) and exchanges them with the other peer. These candidates may represent:

* Direct **peer-to-peer connections**
* Paths discovered via **STUN**
* Relayed connections through **TURN**

ICE then tests these candidates and selects the most reliable route.

#### NAT(Network Address Translation)

Most devices operate behind **NAT routers**, which map private local IP addresses to a public IP.

While necessary for modern networking, NAT can block direct peer-to-peer connections. WebRTC works around this using **ICE, STUN, and TURN**.

#### SDP(Session Description Protocol)

**SDP** is a standardized format used to describe a WebRTC session.

It includes parameters such as:

* Media types (audio, video, data)
* Supported codecs
* Encryption details
* Network information

SDPs are exchanged during connection negotiation.

#### Offer/Answer Model

WebRTC uses an **Offer/Answer negotiation pattern**:

1. One peer generates an **Offer** containing an SDP describing its capabilities.
2. The other peer responds with an **Answer** describing the compatible configuration.

This exchange establishes the session parameters.

#### Signaling

**Signaling** is the process used to exchange connection metadata before a WebRTC connection is established.

This typically includes:

* SDP **offers and answers**
* **ICE candidates**

WebRTC does not define a signaling protocol. In the Krabby ecosystem, this role is handled by the **`rtc_signalling` service**.

#### Data Channel(`RTCDataChannel`)

A **Data Channel** enables peers to exchange arbitrary data directly over the WebRTC connection.

Typical uses include:

* Chat messaging
* File transfer
* Real-time application state

Data channels are **low-latency, encrypted, and peer-to-peer**.

#### SFU(Selective Forwarding Unit)

An **SFU** is a server architecture used in multi-participant calls.

Instead of mixing streams, an SFU:

* Receives media streams from participants
* Selectively forwards them to other participants

This reduces bandwidth and CPU requirements compared to mesh networking.

#### STUN and TURN

These services help establish connections across NATs and firewalls.

**STUN(Session Traversal Utilities for NAT)**
Allows a client to discover its **public IP and port**, enabling direct peer-to-peer connectivity.

**TURN(Traversal Using Relays around NAT)**
Acts as a **relay server** when direct connectivity fails due to strict NAT or firewall restrictions.

This set of concepts covers the **core building blocks involved in establishing and maintaining WebRTC connections within the Krabby ecosystem.**

### Roadmap

Once fully stable, Krabby will be available for use in three distinct modes:

1.  **Raw APIs:** For direct integrations by teams that want maximum control. You host the backend primitives; Krabby provides the code.

2.  **SDKs(Most Recommended):** Optimized for speed of integration. These provide high-level wrappers around our raw APIs for Web, Mobile, and Desktop, while you maintain ownership of the self-hosted backend.

3.  **Buck(by Krabby):** Krabby's SaaS(Software-as-a-Service) solution. **Buck** allows engineering teams to completely bypass the hassle involved with self-hosting. Upon launch, teams can simply subscribe to our enterprise cloud and get access to managed Krabby APIs and premium SDKs instantly.

### Architectural Overview

The Krabby project is structured as a suite of specialized services, each responsible for a specific domain of the communication lifecycle.

#### Identity & Access Management(`chat__auth_server`)

The gateway to the Krabby ecosystem. This service handles user onboarding and secures all other services.

*   **Responsibilities:** Registration, Login/Logout, JWT token management (Access & Refresh), and Argon2 password hashing.
*   **Tech Stack:** Axum, SQLx, PostgreSQL.

#### Business Logic & Persistence(`chat__core_rest_api_server`)

The primary engine for stateful operations, designed for teams that need robust social features beyond just calling.

*   **Responsibilities:** Management of users, rooms (Private/Group), message history, and admin controls.
*   **Features:** Bookmarking, pinning, archiving, and edit-history tracking.
*   **Tech Stack:** Axum, SQLx, AWS S3.

#### Real-time Streaming(`chat__web_socket_server`)

A high-concurrency stateful server designed for live data delivery.

*   **Responsibilities:** Message broadcasting, real-time presence (online/offline), typing indicators, and syncing with the core REST API.
*   **Tech Stack:** Tokio (Async I/O), Axum WebSockets.

#### Call Negotiation(`rtc_signalling`)

The broker for peer-to-peer multimedia sessions.

*   **Responsibilities:** Low-latency relay of SDP Offer/Answer exchanges and ICE candidates.
*   **Focus:** Focused on sub-millisecond signal delivery to ensure rapid call establishment.

#### Multi-Platform Clients(`demo_clients`)

Reference implementations showcasing best-practice API integrations.

*   **Web:** A high-fidelity Next.js 16 platform utilizing Redux Toolkit.
*   **Mobile:** A cross-platform Expo/React Native application duplicating the desktop-grade experience.

#### STUN/TURN Infrastructure

For robust connectivity, Krabby integrates seamlessly with the popular open-source **[Coturn](https://github.com/coturn/coturn)** project to provide production-grade relay services.

### Development & Open Source Standards

Krabby is built to world-class standards, ensuring that the project remains maintainable, easy to use/integrate, secure, and easy to scale.

*   **Standardized Commits:** We enforce [Conventional Commits](https://conventionalcommits.org) using `commitlint` to maintain a readable and automated changelog.

*   **Automated Quality Assurance:** Every project utilizes `Husky` git hooks. We run local compilation checks and static analysis (`cargo fmt`, `clippy`, `prettier`, `eslint`) before any code is pushed.

*   **Unified Configuration:** All backend services use a hierarchical configuration system (`config/base.toml` -> `env.toml` -> `local.toml`) combined with `APP__` environment variable overrides.

*   **Type Safety:** We maintain a centralized **[Data Modeling Reference](https://github.com/KrabbyHQ/demo_clients/blob/main/docs/reference/data_model/schema.sql)** to ensure consistency between Rust backends and TypeScript or similar frontends.

### Getting Involved

Krabby is built by the community, for the community. We welcome contributions ranging from core Rust performance optimizations to high-fidelity frontend UI components.

1.  **Explore the Code:** Dive into the respective service directories to read their localized documentation.

2.  **Follow the Standards:** Ensure your environment is set up with `bun` and `rustup` to trigger the automated contribution checks.

3.  **Build the Future:** Help us define the next generation of real-time communication infrastructure.

<br />

<p align="center">
Built with precision and ❤️ by the <strong>Krabby</strong> Engineering Team.
</p>
