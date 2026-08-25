# Server Updates and Patch Management

# Created on Mon, 25 Aug 2026

## Purpose

This document records the operating system update process and patch
management approach for the Project Athena server.

---

## Current Policy

The server should be kept updated with security and stability fixes.

Updates must be applied deliberately and verified after installation.

---

## Update Procedure

Refresh package metadata:

```bash
sudo apt update
```

Review available upgrades:

```bash
apt list --upgradable
```

Apply standard upgrades:

```bash
sudo apt upgrade
```

Remove packages that are no longer required when appropriate:

```bash
sudo apt autoremove
```

---

## Verification

After significant updates, verify:

```bash
uname -a
```

```bash
hostnamectl
```

```bash
systemctl --failed
```

```bash
sudo ss -tulpn
```

```bash
sudo ufw status verbose
```

SSH access should also be tested after security-sensitive system updates.

---

## Kernel Updates

Kernel updates require additional verification because they may affect:

- Hardware compatibility
- NVMe boot
- Network interfaces
- Raspberry Pi functionality
- Docker compatibility
- Security controls

After a kernel update and reboot:

- [ ] System boots successfully
- [ ] NVMe remains the boot device
- [ ] Network connectivity verified
- [ ] SSH access verified
- [ ] Firewall verified
- [ ] Listening services reviewed

---

## Automatic Updates

Automatic security updates have not yet been formally configured.

Status:

```text
NOT YET IMPLEMENTED
```

This will be addressed as a future security-hardening task.

---

## Update Verification Record

| Date | Action | Result |
| --- | --- | --- |
| 25 Aug 2026 | M0 update state reviewed | Verified |

---

## Future Work

- [ ] Configure automated security updates
- [ ] Define maintenance window
- [ ] Define reboot policy
- [ ] Document rollback approach
- [ ] Add update verification to maintenance procedure