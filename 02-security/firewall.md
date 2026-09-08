# Created on Mon, 25 Aug 2026

# Firewall Configuration

## Purpose

This document records the host firewall configuration for the Project Athena
server.

The firewall provides the first layer of inbound network access control.

The security model is based on default-deny inbound traffic with explicit
allow rules for required services.

Environment-specific network identifiers are intentionally excluded.

---

## Firewall Technology

The server uses UFW (Uncomplicated Firewall) as the host firewall.

Status:

```text
ACTIVE
```

---

## Default Policy

```text
Incoming: DENY
Outgoing: ALLOW
Routed:   DENY
```

This means inbound connections are denied unless an explicit firewall rule
allows them.

Outbound connections are permitted by default.

---

## Current Rules

SSH is the only intentionally permitted inbound administrative service.

```text
22/tcp    ALLOW IN    <TRUSTED_LAN_CIDR>
```

SSH is therefore restricted to the trusted local network.

No public Internet-facing application ports are intentionally opened at the
current baseline stage.

---

## SSH Firewall Rule

The SSH rule was configured before enabling UFW to prevent administrative
lockout.

The final intended rule is:

```text
Source: <TRUSTED_LAN_CIDR>
Destination: Server
Port: 22/tcp
Action: ALLOW
```

---

## Firewall Activation

The firewall was enabled only after the required SSH rule had been added.

The existing SSH session remained operational after firewall activation.

A new SSH connection was subsequently verified successfully.

---

## Unnecessary Network Services

The firewall configuration was implemented alongside removal of unnecessary
network services.

The following services were disabled:

### RPCbind

```text
rpcbind.service
rpcbind.socket
```

Associated port:

```text
111
```

Status:

```text
DISABLED
```

### Avahi

```text
avahi-daemon.service
avahi-daemon.socket
```

Associated port:

```text
5353
```

Status:

```text
DISABLED
```

These services are not required for the current server role.

---

## Verification

The following were verified:

- [x] UFW installed
- [x] UFW enabled
- [x] Logging enabled
- [x] Default incoming policy set to deny
- [x] Default outgoing policy set to allow
- [x] Routed traffic denied
- [x] SSH allowed from trusted LAN
- [x] Public SSH access not intentionally permitted
- [x] RPCbind disabled
- [x] Avahi disabled
- [x] LAN-restricted SSH firewall rule verified

---

## Verification Commands

Check firewall status:

```bash
sudo ufw status verbose
```

Show configured rules:

```bash
sudo ufw show added
```

Check listening network services:

```bash
sudo ss -tulpn
```

Check RPCbind:

```bash
systemctl is-active rpcbind.service
systemctl is-enabled rpcbind.service
```

Check Avahi:

```bash
systemctl is-active avahi-daemon.service
systemctl is-enabled avahi-daemon.service
```

---

## Firewall Change Procedure

Before enabling or changing firewall rules:

1. Identify all required services.
2. Identify their required ports.
3. Add required allow rules.
4. Verify the rules.
5. Enable or reload the firewall.
6. Verify existing administration access.
7. Test the required services.
8. Remove unnecessary rules.

Firewall rules must not be added speculatively.

Only services that are actually required should receive inbound access.

---

## Future Firewall Work

Future firewall changes will be required when additional services are
implemented.

Potential future services include:

- Web applications
- Reverse proxy
- Docker workloads
- Monitoring
- Other approved infrastructure services

Each new service must be evaluated before its port is opened.

The default-deny inbound policy should remain in place.