# Created on Fri, 7 Aug 2026

# Project Athena — Server Security Baseline

## Purpose

This document records the initial security baseline for the Project Athena server platform.

The baseline establishes the minimum security posture before application services, containers, infrastructure automation, or public-facing workloads are deployed.

This document is intentionally generic and contains no identifying infrastructure details, credentials, secrets, private keys, or production configuration.

---

## Baseline Principles

The server is managed according to the Project Athena engineering principles:

1. Secure by Design
2. Reproducible
3. Observable
4. Maintainable
5. Explainable

Security controls should be explicit, documented, testable, and reproducible.

---

# 1. Operating System

## Platform

- Raspberry Pi server platform
- 64-bit ARM architecture
- Debian-based Raspberry Pi OS
- System booting from NVMe storage
- Fresh operating system installation completed

## Storage

- NVMe is the primary system storage
- Root filesystem is mounted from NVMe
- Boot partition is mounted from NVMe
- No secondary storage is currently required for the baseline

## Verification

- [x] Operating system boots successfully
- [x] NVMe storage is detected
- [x] Root filesystem is mounted correctly
- [x] Boot partition is mounted correctly
- [x] System has sufficient available storage

---

# 2. Network Baseline

## Network

The server currently uses a trusted local network for administration.

Network identifiers are intentionally omitted from this document.

### Trusted Administration Network

```text
<TRUSTED_LAN_CIDR>
```

### Server Address

```text
<SERVER_IP>
```

### Gateway

```text
<GATEWAY_IP>
```

These values are environment-specific and must not be committed to public documentation unless there is a specific reason to expose them.

## Network Verification

- [x] Network interface operational
- [x] Server receives a local network address
- [x] Default gateway confirmed
- [x] SSH connectivity verified
- [x] Unnecessary network services reviewed

---

# 3. SSH Security

SSH is the primary administrative access mechanism.

## Authentication

The server uses public-key authentication.

### Configuration

```text
PermitRootLogin              no
PasswordAuthentication       no
PubkeyAuthentication         yes
KbdInteractiveAuthentication no
```

## SSH Hardening

The following controls are enabled:

- [x] Root SSH login disabled
- [x] Password authentication disabled
- [x] Public-key authentication enabled
- [x] Keyboard-interactive authentication disabled
- [x] X11 forwarding disabled
- [x] TCP forwarding disabled
- [x] Maximum authentication attempts reduced
- [x] Login grace period reduced

Current values:

```text
MaxAuthTries       3
LoginGraceTime     30
```

## SSH Access Scope

SSH is restricted to the trusted local network.

```text
SSH_PORT = 22/tcp
SSH_SOURCE = <TRUSTED_LAN_CIDR>
```

SSH is not intentionally exposed directly to the public Internet.

## SSH Verification

- [x] SSH server listening
- [x] SSH key authentication verified
- [x] Windows administration client successfully authenticated
- [x] Password authentication disabled
- [x] Root login disabled
- [x] SSH forwarding disabled
- [x] SSH restricted to trusted LAN

---

# 4. Firewall

UFW is installed and enabled.

## Default Policy

```text
Incoming: DENY
Outgoing: ALLOW
Routed:   DISABLED
```

This establishes a default-deny inbound security model.

## Current Rules

The baseline intentionally exposes only the minimum required administrative service.

```text
22/tcp    ALLOW IN    <TRUSTED_LAN_CIDR>
```

No public Internet-facing service ports are intentionally opened at this stage.

## Firewall Verification

- [x] UFW installed
- [x] UFW enabled
- [x] Default incoming policy set to deny
- [x] Default outgoing policy set to allow
- [x] SSH explicitly allowed from trusted LAN
- [x] SSH connection verified after firewall activation

---

# 5. Unnecessary Network Services

Services that were not required for the current server role were reviewed.

## RPCbind

`rpcbind` was enabled initially but is not required for the current server architecture.

Status:

```text
DISABLED
```

Port 111 is therefore not intentionally exposed.

Verification:

- [x] rpcbind service disabled
- [x] rpcbind socket disabled
- [x] Port 111 no longer listening

## Avahi / mDNS

Avahi was enabled initially but is not required for the current server architecture.

Status:

```text
DISABLED
```

Port 5353 is therefore not intentionally exposed.

Verification:

- [x] Avahi service disabled
- [x] Avahi socket disabled
- [x] Port 5353 no longer listening

---

# 6. Listening Services

The listening network services were reviewed as part of the baseline.

The current intentional administrative listener is:

```text
SSH
TCP
Port 22
Trusted LAN only
```

Dynamic client-side ports and local application sockets may appear during normal operation. Their presence does not automatically indicate that an inbound firewall rule is required.

Before deploying additional services, listening ports must be reviewed and documented.

---

# 7. Public Exposure

At the baseline stage:

```text
Direct public inbound services: NONE
```

The server is not intended to expose administrative services directly to the Internet.

Future public-facing services will use the approved architecture rather than opening ports by default.

Potential future components include:

- Cloudflare Tunnel
- Reverse proxy
- Docker containers
- Application services
- Monitoring services

Each component must be evaluated before exposure.

---

# 8. Secrets and Sensitive Information

The following must never be committed to the public repository:

- Passwords
- SSH private keys
- SSH private-key passphrases
- API tokens
- Cloudflare credentials
- Tunnel credentials
- TLS private keys
- Database credentials
- Recovery codes
- Personal identifiers
- Internal-only infrastructure details
- Unredacted network information where unnecessary

Use placeholders in public documentation:

```text
<SERVER_HOSTNAME>
<SERVER_IP>
<TRUSTED_LAN_CIDR>
<GATEWAY_IP>
<ADMIN_USER>
<DOMAIN>
<TUNNEL_ID>
```

Public documentation should describe architecture and decisions without revealing secrets.

---

# 9. Verification Commands

The following commands can be used to reproduce the baseline checks.

## Operating System

```bash
hostnamectl
uname -a
```

## Storage

```bash
lsblk
df -h
```

## Network

```bash
ip -br addr
ip route
```

## SSH

```bash
sudo sshd -T
```

Target authentication settings:

```text
permitrootlogin no
passwordauthentication no
pubkeyauthentication yes
kbdinteractiveauthentication no
```

## Listening Services

```bash
sudo ss -tulpn
```

## Firewall

```bash
sudo ufw status verbose
```

## Service Status

```bash
systemctl --type=service --state=running
```

---

# 10. Baseline Acceptance Criteria

The baseline is considered complete when:

- [x] Operating system installed and booting correctly
- [x] NVMe storage verified
- [x] Network connectivity verified
- [x] SSH key authentication verified
- [x] SSH password authentication disabled
- [x] Root SSH login disabled
- [x] SSH forwarding disabled
- [x] Unnecessary network services disabled
- [x] UFW installed and enabled
- [x] Default inbound policy set to deny
- [x] SSH restricted to trusted LAN
- [x] No unnecessary public inbound ports intentionally exposed
- [x] Sensitive information excluded from documentation

---

# 11. Known Future Work

The following are intentionally outside the current baseline:

- [ ] Cloudflare Tunnel configuration
- [ ] Docker installation
- [ ] Container security baseline
- [ ] Application deployment
- [ ] Reverse proxy architecture
- [ ] Infrastructure as Code
- [ ] Centralised logging
- [ ] Monitoring and alerting
- [ ] Backup automation
- [ ] Automated security updates
- [ ] Configuration management
- [ ] Disaster recovery testing
- [ ] Vulnerability scanning
- [ ] Network segmentation
- [ ] Service-specific firewall rules

These items will be addressed through subsequent milestones rather than being added prematurely to the foundation baseline.

---

# 12. Change Control

Changes to the security baseline must be:

1. Explicitly identified
2. Tested
3. Documented
4. Reproducible
5. Reviewed against the Project Athena engineering principles

Temporary changes must not silently become permanent configuration.

Security-sensitive changes should be accompanied by an appropriate Architecture Decision Record where the decision has architectural impact.

---

# 13. Baseline Status

**Status:** COMPLETE

**Milestone:** M0 — Foundation

**Baseline Version:** 1.0

**Last Updated:** Tues, 25 Aug 2026

The server has reached the minimum security baseline required to proceed with the next Project Athena infrastructure stage.

Future infrastructure should build on this baseline rather than bypassing or weakening it.