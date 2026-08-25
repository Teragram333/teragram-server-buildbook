# Created on Mon, 25 Aug 2026

# SSH Configuration and Hardening

## Purpose

This document records the SSH configuration and hardening applied to the
Project Athena server.

SSH is the primary remote administration mechanism.

Environment-specific information such as hostnames, IP addresses, usernames,
key fingerprints, and private keys is intentionally excluded.

---

## Authentication Model

SSH uses public-key authentication.

Password-based SSH authentication is disabled.

Root login through SSH is disabled.

### Effective Configuration

```text
PermitRootLogin              no
PasswordAuthentication       no
PubkeyAuthentication         yes
KbdInteractiveAuthentication no
```

---

## Hardening Configuration

The following SSH hardening controls are enabled:

```text
LoginGraceTime               30
MaxAuthTries                 3
X11Forwarding                no
AllowTcpForwarding           no
```

### Rationale

- Root SSH access is disabled to reduce the impact of credential compromise.
- Password authentication is disabled to prevent password-based SSH attacks.
- Public-key authentication provides the approved authentication mechanism.
- Keyboard-interactive authentication is disabled because it is not required.
- X11 forwarding is disabled because it is not required for server administration.
- TCP forwarding is disabled because it is not required for the current server role.
- The login grace period is reduced to limit the time available for incomplete
  authentication attempts.
- Maximum authentication attempts are reduced to limit repeated authentication
  attempts within a single connection.

---

## SSH Access Scope

SSH is restricted by the host firewall to the trusted local network.

```text
Port:   22/tcp
Source: <TRUSTED_LAN_CIDR>
```

SSH is not intentionally exposed directly to the public Internet.

---

## Key Management

The administration workstation uses an ED25519 SSH key pair.

The private key remains exclusively on the administration workstation.

The corresponding public key is stored in the server user's:

```text
~/.ssh/authorized_keys
```

Private keys, passphrases, and complete public-key contents must never be
committed to the public repository.

---

## SSH Configuration Files

The effective SSH configuration may be composed from:

```text
/etc/ssh/sshd_config
/etc/ssh/sshd_config.d/
```

Configuration precedence must be considered when modifying SSH settings.

Effective configuration should be verified with:

```bash
sudo sshd -T
```

---

## Verification

The following controls were verified after configuration:

- [x] SSH service active
- [x] SSH listening on TCP port 22
- [x] Public-key authentication working
- [x] Password authentication disabled
- [x] Root SSH login disabled
- [x] Keyboard-interactive authentication disabled
- [x] X11 forwarding disabled
- [x] TCP forwarding disabled
- [x] Login grace period reduced
- [x] Maximum authentication attempts reduced
- [x] SSH access restricted to trusted LAN
- [x] SSH connection successfully tested after firewall activation

---

## Verification Commands

Check effective SSH configuration:

```bash
sudo sshd -T
```

Check SSH service:

```bash
systemctl status ssh
```

Check SSH listener:

```bash
sudo ss -lntp | grep ':22'
```

Check authorised keys:

```bash
ssh-keygen -lf ~/.ssh/authorized_keys
```

---

## Security Notes

Changes to SSH configuration must be tested before terminating an existing
administrative session.

When modifying SSH authentication settings:

1. Keep an existing administrative session open.
2. Apply the configuration change.
3. Validate the SSH configuration.
4. Restart or reload SSH as required.
5. Establish a new SSH session.
6. Verify successful authentication.
7. Only then close the original session.

This procedure reduces the risk of accidental administrative lockout.