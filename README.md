# ClipNSync

> Universal clipboard sync across all your devices — with end-to-end encryption.

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT)
[![Status: In Development](https://img.shields.io/badge/Status-In%20Development-yellow.svg)]()
[![Platform](https://img.shields.io/badge/Platform-Windows%20%7C%20macOS%20%7C%20Linux%20%7C%20iOS%20%7C%20Android-lightgrey.svg)]()
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](CONTRIBUTING.md)

Copy something on your Mac. Paste it on your Windows PC. Instantly. No cloud company reads your clipboard — everything is encrypted client-side before it ever leaves your device.

---

## Why ClipNSync?

Most clipboard managers are either single-platform, require trusting a third party with your clipboard data, or are closed source. ClipNSync is different:

- **Truly cross-platform** — native apps for Windows, macOS, Linux, iOS, and Android
- **End-to-end encrypted** — AES-256 encryption before data leaves your device. The server never sees plaintext
- **Open source** — audit the code, self-host the server, own your data
- **Self-hostable** — run your own sync server with a single Docker Compose command
- **Rich clipboard history** — text, images, files, and code snippets with search and pinning

---

## Platform Support

| Platform | Status | Tech Stack |
|----------|--------|------------|
| macOS | 🚧 In Development | Swift, Menu Bar |
| Windows | 🚧 In Development | C# / WPF, System Tray |
| Linux | 🚧 In Development | Rust, X11 + Wayland |
| iOS | 📋 Planned | Swift, Share + Keyboard Extension |
| Android | 📋 Planned | Kotlin, Accessibility Service |
| Web Dashboard | 📋 Planned | Next.js |
| Browser Extension | 📋 Planned | Chrome / Firefox |

---

## Architecture

```
┌─────────────┐  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐
│   Windows   │  │    macOS    │  │    Linux    │  │  iOS/Android│
│  C# / WPF   │  │    Swift    │  │    Rust     │  │Swift/Kotlin │
└──────┬──────┘  └──────┬──────┘  └──────┬──────┘  └──────┬──────┘
       │                │                │                 │
       └────────────────┴────────────────┴─────────────────┘
                                 │
                    ┌────────────▼────────────┐
                    │   ClipNSync Sync Server  │
                    │   REST API + WebSocket   │
                    │      (Go backend)        │
                    └────────────┬────────────┘
                                 │
                    ┌────────────▼────────────┐
                    │  PostgreSQL + S3/R2      │
                    │  (encrypted at rest)     │
                    └─────────────────────────┘
```

**How sync works:**
1. You copy something on Device A
2. The native client encrypts it locally with your key (AES-256)
3. Encrypted ciphertext is pushed to the sync server via WebSocket
4. All your other connected devices receive the push notification
5. Each device decrypts locally and updates its clipboard
6. The server never holds your encryption key — ever

---

## Getting Started

> The project is in early development. Instructions will be updated as clients are released.

### Self-hosting the server

```bash
git clone https://github.com/chrysek70/clipnsync.git
cd clipnsync/server
cp .env.example .env
# Edit .env with your settings
docker-compose up -d
```

### Cloud-hosted (coming soon)

A hosted version will be available at [clipnsync.com](https://clipnsync.com) for users who prefer not to self-host.

---

## Repository Structure

```
clipnsync/
├── server/          # Go backend — REST API + WebSocket sync
├── clients/
│   ├── macos/       # Swift — Menu bar app
│   ├── windows/     # C# / WPF — System tray app
│   ├── linux/       # Rust — X11 + Wayland daemon
│   ├── ios/         # Swift — Share + Keyboard extension
│   └── android/     # Kotlin — Background sync service
├── web/             # Next.js — Web dashboard
├── browser-ext/     # Chrome + Firefox extension
└── docs/
    ├── ARCHITECTURE.md
    ├── SELF_HOSTING.md
    ├── ENCRYPTION.md
    └── CONTRIBUTING.md
```

---

## Roadmap

### v0.1 — Foundation
- [ ] Go sync server with REST API and WebSocket
- [ ] User auth with JWT
- [ ] Client-side AES-256 encryption
- [ ] macOS native client
- [ ] Windows native client

### v0.2 — Cross-platform desktop
- [ ] Linux client (X11 + Wayland)
- [ ] Clipboard history with search
- [ ] Pin and star clips
- [ ] Docker Compose self-hosting

### v0.3 — Mobile
- [ ] iOS app + Share extension
- [ ] Android app
- [ ] Custom keyboard for mobile paste history

### v1.0 — Full release
- [ ] Web dashboard
- [ ] Browser extension
- [ ] Teams / shared clipboards
- [ ] Hosted cloud service at clipnsync.com

---

## Security

ClipNSync is designed with a zero-knowledge architecture:

- All clipboard data is encrypted on your device before transmission
- Your encryption key never leaves your device
- The sync server stores and transmits ciphertext only
- You can verify this by auditing the source code or running your own server

For security disclosures, please email **security@clipnsync.com** rather than opening a public issue.

---

## Contributing

Contributions are very welcome! ClipNSync is an ambitious multi-platform project and there is plenty to build.

- Read [CONTRIBUTING.md](docs/CONTRIBUTING.md) before opening a PR
- Check the [Issues](https://github.com/chrysek70/clipnsync/issues) tab for good first issues
- Join the [Discussions](https://github.com/chrysek70/clipnsync/discussions) to propose features or ask questions

---

## License

MIT License — see [LICENSE](LICENSE) for details.

---

<p align="center">
  <a href="https://clipnsync.com">clipnsync.com</a> •
  <a href="https://github.com/chrysek70/clipnsync/discussions">Discussions</a> •
  <a href="https://github.com/chrysek70/clipnsync/issues">Issues</a>
</p>
