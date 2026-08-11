# Created on Tuesday, 11 Aug 2026

# Teragram Server Build Book

## Purpose

This repository documents the architecture, configuration, security, operation, backup, recovery, and rebuild procedures for the Teragram Platform server environment.

The Build Book is intended to provide a clear, repeatable reference for maintaining the current environment and rebuilding the server from a clean installation.

---

## Engineering Approach

This Build Book follows the five Project Athena engineering principles:

1. **Secure by Design** — minimise attack surface and apply least privilege.
2. **Reproducible** — document and automate processes so systems can be rebuilt.
3. **Observable** — provide visibility through logs, metrics, health checks, and alerts.
4. **Maintainable** — keep systems and documentation understandable and sustainable.
5. **Explainable** — document the reasoning behind significant technical decisions.

For further detail, see the Project Athena engineering principles.

---

## Repository Structure

### 01 — Foundation

Documents the underlying server platform.

- Hardware
- Operating system
- Baseline configuration

### 02 — Security

Documents security controls and hardening.

- SSH
- Firewall
- Operating system updates

### 03 — Web Server

Documents web server configuration.

- Apache
- Virtual Hosts
- Site separation

### 04 — Cloudflare

Documents external access and Cloudflare configuration.

- Cloudflare Tunnel
- DNS and hostname configuration
- Origin configuration

### 05 — Backups

Documents backup and recovery procedures.

- Backup strategy
- Configuration backups
- Recovery procedures

### 06 — Monitoring

Documents system observability.

- Monitoring
- Logging
- Health checks
- Alerts

> Monitoring infrastructure is planned for a future milestone.

### 07 — Disaster Recovery

Documents complete server recovery and rebuild procedures.

- Clean installation
- Infrastructure restoration
- Service restoration
- Validation

---

## Documentation Standards

Every document should:

- Begin with a `Created on` date.
- Clearly state its purpose.
- Distinguish between current, planned, and deprecated configurations.
- Explain significant technical decisions.
- Never contain passwords, private keys, API tokens, credentials, or other secrets.
- Avoid exposing sensitive information unnecessarily.

---

## Security Rule

**No credentials or secrets are stored in this repository.**

This includes:

- Passwords
- API keys
- Access tokens
- Private keys
- Cloudflare credentials
- SSH private keys
- Database credentials
- Recovery codes
- Other authentication material

Where a configuration requires a secret, the documentation will describe **how the secret is securely provided**, without recording the secret itself.

---

## Configuration Status

Documentation uses the following terminology:

| Status | Meaning |
| ------ | ------- |
| `Current` | Verified against the running server |
| `Planned` | Intended future configuration |
| `Deprecated` | Previously used but no longer recommended |
| `Unknown` | Requires verification |

Documentation must not describe a configuration as `Current` until it has been verified.

---

## Server Philosophy

The Raspberry Pi is treated as a production server rather than the source of truth.

The intended workflow is:

```text
Windows Development Environment
            ↓
        GitHub
            ↓
 Infrastructure as Code
            ↓
      Raspberry Pi
            ↓
       Production

GitHub contains the documented and version-controlled definition of the environment.

The server should eventually be capable of being rebuilt from this documentation and the associated Infrastructure as Code repository.

## Build Lifecycle

The intended lifecycle for infrastructure changes is:

Research
   ↓
Document
   ↓
Design
   ↓
Automate
   ↓
Test
   ↓
Deploy
   ↓
Observe
   ↓
Review

## Guiding principle:

Document first, automate second, deploy last.

## Related Projects

- `teragram-platform` — Project Athena engineering framework and architectural decisions.
- `teragram-infrastructure` — Infrastructure as Code and automation.
- `teragram.au` — Teragram website.
- `dramaqueen` — DramaQueen application.
- `athena0x` — Cybersecurity learning and technical documentation.

## Current Milestone

**M1 — Server Foundation**

The current objective is to establish a clean, secure, reproducible server architecture and document the existing environment before further configuration changes are made.