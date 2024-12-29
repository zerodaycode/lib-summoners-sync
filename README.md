# 🛡️ Summoners Sync Core Library

[🔗 Overview](#-overview) • [🔗 Purpose](#-purpose) • [🔗 Key Features](#-key-features) • [🔗 Architecture](#-architecture) • [🔗 Integration](#-integration) • [🔗 Security Considerations](#-security-considerations) • [🔗 Contributing](#-contributing)

---

## 🌟 Overview

Welcome to the **Summoners Sync Core Library**, the heart of the **Summoners Sync Project**.  
This Rust-based library provides essential functionality for secure communication and configuration propagation
across microservices, showcasing cutting-edge technology and architecture.

We built this library not only to solve common challenges in the **Summoners Sync** ecosystem but also to demonstrate
our expertise in crafting **high-performance, secure, and reusable components**.

The library fits seamlessly into our multi-language microservices setup, offering bindings for
**Python**, **Java**, **C++**, **Go** and potentially others!

---

## 🎯 Purpose

The **Summoners Sync Core Library** is not just another library—it's the backbone of our project. Here’s why:

1. **JWT Management**: Generate and validate JSON Web Tokens (JWTs) to ensure secure, role-based communication between services.
2. **Multi-Language Support**: Export bindings for different programming languages, enabling easy integration regardless of your stack.
3. **Future-Proof Design**: Built with a modular approach, the library is optimized for growth and maintainability.

At its core, this library represents our dedication to modern, secure, and scalable software development practices.

---

## 🚀 Key Features

### 🌐 **Multi-Language Bindings**

- Python: Seamless integration using PyO3.
- Java: Native support with JNI.
- C++: High-performance bindings using CXX.
- Go: Straightforward integration for Go applications.

### 🔒 **Security Management**

Most of our authentication workflows are `JWT` stateless based. By embeeding all the logic of the generation and validation
in this library, we ensure to have the same generation and validation procedure across all the artifacts that needs auth process.

For example, our `auth` server, written in Rust, will call this library to generate the tokens. But our `api-gateway`, written in
**Java** with ***Spring-Cloud** will validate the tokens by calling via `FFI` the token validation function from **Java**, saving
*REST* communnications between artifacts for just check if a given token for a user in a request is valid.

- Securely create JWT tokens with claims like roles and expirations.
- Validate tokens to verify their authenticity.
- Generate and extract claims. This last part will help to unify the `RBOC` downstreamed to other microservices.

### ⚙️ **Literals Standarization**

- Standardize literal constant values for consistent usage in key critic values,
for example on the propagation of user roles (e.g., `X-Role`).

### 📦 **Build-Time Optimization**

- Use `cfg` features to include only the necessary bindings, generating lightweight libraries.

Thanks to `Rust` powerful *conditional compilation* features, we can publish *dynamic libraries* for
all the target languages one by one. That means, that the calling artifacts will only load at runtime
a library that only contains the bindings generated for their language. For example, the `C++` build
will only be generated with the `C++` bindings, and later the user can fetch it via `Zork++` or `CMake`
and use it directly in their code, without polluting their build folders with other dependencies and build
artifacts for the other languages.

---

## 🛠️ Architecture

The library's architecture is designed for modularity and extensibility:

### Key Design Principles (a brief summary)

    - Separation of Concerns: Core logic and bindings are decoupled.
    - Rust at the Core: The heart of the library is written in Rust for its performance and safety.
    - Conditional Compilation: Only the required bindings are included during the build.

### Overall architecture

```plaintext
+--------------------------+
|  Summoners Sync Core              |
|  (Rust Core Logic)                |
|                                   |
|  - Standarization of literals     |
|  - Fast and efficient Algorhythms |
|  - Role Header Config             |
|  - JWT Creation                   |
|  - JWT Validation                 |
|  - OAuth2 integrations            |
+-----------------+-----------------+
            |
+-----------v--------------+
|   Language Bindings      |
+-----------+--------------+
            |
+-----------v-------------------------------+
|        Target Languages                   |
|                                           |
| Python | Java | C++ | Go | etc.           |
+-------------------------------------------+
```

## 🌍 How It Fits Into the Project

Here's how the library integrates in our ecosystem:

- API Gateway:
  - Validates JWTs at the entry point.
  - Propagates role-based headers to downstream services.

- Authentication Service:
  - Uses the library to generate secure JWTs for client applications.

- Downstream Services:
  - Trust the propagated headers to enforce role-based access control.

- General literals:
  - Exports static and constant values that are shared in multiple places. This ensures
    that we're not n-plicating several times the same values across different applications,
    so their definitions remains constant across all of our services and applications, potentiallly
    reducing hard to debug issues depending on the context.

This ensures a cohesive, secure, and efficient communication pipeline across our microservices.

## 📦 Integration

### Java (Maven)

Add the following dependency to your pom.xml:

```xml
    <dependency>
    <groupId>com.github.your-org</groupId>
    <artifactId>summoners-sync-core</artifactId>
    <version>1.0.0</version>
    </dependency>
```

### Python

Install via pip:

```bash
    pip install summoners-sync-core
```

### C++

Use CMake to fetch and link the library:

```cmake
    find_package(SummonersSyncCore REQUIRED)
    target_link_libraries(your_app PRIVATE SummonersSyncCore)
```

### Go

Import the library as a Go module:

```bash
import "github.com/your-org/summoners-sync-core"
```

### 🔐 Security Considerations

- Secret Management:
    Secrets (like JWT signing keys) are never hardcoded. Instead, inject them dynamically during runtime using environment variables or secret managers.

- Secure Builds:
    Artifacts (e.g., .jar, .so) are signed and versioned for authenticity. Ensure CI/CD logs do not expose sensitive information.

- Scoped Access:
    Restrict access to artifacts on GitHub to authorized users only.

## 🤝 Contributing

We welcome contributions! You can find in the [CONTRIBUTING](./CONTRIBUTING.md) file a detailed explanation about what we're expecting.
But here's a quick overview on how you can get involved:

- First, open an issue describing what are you planning to do.
- Fork this repository.
- Create a feature branch: `git checkout -b feature/your-feature`
- Submit a pull request against the `develop` branch with your changes.
- Wait for the review of the `CODEOWNERS`.
- When is approved, use the `squash-and-merge` strategy.

## 📄 License

This project is licensed under the MIT License. See the LICENSE file for details.
