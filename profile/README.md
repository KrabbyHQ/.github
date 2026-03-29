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

## 🦀 About Krabby

**Krabby** is a production-grade, open-source suite designed for engineering teams who need full control over their real-time communication infrastructure. Built from the ground up with **Rust**, Krabby provides the low-level primitives necessary to build multi-media platforms that rival the performance and reliability of industry giants like WhatsApp, Zoom, and Signal—without the constraints of proprietary vendor lock-in.

Whether you are building high-concurrency chat systems, sub-millisecond audio/video calling, or large-scale presence engines, Krabby offers a decoupled, microservice-oriented architecture that scales horizontally and executes with the safety and speed of Rust.

---

## 📖 Project Directory Study & Documentation

This section provides a high-level architectural overview and technical documentation of the Krabby ecosystem, serving as a roadmap for contributors and engineers.

### 🏛️ Ecosystem Architecture

The project is structured as a suite of specialized services, each responsible for a specific domain of the communication lifecycle.

#### 🛡️ Identity & Access Management (`chat__auth_server`)
The gateway to the Krabby ecosystem. This service handles user onboarding and security.
*   **Key Responsibilities:** Registration, Login/Logout, JWT token generation (Access & Refresh), and secure password hashing.
*   **Tech Stack:** Axum, SQLx, PostgreSQL, Argon2.
*   **Pattern:** Follows a strict query-param based logout pattern for cross-client reliability.

#### ⚙️ Business Logic & Persistence (`chat__core_rest_api_server`)
The primary engine for stateful operations.
*   **Key Responsibilities:** Domain-driven management of users, rooms (Private/Group), message history, and administrative controls.
*   **Storage:** PostgreSQL for metadata and AWS S3 for multimedia attachments/profiles.
*   **Features:** Bookmarking, pinning, archiving, and edit-history tracking.

#### ⚡ Real-time Streaming (`chat__web_socket_server`)
A high-concurrency stateful server designed for live data delivery.
*   **Key Responsibilities:** Message broadcasting, real-time presence (online/offline status), and typing indicators.
*   **Infrastructure:** Built on `tokio` for non-blocking I/O, capable of handling thousands of concurrent socket connections.

#### 📞 Call Negotiation (`rtc_signalling`)
The broker for peer-to-peer multimedia sessions.
*   **Key Responsibilities:** WebRTC signalling (SDP Offer/Answer exchange) and ICE candidate relaying.
*   **Focus:** Focused on low-latency signaling to ensure rapid call establishment for Audio and Video.

#### 📱 Multi-Platform Clients (`demo_clients`)
Reference implementations showcasing the integration of Krabby APIs.
*   **Web (`nextjs__raw_krabby_chat_api_integrations`):** A high-fidelity Next.js 16 platform utilizing Redux Toolkit and Axios for robust state management.
*   **Mobile (`react_native__raw_krabby_chat_api_integrations`):** A cross-platform Expo/React Native application duplicating the web experience with optimized mobile UI/UX.

---

### 🛠️ Development & Open Source Standards

Krabby is built to world-class standards, ensuring that the project remains maintainable, secure, and easy to contribute to.

*   **Standardized Commits:** We enforce [Conventional Commits](https://conventionalcommits.org) using `commitlint` to maintain a readable and automated changelog.
*   **Automated Quality Assurance:** Every sub-project utilizes `Husky` git hooks. We run `cargo fmt`, `clippy`, and compilation checks before any code reaches the remote repository.
*   **Unified Configuration:** All backend services use a hierarchical configuration system (`config/base.toml` -> `env.toml` -> `local.toml`) combined with `APP__` environment variable overrides.
*   **Type Safety:** We maintain a centralized [Data Modeling Reference](https://github.com/KrabbyHQ/demo_clients/blob/main/docs/reference/data_model/schema.sql) to ensure consistency across Rust backends and TypeScript frontends.

---

### 🚀 Getting Involved

Krabby is built by the community, for the community. We welcome contributions ranging from core Rust performance optimizations to high-fidelity frontend UI components.

1.  **Explore the Code:** Dive into the respective service directories to read their localized documentation.
2.  **Follow the Standards:** Ensure your environment is set up with `bun` and `rustup` to trigger the automated contribution checks.
3.  **Build the Future:** Help us define the next generation of open-source communication.

<br />

<p align="center">
Built with precision and ❤️ by the <strong>Krabby</strong> Engineering Team.
</p>