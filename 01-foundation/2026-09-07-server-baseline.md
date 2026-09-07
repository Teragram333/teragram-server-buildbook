\# Server Baseline — 7 September 2026



\*\*Project:\*\* Project Athena / Teragram Server

\*\*Purpose:\*\* Record the current verified state of the server foundation.



> \*\*Security:\*\* Deployment-specific identifiers, credentials, private keys, IP addresses, tunnel IDs, and other sensitive values are intentionally omitted.



\## Server



\* Raspberry Pi 5

\* Debian GNU/Linux 13 (Trixie)

\* ARM64

\* NVMe-based system storage

\* Wi-Fi is the active network connection

\* Ethernet is currently unused



\## Core Services



The following components are installed and operational:



\* Apache HTTP Server

\* Docker Engine

\* Cloudflare Tunnel (`cloudflared`)

\* NetworkManager



\## Web Architecture



The current request path is:



```text

Internet

&#x20;   ↓

Cloudflare

&#x20;   ↓

Cloudflare Tunnel

&#x20;   ↓

cloudflared

&#x20;   ↓

HTTPS localhost:443

&#x20;   ↓

Apache

&#x20;   ↓

Docker/application layer

```



The origin is not intended to be directly exposed to the Internet.



\## HTTPS



Apache terminates HTTPS on port `443`.



A Cloudflare Origin CA certificate is installed outside the Git repository.



Certificate material is stored under:



```text

/etc/ssl/teragram/

├── origin.crt

└── origin.key

```



The private key is restricted and must never be committed to source control.



The local HTTPS origin was verified successfully using a localhost certificate-resolution test.



\## Cloudflare Tunnel



The Teragram tunnel is locally managed.



Both public website hostnames use the same tunnel and connect to the local Apache HTTPS listener.



The tunnel configuration uses:



```text

https://localhost:443

```



with the origin server name configured to match the certificate.



Tunnel ingress validation returned:



```text

OK

```



The `cloudflared` service was verified as:



```text

active

```



\## DNS



The previous direct DNS path to the origin was removed.



Public website DNS now routes through Cloudflare and the Cloudflare Tunnel.



The server's DNS configuration was updated to use reliable public DNS resolvers after local DNS resolution was found to be inconsistent.



DNS resolution was subsequently verified successfully.



\## Apache



Apache configuration was validated successfully:



```text

Syntax OK

```



Apache is listening on HTTPS port `443`.



The default HTTP virtual host was disabled in favour of the Teragram HTTPS configuration.



\## Docker



Docker is installed and operational.



Container resources are organised under:



```text

/opt/containers/

├── projects/

├── data/

└── backups/

```



Application services should remain private and should not publish host ports unless there is an explicit requirement.



\## Security Baseline



The current architecture follows these principles:



\* Cloudflare Tunnel instead of direct inbound web exposure

\* HTTPS between Cloudflare and Apache

\* Private application/container networking

\* Least privilege

\* Minimal exposed services

\* Secrets kept outside source control

\* Deployment-specific identifiers excluded from public documentation

\* Rebuildability as a design goal



\## Backup



The server backup strategy has been validated.



Backups are maintained separately from the public Git repository.



Recovery-critical secrets and credentials are not stored in the public repository.



\## Current Status



\### Completed



\* \[x] Raspberry Pi OS rebuilt on NVMe

\* \[x] Base operating system verified

\* \[x] Network configuration verified

\* \[x] DNS resolution verified

\* \[x] Apache installed

\* \[x] Apache HTTPS configured

\* \[x] Origin CA certificate installed

\* \[x] Local HTTPS origin verified

\* \[x] Cloudflare Tunnel configured

\* \[x] Tunnel ingress validated

\* \[x] Public DNS routed through Cloudflare

\* \[x] Docker installed

\* \[x] Container directory structure created

\* \[x] Backup strategy validated

\* \[x] Public platform documentation updated



\### Next



The next major server/application milestone is the deployment of the containerised Teragram application.



Future work includes:



\* Application container deployment

\* Private Docker networking

\* Persistent application data

\* Database deployment

\* Health checks

\* Monitoring

\* Alerting

\* Deployment automation

\* Infrastructure as Code



\## Documentation Rule



This baseline records the \*\*verified state of the server on 7 September 2026\*\*.



Detailed procedures belong in their respective Buildbook sections:



\* `01-foundation/` — base server and operating system

\* `02-security/` — security hardening

\* `03-web-server/` — Apache and HTTPS

\* `04-cloudflare/` — Cloudflare and Tunnel

\* `05-backups/` — backup procedures

\* `06-monitoring/` — monitoring and observability

\* `07-disaster-recovery/` — rebuild and recovery procedures



