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

**Krabby** is a production-grade, open-source suite designed for engineering teams who need full control over their real-time communication infrastructure. Built from the ground up with **Rust**, Krabby provides the low-level primitives necessary to build multi-media platforms that rival the performance and reliability of industry giants like WhatsApp, Zoom, and Signal - without the constraints of proprietary vendor lock-in.

Whether you are building high-concurrency chat systems, sub-millisecond audio/video calling, or large-scale presence engines, Krabby offers a decoupled, microservice-oriented architecture that scales horizontally and executes with the safety and speed of Rust.

---

## Documentation

This section provides technical documentation for the entire Krabby project. It details our architectural philosophy, provides a general overview into the Krabby ecosystem, and includes the key information needed to help you settle quickly into intergrating our services.

### Table Of Content

1. [Design Philosophy](#design-philosophy)

2. [WebRTC Glossary](#webrtc-glossary)

3. [Our Roadmap](#our-roadmap)

4. [Our Core Rust Stack](#our-core-rust-stack)

5. [Architectural Overview](#architectural-overview)

6. [Development & Open Source Standards](#development--open-source-standards)

7. [Getting Involved](#getting-involved)

8. [FAQs](#faqs)

### Design Philosophy

All Krabby services are intentionally built to be **highly modular and minimal**. This approach prevents bloat, eases developer onboarding, and enhances the flexibility/extensibility of the entire suite.

- **Modular Microservices:** We decouple Identity, Logic, Real-time Data, and Signaling into specialized Rust binaries.

- **Architectural Simplicity:** We consciously avoid overly complex backend patterns(like heavy Event-Driven Architectures) in the core primitives. This allows teams of all sizes to adapt the base easily.

> This further explains why all Krabby backend services are engineered to communicate with a single PostgreSQL database, providing a simple and stable foundation for teams that later wish to scale into more complex data architectures.

### WebRTC Glossary

Krabby uses **WebRTC(Web Real-Time Communication)** to enable real-time audio, video, and data exchange between clients. While much of the complexity is abstracted away, understanding the core terminology will help when integrating, debugging, or extending the system.

#### Peer Connection(`RTCPeerConnection`)

The **Peer Connection** is the core WebRTC interface responsible for managing communication between two peers.

It handles:

- ICE candidate gathering and connectivity checks
- Media stream transmission (audio/video)
- Data channels
- Encryption and security

Most WebRTC integrations revolve around configuring and managing a **`RTCPeerConnection`** instance.

#### ICE(Interactive Connectivity Establishment)

**ICE** is the framework used to discover the best network path between two peers.

Each peer gathers **ICE candidates** (possible connection routes) and exchanges them with the other peer. These candidates may represent:

- Direct **peer-to-peer connections**
- Paths discovered via **STUN**
- Relayed connections through **TURN**

ICE then tests these candidates and selects the most reliable route.

#### NAT(Network Address Translation)

Most devices operate behind **NAT routers**, which map private local IP addresses to a public IP.

While necessary for modern networking, NAT can block direct peer-to-peer connections. WebRTC works around this using **ICE, STUN, and TURN**.

#### SDP(Session Description Protocol)

**SDP** is a standardized format used to describe a WebRTC session.

It includes parameters such as:

- Media types (audio, video, data)
- Supported codecs
- Encryption details
- Network information

SDPs are exchanged during connection negotiation.

#### Offer/Answer Model

WebRTC uses an **Offer/Answer negotiation pattern**:

1. One peer generates an **Offer** containing an SDP describing its capabilities.
2. The other peer responds with an **Answer** describing the compatible configuration.

This exchange establishes the session parameters.

#### Signaling

**Signaling** is the process used to exchange connection metadata before a WebRTC connection is established.

This typically includes:

- SDP **offers and answers**
- **ICE candidates**

WebRTC does not define a signaling protocol. In the Krabby ecosystem, this role is handled by the **`rtc_signalling` service**.

#### Data Channel(`RTCDataChannel`)

A **Data Channel** enables peers to exchange arbitrary data directly over the WebRTC connection.

Typical uses include:

- Chat messaging
- File transfer
- Real-time application state

Data channels are **low-latency, encrypted, and peer-to-peer**.

#### SFU(Selective Forwarding Unit)

An **SFU** is a server architecture used in multi-participant calls.

Instead of mixing streams, an SFU:

- Receives media streams from participants
- Selectively forwards them to other participants

This reduces bandwidth and CPU requirements compared to mesh networking.

#### STUN and TURN

These services help establish connections across NATs and firewalls.

**STUN(Session Traversal Utilities for NAT)**

Allows a client to discover its **public IP and port**, enabling direct peer-to-peer connectivity.

**TURN(Traversal Using Relays around NAT)**

Acts as a **relay server** when direct connectivity fails due to strict NAT or firewall restrictions.

This set of concepts covers the **core building blocks involved in establishing and maintaining WebRTC connections within the Krabby ecosystem.**

### Our Roadmap

Once fully stable, Krabby will be available for use in three distinct modes:

1.  **Raw APIs:** For direct integrations by teams that want maximum control. You host the backend primitives; Krabby provides the code.

2.  **SDKs(Most Recommended):** Optimized for speed of integration. These provide high-level wrappers around our raw APIs for Web, Mobile, and Desktop, while you still maintain ownership of the self-hosted backend.

3.  **Buck(by Krabby):** Krabby's SaaS(Software-as-a-Service) solution. **Buck** allows engineering teams to completely bypass the hassle involved with self-hosting. Upon launch, teams can simply subscribe to our enterprise cloud and get access to managed Krabby APIs and premium SDKs instantly.

### Our Core Rust Stack.

Krabby is built using modern, high-performance Rust ecosystem tooling that are designed for reliability, scalability, and strong type safety. The stack emphasizes asynchronous execution, efficient resource usage, and production-grade security while remaining modular enough to support multiple specialized services such as authentication, core APIs, real-time messaging, and RTC signaling.

The technologies listed below form the foundation of the system. They handle everything from request routing and asynchronous execution to data persistence, authentication, configuration management, and infrastructure integration.

#### Framework & Async Runtime

- **[Axum](https://github.com/tokio-rs/axum)**

  Primary web framework used across all services (`auth`, `core`, `websocket`, and `signaling`). Provides modular, type-safe routing built on top of `tower` and `hyper`.

- **[Tokio](https://tokio.rs/)**

  Asynchronous runtime powering the system’s high-concurrency workloads, particularly the WebSocket and signaling services.

#### Persistence & Data

- **[SQLx](https://github.com/launchbadge/sqlx)**

  Asynchronous SQL toolkit used with **PostgreSQL**, featuring compile-time query validation and a built-in migration system.

- **[PostgreSQL](https://www.postgresql.org/)**

  Primary relational database engine used for all stateful services.

#### Security & Identity

- **[Argon2](https://github.com/RustCrypto/password-hashes/tree/master/argon2)**

  Production-grade, side-channel–resistant password hashing used in the `auth_server`.

- **[JSON Web Tokens (JWT)](https://github.com/Keats/jsonwebtoken)**

  Stateless authentication mechanism using access and refresh token pairs for session management.

#### Real-Time & Signaling

- **[WebSockets](https://docs.rs/axum/latest/axum/extract/ws/index.html)**

  Integrated via Axum to support live messaging, presence tracking, and RTC signal relay.

- **WebRTC Primitives**

  Custom signaling logic in `rtc_signalling` responsible for SDP(offer/answer) negotiation and ICE candidate exchange.

#### Configuration & Environment

- **[config-rs](https://github.com/mehcode/config-rs)**

  Layered configuration system that merges:
  - `base.toml`
  - environment-specific configs (e.g., `development`, `production`)
  - `APP__`-prefixed environment variables

- **[Dotenvy](https://github.com/allan2/dotenvy)**

  Loads environment variables from `.env` files during local development.

#### Observability & Logging

- **[Tracing](https://github.com/tokio-rs/tracing)**

  Structured logging framework used to track request lifecycles and system events.

- **[tracing-subscriber](https://docs.rs/tracing-subscriber)**

  Configured to emit JSON logs suitable for production log aggregation systems.

#### Infrastructure Integration

- **[AWS SDK for Rust](https://github.com/awslabs/aws-sdk-rust)**

  The AWS S3 SDK - for handling file storage.

#### Key Utilities

- **[Serde](https://serde.rs/)**

  Framework for serializing and deserializing Rust data structures such as JSON and TOML.

- **[Chrono](https://github.com/chronotope/chrono)**

  Library for robust date and time handling.

- **[UUID](https://github.com/uuid-rs/uuid)**

  Generates unique identifiers across distributed services.

### Architectural Overview

The Krabby project is structured as a suite of specialized services, each responsible for a specific domain of the communication lifecycle.

#### Identity & Access Management([`chat__auth_server`](https://github.com/KrabbyHQ/chat__auth_server))

The gateway to the Krabby ecosystem. This service handles user onboarding and secures all other services.

- **Responsibilities:** Registration, Login/Logout, JWT token management (Access & Refresh), and Argon2 password hashing.

#### Business Logic & Persistence([`chat__core_rest_api_server`](https://github.com/KrabbyHQ/chat__core_rest_api_server))

The primary engine for stateful operations, designed for teams that need robust social features beyond just calling.

- **Responsibilities:** Management of users, rooms (Private/Group), message history, and admin controls.
- **Features:** Bookmarking, pinning, archiving, and edit-history tracking.

#### Real-time Streaming([`chat__web_socket_server`](https://github.com/KrabbyHQ/chat__web_socket_server))

A high-concurrency stateful server designed for live data delivery.

- **Responsibilities:** Message broadcasting, real-time presence (online/offline), typing indicators, and syncing with the core REST API.

#### Call Negotiation([`rtc_signalling`](https://github.com/KrabbyHQ/rtc_signalling))

The broker for peer-to-peer multimedia sessions.

- **Responsibilities:** Low-latency relay of SDP Offer/Answer exchanges and ICE candidates.
- **Focus:** Focused on sub-millisecond signal delivery to ensure rapid call establishment.

#### Multi-Platform Clients([`demo_clients`](https://github.com/KrabbyHQ/demo_clients))

Reference implementations showcasing best-practice API integrations.

- **Web:** A high-fidelity Next.js 16 platform built to production-grade standards, with state-of-art web engineering tooling.
- **Mobile:** A cross-platform Expo/React Native application built to provide a matching experience for mobile platforms.

#### STUN/TURN Infrastructure

For robust connectivity, all related Krabby services will integrate the popular open-source **[Coturn](https://github.com/coturn/coturn)** project to provide production-grade relay services.

### Development & Open Source Standards

Krabby is built to world-class standards, ensuring that the project remains maintainable, easy to use/integrate, secure, and easy to scale.

- **Standardized Commits:** We enforce [Conventional Commits](https://conventionalcommits.org) using `commitlint` to maintain a readable and automated changelog.

- **Automated Quality Assurance:** Every project utilizes `Husky` git hooks. We run local compilation checks and static analysis(`cargo fmt`, `clippy`, `prettier`, `eslint` - as the case may be) before any code is pushed. Our github CI workflow on each project, further helps to ensure all deployments meet the required standards.

- **Unified Configuration:** All backend services use a hierarchical configuration system(`config/base.toml` -> `env.toml` -> `local.toml`) combined with `APP__` environment variable overrides. This helps us maintain uniform config standards across board in the most professional way possible.

- **Type Safety:** We maintain a centralized **[Data Modeling Reference](https://github.com/KrabbyHQ/demo_clients/blob/main/docs/reference/data_model/schema.sql)** to ensure consistency between Rust backends and TypeScript(or similar) frontends.

### Getting Involved

Krabby is built by the community, for the community. We welcome contributions ranging from core Rust performance optimizations to high-fidelity frontend UIs, and documentation.

1.  **Explore the Code:** Dive into the respective service directories to read their localized documentation.

2.  **Follow the Standards:** Ensure your environment is set up with `husky` to trigger the automated contribution checks locally.

3.  **Build the Future:** Help us define the next generation of real-time communication infrastructure.

### FAQs

Below are answers to some common questions about Krabby, its goals, and how it is intended to be used. If you are new to the project, this section should help clarify expectations around usage, licensing, and the level of control Krabby is designed to provide.

---

### What problem does Krabby solve?

Krabby is designed to help developers **build and control their own real-time communication infrastructure**. Instead of relying entirely on third-party RTC platforms, Krabby enables teams to run their own services for:

- Real-time messaging

- WebRTC signaling

- Presence and event systems

- Authentication and session management

This approach gives teams **greater flexibility, ownership, and control over their real-time systems**.

---

### Must I know Rust to be able to use Krabby?

While we aim to make Krabby as accessible as possible, the core philosophy of the project is to give developers **full control over their real-time communication infrastructure**. This level of control typically requires the ability to self-host and adapt the services that power your system.

Because Krabby services are written in Rust, you will need **some working knowledge of Rust** — at least enough to understand, modify, and deploy your own customized Krabby services.

If you don't know Rust and simply want to integrate SDKs and ship faster, you can instead use **[Buck(by Krabby)](#our-roadmap)**, which provides a more accessible integration path.

---

### Is Krabby free to use?

Yes. Upon the first stable release, **all Krabby APIs and SDKs will be publicly available** under the **MIT License**.

This means you can freely use, modify, self-host, and integrate Krabby into your own applications or infrastructure.

---

### Can Krabby be used in production?

Yes. Krabby is being designed with **production-grade architecture** in mind, including strong typing, async concurrency, structured logging, and secure authentication patterns.

However, until the **first stable release**, APIs and internal components may still evolve.

---

### Do I need to self-host Krabby?

Krabby is built primarily as a **self-hosted infrastructure toolkit**. This design ensures that developers and organizations maintain full ownership of their communication stack, performance characteristics, and data.

Self-hosting also allows teams to customize services, optimize deployments, and scale according to their specific needs.

If you wish to avoid the hassle of self-hosting and move straight to shipping with premium Krabby APIs/SDKs, checkout **[Buck(by Krabby)](#our-roadmap)**.

---

<p align="center">
Built with precision and ❤️ by the <strong>Krabby</strong> Engineering Team.
</p>
