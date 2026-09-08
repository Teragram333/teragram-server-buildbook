# Created on Tue, 8 Sep 2026

# Apache HTTP Server

## Purpose

This document records the Apache configuration used as the HTTPS origin for the Teragram platform.

Apache is intentionally positioned behind Cloudflare Tunnel and is not directly exposed to the public Internet.

---

## Current Status

Apache is installed, enabled, and serving HTTPS on port 443.

HTTP port 80 is not currently listened to.

---

## HTTPS Origin

Apache provides the local HTTPS origin used by cloudflared.

* HTTPS listener: `443`
* Server name: `<DOMAIN>`
* HTTP listener: disabled
* TLS certificate: Cloudflare Origin CA
* Certificate and private key are stored outside the public repository.

---

## HTTP Exposure

The previous HTTP listener was removed from the active Apache configuration.

* Default HTTP virtual host disabled.
* `Listen 80` removed from `ports.conf`.
* Apache listener audit confirms HTTPS on port 443.
* No direct HTTP origin listener is required.

---

## Architecture

```text
Cloudflare
  ↓
Cloudflare Tunnel
  ↓
cloudflared
  ↓
https://localhost:443
  ↓
Apache
  ↓
Application services
```

---

## Verification

### Apache Configuration

```bash
sudo apache2ctl configtest
```

Expected:

```text
Syntax OK
```

### Local HTTPS Origin

```bash
curl -Ik https://<DOMAIN> --resolve <DOMAIN>:443:127.0.0.1
```

### Listener Audit

```bash
sudo ss -tulpn | grep -E ':(80|443)\b'
```

Apache should have no active port 80 listener and should listen on port 443.

---

## Security

* Origin TLS is enabled.
* Origin certificate and private key are outside source control.
* Private certificate material must never be committed to the public repository.
* Apache is intended to receive traffic from the local Cloudflare Tunnel path rather than direct Internet connections.
