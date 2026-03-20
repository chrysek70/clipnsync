# ClipNSync

> Universal clipboard sync across all your devices — with end-to-end encryption.

[![License: BUSL-1.1](https://img.shields.io/badge/License-BUSL%201.1-blue.svg)](LICENSE)
[![Status: In Development](https://img.shields.io/badge/Status-In%20Development-yellow.svg)]()
[![Platform](https://img.shields.io/badge/Platform-Windows%20%7C%20macOS%20%7C%20Linux%20%7C%20iOS%20%7C%20Android-lightgrey.svg)]()
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](docs/CONTRIBUTING.md)

Copy something on your Mac. Paste it on your Windows PC. Instantly. No cloud company reads your clipboard — everything is encrypted client-side before it ever leaves your device.

---

## Why ClipNSync?

Most clipboard managers are either single-platform, require trusting a third party with your clipboard data, or are closed source. ClipNSync is different:

- **Truly cross-platform** — native apps for Windows, macOS, Linux, iOS, and Android
- **End-to-end encrypted** — AES-256 encryption before data leaves your device. The server never sees plaintext
- **Bring your own cloud** — sync via iCloud, Google Drive, Dropbox, OneDrive, Nextcloud, or ClipNSync Cloud
- **Source available** — audit the encryption, self-host the server, verify our privacy claims
- **Self-hostable** — run your own sync server with a single Docker Compose command

---

## Sync Backends

ClipNSync lets you choose where your encrypted clipboard data lives:

| Backend | Cost | Best for |
|---------|------|----------|
| iCloud Drive | Free (your storage) | Apple ecosystem users |
| Google Drive | Free (your storage) | Android + cross-platform users |
| Dropbox | Free (your storage) | Existing Dropbox users |
| OneDrive | Free (your storage) | Windows users |
| Nextcloud | Free (self-hosted) | Privacy-focused / self-hosters |
| ClipNSync Cloud | See pricing | Easiest setup, real-time push, web dashboard |

All backends use the same client-side AES-256 encryption. ClipNSync (or any cloud provider) never sees your plaintext clipboard data.

---

## How It Works

```
Device A copies text
  -> encrypted locally with your key (AES-256)
  -> written to your chosen cloud storage
  -> cloud syncs encrypted file to all devices
  -> Device B detects file change
  -> decrypts locally
  -> clipboard updated instantly
```

The sync server (or cloud provider) only ever stores and transfers ciphertext. Your encryption key never leaves your devices.

---

## Platform Support

| Platform | Status | Tech Stack |
|----------|--------|------------|
| macOS | In Development | Swift, Menu Bar app |
| Windows | In Development | C# / WPF, System Tray app |
| Linux | In Development | Rust, X11 + Wayland daemon |
| iOS | Planned | Swift, Share + Keyboard Extension |
| Android | Planned | Kotlin, Background sync service |
| Web Dashboard | Planned | Next.js |
| Browser Extension | Planned | Chrome / Firefox |

---

## Pricing

| Plan | Price | Devices | Features |
|------|-------|---------|----------|
| Free | $0 | Up to 3 | Basic text sync, last 100 clips |
| Personal | $2.99/mo or $24.99/yr | Unlimited | Full history, images, files, all backends |
| Family | $4.99/mo or $39.99/yr | 6 users | All Personal features, shared clipboards |
| Team | $9.99/mo per team | Unlimited users | Audit logs, priority support, SSO |

Native apps are available separately on the App Store, Mac App Store, and Microsoft Store.

Self-hosting is always free for personal use under the [BUSL 1.1 license](LICENSE).

---

## Architecture

```
+------------------+  +------------------+  +------------------+
|     Windows      |  |      macOS       |  |  iOS / Android   |
|   C# / WPF       |  |     Swift        |  |  Swift / Kotlin  |
+--------+---------+  +--------+---------+  +--------+---------+
         |                     |                     |
         +---------------------+---------------------+
                               |
                  (encrypted file sync)
                               |
               +---------------+---------------+
               |                               |
     +---------+--------+           +----------+---------+
     |  Your cloud of   |           |  ClipNSync Cloud   |
     |  choice          |           |  (hosted service)  |
     |  iCloud / Drive  |           |  Real-time push    |
     |  Dropbox / etc.  |           |  Web dashboard     |
     +------------------+           +--------------------+
```

---

## Getting Started

> The project is in early development. Instructions will be updated as clients are released.

### Self-hosting the sync server

```bash
git clone https://github.com/chrysek70/clipnsync.git
cd clipnsync/server
cp .env.example .env
# Edit .env with your settings
docker-compose up -d
```

### Hosted service

Sign up at [clipnsync.com](https://clipnsync.com) for the easiest setup with real-time push sync and a web dashboard.

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
- [ ] Core encryption library (AES-256, shared across all clients)
- [ ] File-based sync protocol (works with any cloud storage)
- [ ] macOS native client with iCloud + Google Drive support
- [ ] Windows native client

### v0.2 — Cross-platform desktop
- [ ] Linux client (X11 + Wayland)
- [ ] Dropbox, OneDrive, Nextcloud backends
- [ ] Clipboard history with search
- [ ] Pin and star clips
- [ ] Docker Compose self-hosting

### v0.3 — Mobile
- [ ] iOS app + Share extension + Keyboard extension
- [ ] Android app
- [ ] Mobile clipboard history browser

### v1.0 — Full release
- [ ] ClipNSync Cloud hosted service
- [ ] Web dashboard
- [ ] Browser extension
- [ ] Family and Team plans
- [ ] Apps on App Store, Mac App Store, Microsoft Store

---

## Security

ClipNSync is designed with a zero-knowledge architecture:

- All clipboard data is encrypted on your device before transmission
- Your encryption key never leaves your device
- The sync server or cloud provider stores and transmits ciphertext only
- You can verify this by auditing the source code or running your own server

For security disclosures please email **security@clipnsync.com** rather than opening a public issue.

---

## License

ClipNSync is source-available under the [Business Source License 1.1](LICENSE).

**You may:**
- Read and audit the source code
- Self-host the server for personal, non-commercial use
- Contribute to the project
- Fork for personal non-commercial projects

**You may not:**
- Offer ClipNSync as a hosted service to others
- Bundle it into a commercial product
- Use it within a for-profit organization without a commercial license

On **2036-01-01** the license converts to MIT.

For commercial licensing: **license@clipnsync.com**

---

## Contributing

Contributions are welcome! Read [CONTRIBUTING.md](docs/CONTRIBUTING.md) before opening a PR.
Check [Issues](https://github.com/chrysek70/clipnsync/issues) for good first issues and join [Discussions](https://github.com/chrysek70/clipnsync/discussions) to propose features.

---

<p align="center">
  <a href="https://clipnsync.com">clipnsync.com</a> •
  <a href="https://github.com/chrysek70/clipnsync/discussions">Discussions</a> •
  <a href="https://github.com/chrysek70/clipnsync/issues">Issues</a> •
  <a href="mailto:license@clipnsync.com">Commercial Licensing</a>
</p>
