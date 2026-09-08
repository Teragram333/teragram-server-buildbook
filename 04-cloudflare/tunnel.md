# Created on Tue, 8 Sep 2026

# Cloudflare Tunnel

## Purpose

This document records the Cloudflare Tunnel configuration used as the public access path for the Teragram platform.

Cloudflare Tunnel provides the connection between Cloudflare's edge network and the locally hosted Teragram server without requiring direct public inbound access to the origin server.

---

## Current Status

Cloudflare Tunnel is active and is the intended public access path for the Teragram platform.

The active tunnel is locally managed by the server.

A stale tunnel was removed during the server security and exposure audit.

---

## Architecture

```text
Internet
  ↓
Cloudflare
  ↓
Cloudflare Tunnel
  ↓
cloudflared
  ↓
https://localhost:443
  ↓
Apache HTTPS Origin
```

The origin server is not intended to receive direct public traffic.

---

## Origin Connection

The active tunnel forwards traffic to the local Apache HTTPS origin.

```text
Tunnel
  ↓
cloudflared
  ↓
https://localhost:443
```

TLS is therefore maintained between Cloudflare and the Apache origin.

The Apache origin certificate is issued for Cloudflare Origin use and is not stored in the public repository.

---

## Public Exposure Model

The intended exposure model is:

* Public traffic terminates at Cloudflare.
* Cloudflare Tunnel establishes the outbound connection from the server.
* The origin server does not require direct public inbound web ports.
* Apache provides the local HTTPS origin.
* Application services will remain behind the server's internal architecture.

This reduces the public attack surface compared with directly exposing the origin server.

---

## Tunnel Management

Tunnel credentials and configuration are stored on the server and must not be committed to the public repository.

The following must remain private:

* Tunnel credentials.
* Tunnel tokens.
* Tunnel identifiers where unnecessary.
* Origin certificate private keys.
* Deployment-specific configuration values.

Public documentation uses placeholders where appropriate:

```text
<TUNNEL_ID>
<DOMAIN>
<SERVER_HOSTNAME>
```

---

## Verification

### Tunnel Service

```bash
systemctl status cloudflared
```

### Active Tunnel

```bash
cloudflared tunnel list
```

### Local Origin

```bash
curl -Ik https://<DOMAIN> --resolve <DOMAIN>:443:127.0.0.1
```

### Public Access

Public access should be verified through the intended Cloudflare hostname rather than by exposing the origin directly.

---

## Security

* Cloudflare Tunnel is the intended public access path.
* No direct public inbound origin exposure is part of the intended architecture.
* Tunnel credentials remain outside source control.
* Origin private keys remain outside source control.
* Deployment-specific infrastructure values are not included in public documentation.
* Changes to public exposure should be reviewed against the Project Athena security principles.
