# vfdc
# 🍊 Aurora Liquid DC v4.2.1 — Final Hardened Capsule

Aurora Liquid DC is a self-contained, browser-based infrastructure capsule prototype. It lets you model compute, storage, network, security, memory context, rationale, policy checks, signed event logs, and exportable verification bundles from a single HTML file.

Version **v4.2.1** is the final hardened prototype release. It includes deterministic replay, browser-local Ed25519 signing, event-chain verification, memory-context validation, policy simulation, import verification, patch packs, and a bundled Node.js verifier.

In plain human words: this is a portable infrastructure blueprint builder that records what changed, why it changed, verifies the history, and exports a ZIP capsule that can be checked later. Like Git, Terraform, a notebook, and a paranoid airport security scanner got trapped in a browser tab together.

---

## Project Status

**Version:** v4.2.1  
**Codename:** Final Hardened Capsule  
**Status:** Production-hardened prototype  
**RoboCop Test Score:** 9.8/10  
**Runtime:** Single-page browser application  
**Storage:** Browser `localStorage`  
**Export Format:** ZIP capsule  
**Verification:** Browser import checks plus bundled Node.js verifier  

This project is ready for real-world evaluation, demos, internal review, prototyping, and security-model exploration.

It is **not yet a production key-management system**. Browser-local signing is useful for demonstration and evaluation, but production deployments should move signing keys to a proper key manager, hardware-backed keystore, HSM, secure enclave, server-side signer, or cloud KMS.

Yes, I know, boring caveat. Unfortunately, cryptography is where optimism goes to be audited.

---

## What Aurora Liquid DC Does

Aurora Liquid DC provides a browser interface for building and exporting a self-verifying infrastructure capsule.

It can:

- Model compute workers, storage buckets, network rules, and security policies.
- Track every infrastructure mutation as a signed, chained event.
- Store local memory records that explain project decisions.
- Attach rationale and memory references to future changes.
- Generate ADR-style decision documents.
- Simulate simple security policy checks.
- Export a ZIP capsule containing the blueprint, event log, memory, policy report, prior-art notes, verifier, and checksums.
- Import a capsule ZIP back into the browser and perform basic verification.
- Run a bundled Node.js verifier for deeper verification, including signatures and file checksums.
- Generate patch packs from snapshot points.
- Display live verifier status in the UI.

The important idea is that infrastructure changes are not just final-state blobs. They become a replayable, hash-linked, rationale-carrying event history.

Tiny bureaucracy, but useful. Like a clipboard with cryptographic teeth.

---

## Main Features

### 1. Infrastructure Model

Aurora tracks four primary infrastructure domains:

#### Compute

Represents worker nodes or services.

Example default resources:

- `llama3-inference`
- `api-gateway`

Each compute item includes:

- Name
- CPU count
- Memory size

#### Storage

Represents object stores, model caches, volumes, or buckets.

Each storage item includes:

- Name
- Capacity

#### Network

Represents route and firewall-like rules.

Example default rules:

- `ALLOW 443/TCP -> API`
- `DENY SSH from 0.0.0.0/0`

#### Security

Represents security policies.

Example default policies:

- `mTLS + JWT`
- `AES-256 at rest`

---

### 2. Event-Sourced State

Aurora does not only store the current infrastructure state. It stores a chronological event log.

Every mutation is recorded as an event, such as:

- `compute.add`
- `compute.remove`
- `storage.add`
- `storage.remove`
- `network.add`
- `network.remove`
- `security.add`
- `security.remove`
- `settings.update`
- `snapshot.capture`

Each event includes:

- Event ID
- Timestamp
- Event type
- Source
- Target
- Payload
- Rationale
- Previous event hash
- Current checksum

This creates a hash-linked event chain. If an old event is changed, the chain breaks.

That means tampering becomes visible. Annoying for villains, helpful for everyone else.

---

### 3. Deterministic Replay

The application can rebuild the current infrastructure state from the event log.

This gives Aurora its main verification property:

> The exported blueprint must match the blueprint produced by replaying the event history.

If the blueprint says one thing and the event log replays to another, verification fails.

This protects against silent edits to the final infrastructure blueprint.

---

### 4. Persistent Browser-Local Ed25519 Signing

Aurora creates or loads a persistent Ed25519 keypair using the browser’s Web Crypto API.

The keypair is stored in browser `localStorage` as exported key material.

Aurora uses this keypair to sign exported capsules when signing is enabled.

The UI displays a signer fingerprint so users can identify which local signing key is active.

Important note:

This is acceptable for a prototype, evaluation build, or local demo. For production systems, private keys should not live casually in browser `localStorage`, because browsers are basically raccoons with JavaScript engines.

Recommended production alternatives:

- Cloud KMS
- Hardware Security Module
- Secure enclave
- WebAuthn-backed signing
- Server-side signing service
- Enterprise key custody workflow

---

### 5. Memory Context

Aurora includes a local memory system for storing project knowledge, decisions, warnings, and rationale.

Memory records include:

- ID
- Room
- Title
- Content
- Tags
- Created timestamp

Default example:

```text
OIDC over static keys
Require OIDC for production
