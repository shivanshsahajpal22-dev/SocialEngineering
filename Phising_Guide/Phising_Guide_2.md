# PHISHING AND RED TEAM INFRA SETUP GUIDE 

`Email · SMS · Phone · Other operations — cloud-hosted, built to stay out of spam and phishing filters and not get tracked and flagged.` 

> One guide, three self-contained sections. Each section (Email / SMS / Phone) repeats the shared foundation in short form so you can jump straight to the channel you need.

`We will start by, in general, red team infra before specifically molding it for the social engineering-based attacks.`

--- 

## Pre-Engagement Authorization — Infrastructure-Relevant Documentation

Before any infrastructure is provisioned, the following documents must exist, be signed, and be read and understood by everyone on the infra/ops team. This section covers only the documents that directly bound or govern infrastructure decisions — not the full legal/business paperwork stack (MSA, NDA, payment terms, etc.), which sits outside infra scope.

---

### 1. Rules of Engagement (RoE) — Infra-Relevant Sections

The RoE is the single most important document for infra build decisions. Before provisioning anything, the infra lead must extract and confirm the following from it:

- [ ] **In-scope IP ranges, domains, and systems** — the exact boundaries infrastructure is allowed to target or interact with
- [ ] **Explicitly out-of-scope systems** — written down separately from "everything else," since ambiguity here is the most common source of accidental scope violations
- [ ] **Engagement start and end date/time** — infra must not go live before this window opens and must be torn down at or before it closes
- [ ] **Permitted techniques** — confirm phishing/SMS/phone infra is actually authorized if being built; don't assume
- [ ] **Explicitly excluded techniques** — anything the client has ruled out (e.g., no physical social engineering, no calling specific departments, no targeting specific individuals)
- [ ] **Data handling instructions specific to infra** — what to do with captured credentials, screenshots, or session data if the infra captures any during the test
- [ ] **Any client-specified infra constraints** — e.g., client requires all infra hosted in a specific country/cloud region, or requires use of client-provided IP space for sending

**Process requirement:** the infra lead should produce a short, written "scope summary" distilled from the RoE before build starts, and have it reviewed by at least one other team member — not rely on everyone independently reading and interpreting the full RoE document themselves.

---

### 2. Written Authorization Letter ("Get Out of Jail Free" Letter)

- Must be signed by someone with actual authority to authorize the testing (typically a named executive, CISO, or legal representative of the client) — not just any employee contact.
- Must specifically reference the infrastructure/testing activity being authorized, the date range, and the engagement name/ID.
- A copy (physical or digital) should be accessible to every operator standing up or operating infrastructure during the engagement — not just held by the engagement lead.
- Confirm this letter is current and not a reused/expired letter from a prior engagement with the same client — authorization does not automatically carry over between engagements.

---

### 3. Deconfliction Contact Confirmation

Before infra goes live, confirm in writing (email is sufficient) that:

- [ ] A named contact (or small group) on the client's security/SOC side knows the engagement window and has a way to reach the red team lead if activity is detected
- [ ] The red team lead has a way to reach that same contact if something unexpected happens on the infra side (e.g., infra accidentally touches an out-of-scope system)
- [ ] A verification method (code word, reference number, or ticket ID) is agreed in advance, so a phone-based deconfliction call can confirm authenticity quickly without revealing operational details over an insecure channel

---

### 4. Data Handling / Confidentiality Terms (Infra-Relevant Extract)

Pull only the parts relevant to infrastructure from the broader NDA/confidentiality agreement:

- [ ] Retention period for any data captured by infra (credentials, call recordings, SMS replies, screenshots)
- [ ] Where captured data may be stored (e.g., client may require data stay within a specific region or not leave a specific cloud account)
- [ ] Destruction requirements and deadline after engagement end
- [ ] Who on the team is authorized to access captured data

---

### 5. Pre-Build Sign-Off Checklist

Before `terraform apply` (or any manual provisioning) begins, the infra lead confirms and documents (e.g., in a ticket or engagement tracker) that:

- [ ] RoE has been read in full by the infra lead and summarized for the team
- [ ] Authorization letter is signed, current, and accessible to the team
- [ ] Deconfliction contact and verification method are confirmed in writing
- [ ] Data handling terms relevant to infra are understood by everyone who will touch captured data
- [ ] Engagement start/end dates are entered into the team's calendar with a teardown reminder set for the end date
- [ ] Scope boundaries (in-scope/out-of-scope) are documented somewhere the whole team can reference during the build, not just known by the lead

---

### 6. Ongoing Compliance During the Engagement

- Re-check the RoE if the engagement needs to extend (new dates require a new/updated authorization, not an assumption that the original one stretches)
- If infra needs to touch a system not clearly covered in the original scope, pause and get written confirmation before proceeding — don't interpret ambiguity in favor of proceeding
- Keep a timestamped log of infra actions (stand-up, config changes, teardown) that can be cross-referenced against the RoE's authorized window if questioned later

---

### Why This Matters for Infra Specifically

Every infra decision downstream — which IPs get provisioned, which domains get registered, how long anything stays live — is only lawful and professionally defensible because it maps back to these documents. Building infra before this checklist is complete means the activity has no documented authorization basis, regardless of intent.

---

## Manual Red Team Infrastructure Setup (Tool-Independent)

This guide walks through building a redirector + teamserver + domain + TLS stack by hand — the same outcome tools like Red Baron automate, but without depending on a single tool's maintenance status, provider compatibility, or bundled plugin versions.

---

### 1. Architecture Overview

A resilient minimal setup has three logical tiers, with optional layers for scale and additional campaign types:

```
Target --> DNS (domain) --> Redirector(s) (public-facing) --> Teamserver (C2 backend)
                                      |
                              (optional: CDN/domain fronting layer)

Phishing track (parallel): Mail server --> SMTP relay --> Landing page redirector
```

- **Redirector**: disposable, cheap VPS running a reverse proxy. Filters traffic by user-agent, URI, or header and forwards only legitimate C2 traffic to the teamserver; everything else gets redirected to a decoy site or dropped.
- **Teamserver**: runs the actual C2 framework (Cobalt Strike, Sliver, Havoc, Mythic, etc.). Never exposed directly to the internet — only reachable from known redirector IP(s) and operator SSH.
- **Domain + TLS**: a domain pointed at the redirector with a valid certificate so traffic blends in as normal HTTPS.
- **(Optional) CDN/domain fronting layer**: adds a layer like CloudFront/Azure CDN in front of the redirector for additional attribution resistance.
- **(Optional) Phishing infrastructure**: a separate mail-sending server and landing-page host, isolated from the C2 chain so burning one doesn't burn the other.

**Design principles to carry through every step:**
- Assume every host is disposable — nothing important should live only on a redirector.
- One redirector per domain/campaign where possible, to limit blast radius if one gets burned.
- Keep teamserver credentials, logs, and loot off the redirector entirely.
- Use separate cloud accounts/billing per client engagement where feasible, to avoid cross-contamination of infrastructure history.
- Keep phishing infrastructure (mail/landing pages) segregated from C2 infrastructure — a phishing domain getting reported shouldn't expose the C2 channel.

---

### 2. Pre-Engagement Planning (OPSEC Considerations)

Before provisioning anything:

- **Domain age/category**: Freshly registered domains are often flagged by proxy categorization engines (Zscaler, Bluecoat/Symantec, Palo Alto URL filtering) as "newly registered" or "uncategorized" — both commonly blocked by default policies. Where the engagement allows it, use:
  - Aged domains (purchased from expired-domain marketplaces) with existing category history, or
  - Submit new domains for categorization in advance via the vendor's self-service portal (Palo Alto, Zscaler, Bluecoat all offer this) so they land in a benign category (e.g., "Technology", "Business") well before the engagement starts.
- **ASN/IP reputation**: Cloud provider IP ranges (DigitalOcean, Linode, AWS) are commonly flagged by threat-intel feeds as "hosting provider" / "cloud" categories. Rotating providers across redirectors mitigates a single ASN getting blocklisted engagement-wide.
- **SSL cert transparency logs**: Let's Encrypt certs are logged publicly in Certificate Transparency (CT) logs the moment they're issued. If stealth against a well-resourced blue team matters:
  - Issue the cert close to go-live rather than weeks in advance.
  - Monitor CT logs yourself (e.g., via `crt.sh`) for your own domains to know what a defender watching the same logs would see.
- **Legal/compliance groundwork**: Confirm signed rules of engagement (RoE), authorization letter, and scope boundaries are in hand before any infrastructure goes live. Keep a copy of the signed authorization accessible to the ops team in case infrastructure gets flagged by a third-party abuse team (hosting provider, registrar) mid-engagement.
- **Document everything**: IPs, domains, cert serials, and timestamps should be logged as you go for the final engagement report and for deconfliction with the blue team/SOC if required.

---

### 3. Provision the Hosts

Spin up VPS instances manually through your provider's console, CLI, or API — no IaC tool required.

```bash
# Example: DigitalOcean CLI (doctl)
doctl compute droplet create redirector-01 \
  --region nyc3 --size s-1vcpu-1gb --image ubuntu-22-04-x64 \
  --ssh-keys <your-ssh-key-id>

doctl compute droplet create teamserver-01 \
  --region nyc3 --size s-2vcpu-4gb --image ubuntu-22-04-x64 \
  --ssh-keys <your-ssh-key-id>
```

```bash
# Example: AWS CLI
aws ec2 run-instances \
  --image-id ami-0abcdef1234567890 \
  --instance-type t3.micro \
  --key-name operator-key \
  --security-group-ids sg-0123456789abcdef0 \
  --subnet-id subnet-0123456789abcdef0 \
  --tag-specifications 'ResourceType=instance,Tags=[{Key=Name,Value=redirector-01}]'
```

**Harden SSH access immediately on both hosts:**

```bash
sed -i 's/#PasswordAuthentication yes/PasswordAuthentication no/' /etc/ssh/sshd_config
sed -i 's/#PermitRootLogin prohibit-password/PermitRootLogin no/' /etc/ssh/sshd_config
systemctl restart sshd
```

Consider moving SSH to a non-default port and installing `fail2ban` to blunt automated scanning:

```bash
apt install -y fail2ban
systemctl enable --now fail2ban
```

**Lock down the teamserver's firewall so only the redirector(s) and operator IP can reach it:**

```bash
ufw default deny incoming
ufw default allow outgoing
ufw allow from <redirector_ip> to any port <c2_port>
ufw allow from <operator_ip> to any port 22
ufw enable
```

---

### 4. Operator Access (VPN/Bastion Layer)

Avoid SSHing into teamservers directly from operator home/office IPs where possible — use an intermediary:

- Stand up a lightweight **WireGuard** VPN endpoint that all operators connect through, so the teamserver firewall only ever needs to allow one stable VPN IP rather than every operator's changing home IP.

```bash
apt install -y wireguard
wg genkey | tee privatekey | wg pubkey > publickey
```

- Alternatively, use a hardened **bastion host** as a single SSH jump point, with MFA enforced on the bastion itself, and no direct SSH from the internet to the teamserver at all.
- Rotate operator SSH keys at the start of each engagement rather than reusing a long-lived key pair across clients.

---

### 5. Register and Point a Domain

Register through any registrar (Namecheap, Porkbun, GoDaddy) manually via web UI or API, then point an A record (and optionally a CNAME for a subdomain) at the redirector:

```
Type: A
Name: @ (or subdomain, e.g. "cdn")
Value: <redirector_public_ip>
TTL: 300
```

If using multiple redirectors for redundancy, create multiple A records (round-robin DNS) or use a DNS failover service:

```
Type: A
Name: @
Value: <redirector_01_ip>

Type: A
Name: @
Value: <redirector_02_ip>
```

**Submit the domain for proxy categorization** (see Section 2) as early in the engagement timeline as your rules of engagement allow.

---

### 6. Issue a TLS Certificate

Use **Certbot** directly on the redirector instead of relying on a Terraform ACME provider:

```bash
apt update && apt install -y certbot python3-certbot-apache
certbot --apache -d yourdomain.com --non-interactive --agree-tos -m you@example.com
```

For Nginx instead of Apache:

```bash
apt install -y certbot python3-certbot-nginx
certbot --nginx -d yourdomain.com --non-interactive --agree-tos -m you@example.com
```

**Verify auto-renewal is scheduled:**

```bash
systemctl list-timers | grep certbot
certbot renew --dry-run
```

**Rate limits:** Let's Encrypt enforces 50 certificates per registered domain per week and 5 duplicate certificates per week — plan cert issuance accordingly if rotating domains frequently.

---

### 7. Configure the Redirector

**Option A — Apache with `mod_rewrite`:**

```bash
apt install -y apache2
a2enmod rewrite proxy proxy_http ssl
```

`/etc/apache2/sites-enabled/000-default-le-ssl.conf`:

```apache
<VirtualHost *:443>
    ServerName yourdomain.com

    RewriteEngine On
    RewriteCond %{HTTP_USER_AGENT} !^Mozilla/5\.0\ \(Windows\ NT\ 10\.0.*$
    RewriteRule ^.*$ https://www.decoy-site.com/ [L,R=302]

    RewriteCond %{HTTP_USER_AGENT} ^Mozilla/5\.0\ \(Windows\ NT\ 10\.0.*$
    RewriteRule ^(.*)$ https://<teamserver_ip>:<c2_port>$1 [P,L]

    SSLEngine on
    SSLCertificateFile /etc/letsencrypt/live/yourdomain.com/fullchain.pem
    SSLCertificateKeyFile /etc/letsencrypt/live/yourdomain.com/privkey.pem

    ServerTokens Prod
    ServerSignature Off
</VirtualHost>
```

```bash
systemctl restart apache2
```

**Option B — Nginx (lighter-weight alternative):**

```nginx
server {
    listen 443 ssl;
    server_name yourdomain.com;

    ssl_certificate /etc/letsencrypt/live/yourdomain.com/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/yourdomain.com/privkey.pem;

    location / {
        if ($http_user_agent !~* "Mozilla/5.0 \(Windows NT 10.0") {
            return 302 https://www.decoy-site.com/;
        }
        proxy_pass https://<teamserver_ip>:<c2_port>;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }

    server_tokens off;
}
```

```bash
systemctl restart nginx
```

**Redundancy tip:** stand up a second redirector pointed at a backup domain or secondary A record, so a single blocked/burned redirector doesn't take down the whole engagement.

---

### 8. (Optional) Add a CDN/Domain Fronting Layer

For additional attribution resistance, place a CDN (e.g., CloudFront, Azure CDN, Fastly) in front of the redirector:

1. Create a CDN distribution with the redirector as the origin.
2. Use a high-reputation "front domain" already hosted on the same CDN as the TLS SNI value, while routing the `Host` header to your actual domain internally.
3. Confirm this complies with your engagement's rules of engagement — domain fronting abuses provider trust relationships, and some providers (notably AWS and Azure) have actively restricted or blocked it, so test against your specific CDN before relying on it operationally.

---

### 9. Deploy the C2 Teamserver

Install and launch your chosen framework directly on the teamserver host:

```bash
# Example: Sliver
curl https://sliver.sh/install | sudo bash
sliver-server
```

```bash
# Example: Cobalt Strike (licensed install, not downloadable publicly)
./teamserver <teamserver_ip> <password> /path/to/malleable.profile
```

Bind listeners to localhost or the internal interface only — all public-facing traffic should route exclusively through the redirector.

---

### 10. Phishing Infrastructure (If In Scope)

If the engagement includes phishing as an initial access vector, build this as a **separate, isolated chain**:

- **Mail-sending server**: a dedicated VPS (not the C2 teamserver) running something like `Postfix`, configured with:
  - **SPF** record authorizing the sending IP
  - **DKIM** signing enabled
  - **DMARC** record (even a permissive `p=none` policy helps deliverability)
- **Landing page host**: a separate redirector/host serving the credential-harvesting or payload-delivery page, decoupled from the C2 redirector so a reported phishing domain doesn't expose the C2 domain.
- **Link tracking**: if click-tracking is needed, host it on the landing page infrastructure, not the mail server, to limit what a abuse report can trace back to.

Keep mail, landing page, and C2 on separate domains and separate hosts entirely — this is one of the most common infra-isolation mistakes in rushed engagement setups.

---

### 11. Monitoring and Alerting

Set up basic health checks so a burned or dead redirector doesn't go unnoticed mid-engagement:

```bash
# Simple cron-based healthcheck hitting the redirector and alerting on failure
*/5 * * * * curl -sf https://yourdomain.com/health || echo "Redirector down" | mail -s "ALERT" ops@example.com
```

For more robust setups, point an external uptime monitor (self-hosted Uptime Kuma, or similar) at each redirector's health endpoint so the team gets paged rather than discovering an outage from a stalled beacon.

---

### 12. Logging and Telemetry

Since you're not using a packaged tool's built-in reporting, set this up manually:

- **Centralize redirector + teamserver logs** to a separate logging host (not the teamserver itself) using `rsyslog` forwarding or an ELK stack if search/visualization is needed:

```bash
# On redirector/teamserver: forward logs via rsyslog
echo "*.* @<logging_host_ip>:514" >> /etc/rsyslog.conf
systemctl restart rsyslog
```

- Log every beacon/implant check-in, redirector hit, and cert issuance with timestamps — this becomes part of the final engagement report and supports deconfliction if the blue team flags activity mid-engagement.
- Keep a simple append-only log file per host (`/var/log/redteam-infra.log`) tracking what was stood up, when, and by whom.

---

### 13. Making It Repeatable Without a Framework Dependency

Wrap the manual steps in scripts you own and version yourself.

**Bash approach:**

```bash
#!/usr/bin/env bash
set -euo pipefail

DOMAIN="$1"
TEAMSERVER_IP="$2"
C2_PORT="$3"

apt update && apt install -y apache2 certbot python3-certbot-apache fail2ban
a2enmod rewrite proxy proxy_http ssl
certbot --apache -d "$DOMAIN" --non-interactive --agree-tos -m ops@example.com
envsubst < redirector.conf.template > /etc/apache2/sites-enabled/000-default-le-ssl.conf
systemctl restart apache2
systemctl enable --now fail2ban
```

**Ansible approach (more maintainable across engagements):**

```yaml
- hosts: redirector
  become: true
  vars:
    domain: "{{ redirector_domain }}"
    teamserver_ip: "{{ c2_backend_ip }}"
    c2_port: "{{ c2_backend_port }}"
  tasks:
    - name: Install required packages
      apt:
        name: [apache2, certbot, python3-certbot-apache, fail2ban]
        state: present
        update_cache: true

    - name: Enable required Apache modules
      command: a2enmod {{ item }}
      loop: [rewrite, proxy, proxy_http, ssl]

    - name: Issue TLS certificate
      command: >
        certbot --apache -d {{ domain }} --non-interactive
        --agree-tos -m ops@example.com

    - name: Template redirector vhost config
      template:
        src: redirector.conf.j2
        dest: /etc/apache2/sites-enabled/000-default-le-ssl.conf
      notify: restart apache

    - name: Ensure fail2ban is running
      service:
        name: fail2ban
        state: started
        enabled: true

  handlers:
    - name: restart apache
      service:
        name: apache2
        state: restarted
```

Keep these scripts/playbooks in a private git repository, parameterized per engagement (domain, teamserver IP, C2 port, user-agent string) so standing up new infra is a config change, not a rewrite.

---

### 14. Cost Tracking

Without a tool auto-tagging resources, track spend manually to avoid surprise bills or orphaned infrastructure after an engagement:

- Tag every resource with the engagement/client name in the provider console at creation time.
- Maintain a simple spreadsheet or ledger of: host, provider, creation date, monthly cost, teardown date.
- Set a calendar reminder tied to the engagement end date to confirm teardown actually happened — orphaned VPS instances are a common source of unplanned cost and lingering attack surface.

---

### 15. Teardown and Cleanup

Tear down deliberately and in order:

```bash
# 1. Revoke the cert if the domain won't be reused
certbot revoke --cert-path /etc/letsencrypt/live/yourdomain.com/cert.pem

# 2. Remove DNS records via registrar UI/API

# 3. Destroy the compute instances
doctl compute droplet delete redirector-01 teamserver-01
```

**Before destroying hosts:**
- Pull final logs off the redirector/teamserver for the engagement report.
- Securely wipe sensitive data if the host isn't being destroyed outright (`shred` or provider-level disk wipe on reuse).
- Confirm with the client whether domains/infrastructure should be retained for a retest window before fully decommissioning.
- Close out any categorization submissions or abuse-team correspondence tied to the domain/IPs.

---

### 16. Summary Checklist

| Step | Manual equivalent |
|---|---|
| Host provisioning | Provider CLI/console instead of Terraform `apply` |
| SSH/firewall hardening | Manual `sshd_config` + `ufw`/`fail2ban` |
| Operator access | WireGuard VPN or bastion host instead of direct SSH |
| Domain + DNS | Registrar UI/API instead of GoDaddy Terraform provider |
| Domain categorization | Manual submission to proxy vendor portals |
| TLS cert | Certbot directly instead of ACME Terraform provider |
| Redirector config | Hand-written Apache/Nginx config |
| CDN/domain fronting | Manual CDN distribution setup (optional) |
| Phishing infra | Separate mail/landing-page hosts, isolated from C2 |
| Monitoring | Cron health checks or self-hosted uptime monitor |
| Logging | `rsyslog`/ELK forwarding instead of built-in tool reporting |
| Repeatability | Your own Ansible/Bash scripts, version-controlled |
| Cost tracking | Manual tagging and a spend ledger |
| Teardown | Manual cert revoke, DNS cleanup, instance deletion |

This keeps every component under your own version control and audit trail, at the cost of more upfront setup time per engagement compared to a single automated `apply`/`destroy` cycle.

---
## Additional Infrastructure Components (Advanced Build-Out)

The sections above cover the baseline redirector → teamserver chain. A full professional build typically extends this with the following.

### 1. Payload Delivery Infrastructure

Separate payload hosting from your C2 redirector chain entirely — a file-hosting layer serving initial-access artifacts (droppers, macros, LNKs) should not share infrastructure with your ongoing C2 traffic.

- Stand up a dedicated **staging host** for serving first-stage payloads, isolated from both the C2 redirector and the teamserver.
- Use short-lived, randomized URIs per payload rather than static, predictable paths — reduces the chance a single discovered link exposes your whole delivery mechanism.
- Serve payloads over HTTPS with the same domain-categorization and cert discipline as your C2 domains (see Section 2 of the main guide).
- Log every payload request (timestamp, source IP, user-agent) separately from C2 logs, so a compromised staging host doesn't automatically expose ongoing C2 sessions.

### 2. Short-Haul Staging C2 vs. Long-Haul C2

Distinguish between the C2 channel used for initial execution/staging and the channel used for sustained, long-term access:

| Channel | Purpose | Characteristics |
|---|---|---|
| **Staging/short-haul** | Initial beacon callback immediately after execution | Noisy, used briefly, burned readily, tied to the specific payload delivery |
| **Long-haul** | Sustained post-exploitation access | Quiet, low-and-slow check-in intervals, jitter enabled, separate domain/redirector from staging |

Route staged payloads to immediately migrate/stage into the long-haul listener rather than persisting on the loud staging channel. Use separate domains and separate redirectors for each so burning the staging infrastructure (which gets hit first and is most exposed) doesn't compromise the long-haul channel.

### 3. Multiple C2 Channel Diversity

Don't rely on a single transport. Build redundancy across protocol types so a block on one doesn't end the engagement:

- **Primary**: HTTP/S listener (as covered in the main guide)
- **Secondary**: DNS-based listener, for environments with aggressive HTTP/S egress filtering
- **Internal-only**: SMB/named-pipe listener for peer-to-peer C2 once inside the network (no direct internet egress required for this one)

Each transport should have its own redirector/infrastructure path where it touches the internet — don't funnel DNS and HTTP/S C2 through the same redirector host.

### 4. Internal Pivoting Infrastructure

Once initial access is established, infrastructure needs extend inward:

- **SOCKS proxy**: most C2 frameworks support spinning up a SOCKS proxy through an active implant, letting operators tunnel tools (e.g., for further enumeration) through the compromised host.
- **Pivot listeners**: a secondary listener bound to an internal interface on a compromised host, allowing other internal hosts to beacon to it instead of back out to the internet — critical in segmented networks where only one host has egress.
- **Peer-to-peer C2 chaining**: implants relay through each other (host A relays to host B's C2 traffic) to minimize the number of hosts with direct external C2 callbacks.

Document the pivot chain topology as it's built during the engagement — this becomes part of the attack-path narrative in the final report.

### 5. Malleable / Traffic-Shaping Profiles

Customizing the C2 traffic signature itself is often as important as the redirector:

- **Cobalt Strike**: malleable C2 profiles define HTTP header structure, URI patterns, and timing to mimic legitimate traffic (e.g., shaping Beacon traffic to resemble a known SaaS API or CDN pattern).
- **Sliver / Mythic**: equivalent C2 profile customization through their respective profile/config systems.
- Build profiles in advance and test them against the client's actual security stack where possible (or a representative EDR/proxy in a lab) rather than relying on default profiles, which are the most heavily signatured by defensive products.
- Version-control your profiles like any other piece of infra-as-code, so proven profiles can be reused (with domain/IOC values swapped) across engagements.

### 6. Payload Build and Signing Infrastructure

Define where and how payloads/loaders are actually produced:

- A dedicated, isolated **build host** (not the teamserver) for compiling loaders/droppers — keeps build artifacts and toolchains off infrastructure that's directly internet-facing.
- If code-signing certificates are used (common for reducing AV/EDR friction on signed binaries), store signing keys in a secrets manager, never on the build host itself long-term.
- Keep a build log mapping payload hash → build date → engagement, both for your own tracking and for inclusion in client reporting (IOCs delivered at the end of the engagement).

### 7. Team Collaboration and Data Handling

Operational data (loot, credentials, screenshots, session logs) needs its own handling plan, separate from C2 infrastructure:

- Centralize loot in an encrypted-at-rest store (not left sitting on the teamserver indefinitely).
- Enforce access controls so only engagement operators can reach the loot store — not the whole red team org.
- Define a retention and destruction policy tied to the engagement's end date and the client's data-handling requirements.
- If using a C2 framework's built-in multiplayer/logging features (Cobalt Strike's event log, Sliver's multiplayer mode), still mirror critical data to your own controlled store rather than relying solely on the teamserver's local storage.

### 8. Deconfliction Channel with the Blue Team

Most engagement rules of engagement require a live deconfliction mechanism:

- Establish a direct contact channel (phone number, dedicated email, or ticketing system) with the client's SOC/blue team lead before infrastructure goes live, so activity can be confirmed as authorized if flagged mid-engagement.
- Keep a timestamped log of all offensive actions (not just beacon check-ins) that can be cross-referenced quickly during a deconfliction call.
- Agree in advance on a code word or reference number tied to the engagement, so verification over the phone can happen fast without revealing operational details over an insecure channel.

### 9. Infrastructure Diagramming for Client Reporting

The final report typically needs a visual map of everything built:

- Diagram the full chain: domains → redirectors → teamserver(s) → pivot points → internal hosts reached.
- Include timestamps for when each piece of infrastructure was stood up and torn down.
- Map IOCs (domains, IPs, cert serials, payload hashes) against the diagram so the client's blue team can use it directly for retrospective detection engineering.
- Tools like diagrams.io, Lucidchart, or even a simple Mermaid diagram embedded in the report work — the point is making the attack path legible to people who weren't on the operator side.
  
---

## Social engineering molding: Part 1: for Emails

### Raw Email Infrastructure — Building It Ground-Up (Path A: Cloud ESP + Dedicated IPs)

#### Why this isn't "just another VPS service"

A regular server (your FTP host, a web API, etc.) is synchronous — a request comes in, your server handles it, a response goes back on the same connection, done. Email infra doesn't work that way, and building it like a normal server is the most common mistake.

| | Regular VPS/API server | Email sending infra |
| --- | --- | --- |
| Protocol shape | Request → response, same connection | SMTP is **store-and-forward** — a message is accepted, queued, and delivered later, possibly after retries/hops |
| "Success" means | The response came back 200 OK | The ESP accepted the message into *its* queue — that is NOT delivery, just acceptance |
| How you learn the real outcome | You already know from the response | You find out **later, asynchronously**, via a webhook (delivered/bounced/complained) — there is no live connection to the recipient to check against |
| Scaling model | Scale the server, sessions are often sticky | Scale **workers**, not servers — SMTP/ESP sending has no session state tying a send to one machine, so you can run N workers in parallel with zero coordination |
| Trust model | Firewall rules, auth tokens you control | Trust is **cryptographic and reputation-based**, verified by someone else (the receiving provider) after the fact — you can't just "allow" your own mail in |
| What you're actually hosting | The whole service, end to end | With Path A, the ESP IS your MTA fleet — you're hosting the *client* that calls it, plus the inbound webhook receiver, not the mail server itself |

This last row is the big mental shift: in Path A, you never run an MTA. The ESP (SES/Postmark/SendGrid/Mailgun) runs the actual Postfix-equivalent infrastructure. What you're building is the **pipeline around it** — the part that queues sends, calls their API, and listens for what happened after.

#### The actual components you're building
```
Your app → Queue → Worker service → ESP Send API → (ESP's MTA fleet handles real delivery)
↓
Webhook events (delivered/bounce/complaint/open/click)
↓
Webhook receiver (behind your redirector) → Your event store
```

Four things to actually build, in this order:

**1. The queue**
- Define the message schema up front: recipient, template ID, personalization payload, your own idempotency key, which IP pool/sending identity it should use (system vs. marketing)
- This is what makes the pipeline async-safe — the worker never calls the ESP directly from a web request

**2. The worker service**
- Pulls from the queue, calls the ESP's send API with your stored credentials
- **Critical difference from a normal API integration:** the response you get back means "accepted into their queue," not "delivered." Treat it as step one of a multi-step status, not a final result
- Store that initial state (`submitted`) in your own event store, keyed by the message ID the ESP hands back — you'll need that ID to match up the webhook event that arrives later

**3. The webhook receiver (sits behind the redirector you already built)**
- This is where the *real* delivery outcome shows up — minutes, sometimes hours, after the original send
- Each ESP has its own webhook format and signature method:
  - **SES** — typically via SNS notifications; verify the SNS message signature
  - **SendGrid** — signed webhook with an Ed25519 verification key you configure
  - **Postmark** — webhook with Basic Auth or a shared token you set
  - **Mailgun** — HMAC signature using your webhook signing key
- Translate whichever format into one internal event schema so the rest of your system doesn't care which ESP sent it
- This is the piece the redirector exists to protect — see below

**4. The event store / state machine**
- One record per message, moving through: `queued → submitted → accepted/delivered/bounced/complained → opened/clicked`
- This state machine *is* your real delivery status — not the original API response from step 2
- Feed bounce/complaint transitions straight into your existing do-not-contact service (already built — just wire this pipeline into it, nothing new to design there)

#### Horizontal scaling, the email-specific way

- Run **N worker instances** consuming the same queue in parallel — safe to do because there's no session state linking a send to a specific worker, unlike a sticky-session web server
- Multiple **dedicated IPs = multiple send pools**, not multiple servers. You route system vs. marketing traffic to different IP pools by selecting the ESP's "IP pool" parameter at send time in your worker code — you don't stand up separate machines for this
- Scaling the webhook receiver is a normal stateless-web-server scaling problem (more instances behind the load balancer) — this is the one part of the whole pipeline that *does* behave like a regular server

#### Redirector (Front Door) — build this before any of the above touches the internet

A redirector/reverse proxy is the one thing the internet is allowed to touch directly. It sits in front of your APIs and pages, checks the traffic, then passes it inward. Build and prove this out *before* the real mail-sending pipeline goes live, because the webhook receiver above depends on it being solid from day one.

Put a redirector in front of:
- Your **send API** (where internal apps submit email requests into the queue)
- **Webhook receivers** (where the ESP reports bounces, complaints, opens — the component from step 3 above)
- Your **tracking domain** (`track.example.com`)
- Your **unsubscribe/preference pages**
- The **MTA-STS policy site** (`mta-sts.example.com`)

Checklist:
- [ ] HTTPS only, auto-renewing certs, HTTP redirects to HTTPS
- [ ] Rate limits and request-size limits on every endpoint
- [ ] A web application firewall (WAF) for bots and common attacks
- [ ] Webhook routes: allow only the provider's published IPs where available, **and still verify every signature** — IP allow-listing alone is not authentication
- [ ] Internal services never exposed directly to the internet
- [ ] Two or more redirector instances behind a health check, so one failure doesn't take the front door down
- [ ] Unsubscribe/tracking links stay fast and always up — a broken unsubscribe link *causes* spam complaints
- [ ] Use a cloud-managed load balancer + WAF, or Nginx/Envoy/Caddy, all defined in your IaC

**One hard rule specific to email:** the redirector is for *inbound* traffic (webhooks, pages). It must never sit between your worker service and the ESP's API for outbound sending — that's a direct API call on its own fixed egress, not something to route through a reverse proxy.

**Never:** send mail through rotating, shared, free, or "residential" proxies, or rotate sending IPs to dodge filters — providers read that as hiding, which is itself a phishing-style signal.

**Fast-rebuild notes for the redirector layer** (legitimate disaster recovery, not filter-evasion):
- Keep the redirector's config as its own small IaC module, separate from the worker/queue module, so you can redeploy just the front door in minutes if it's compromised or misconfigured.
- Pre-bake a WAF ruleset and rate-limit profile as versioned config, so a fresh redirector inherits the same protections immediately rather than starting wide-open.
- Keep a second, idle redirector stack (different AZ/region) that can be promoted with a DNS/load-balancer switch, so a redirector failure doesn't take down webhook intake or unsubscribe pages while you rebuild.

#### Build order, start to finish

1. Redirector stood up and tested (health checks, WAF, signature verification stub) — before anything else goes live
2. Queue + message schema defined
3. Worker service built, calling the ESP API, writing `submitted` state
4. Webhook receiver wired in behind the redirector, writing real delivery state
5. Event store / state machine connects the two — this is your actual source of truth for "did it send"
6. Do-not-contact service wired to consume bounce/complaint events from the state machine
7. End-to-end test: one message through the whole pipeline, confirm the state machine updates from `submitted` to `delivered` only after the real webhook arrives — not before

### DEFENCE EVASION TACTICS 

### Domain Aging and Domain Choosing 

`Use a separate subdomain per purpose so one problem can't hurt the others:`

**Here is websites to look for domains and aquire them**

Domain Registrars

- [Cloudflare Registrar](https://www.cloudflare.com/products/registrar/)
- [Namecheap](https://www.namecheap.com/)
- [Porkbun](https://porkbun.com/)
- [Google Domains (via Squarespace)](https://domains.squarespace.com/)
- [AWS Route 53](https://aws.amazon.com/route53/)

Subdomain / DNS Management

> Note: subdomains aren't bought separately — you create them free as DNS records under a domain you already own, using one of these DNS providers.

- [Cloudflare DNS](https://www.cloudflare.com/dns/)
- [AWS Route 53](https://aws.amazon.com/route53/)
- [Namecheap DNS](https://www.namecheap.com/domains/freedns/)
- [Google Cloud DNS](https://cloud.google.com/dns)
- [DNSimple](https://dnsimple.com/)

**Domain hygiene and warm-up (this is what phishing filters actually check):**

- Buy domains **4+ weeks before launch** — brand-new domains look suspicious.
- Lock the registrar, turn on MFA, limit access, use DNSSEC where available. 
- Register all the DNS entries: SPF records, DKIM and DMARC--start at p=none 

**Fast-rebuild note:** keep a domain-provisioning Terraform module (subdomain structure + protective records for unused domains) as a template, so standing up a new brand/domain set is a config change, not a from-scratch DNS session.

### Good Ip range accruing (you want reputable ip)

`for this, have a good select a good ip range that has no bad past history associated with it, if possible, keep good IP range for backup too.`

- Use **dedicated IPs**, not shared: one for system mail, different from the rest of red team infra. 
- **Check every IP before using it** against [Spamhaus](https://check.spamhaus.org), [Barracuda](https://www.barracudacentral.org/lookups), [SORBS](https://www.sorbs.net/lookup.shtml), [MXToolbox](https://mxtoolbox.com/blacklists.aspx). Reject recycled IPs with bad history.
- **Path B only:** confirm outbound port 25 works; run 2+ mail servers; firewall so inbound 25 is only open on the bounce receiver and submission only from your apps; install fail-ban tooling; sign every message and refuse to send unsigned mail; never run an open relay; keep clocks synced.
- **Reverse DNS (PTR):** each IP points to a name (`mta1.mail.example.com`); that name resolves back to the same IP; the server announces the same name. All three must match.
- IPv4 first; IPv6 optional.

### Authentication makers (this is who I am prof)

`NOTE: THIS SECTION IS STILL INCOMPLETE AND NEEDS ATTENTION its covered for the infra angle not the actaul red team infra and still requires reframing`

#### The records — your email "ID card"

| Record | Plain meaning | Key rules |
| --- | --- | --- |
| **SPF** | Lists who may send for your domain | End with `-all` once confident; max 10 lookups (list IPs directly to save lookups); also set on the bounce domain |
| **DKIM** | Tamper-proof signature | 2048-bit key; one new key name per rotation; rotate every 6–12 months; signing domain must match the From domain |
| **DMARC** | What to do when SPF/DKIM fail, plus reports | Roll out in steps (below) |
| **Custom Return-Path** | Your own bounce domain | Needed so SPF *aligns* with the visible From domain |
| **MX** | Where replies/abuse reports land | Every sending domain must also be able to receive |
| **PTR** | Reverse DNS on your sending IP | Set at the IP owner; must match the sending server's name and the A record |
| **MTA-STS + TLS-RPT** | Forces encrypted delivery to you, reports failures | Start in "testing" mode |
| **BIMI** (optional) | Shows your logo in the inbox | Needs DMARC at quarantine/reject |

#### How each one is actually generated

| Record | Who generates the value | What you do |
| --- | --- | --- |
| SPF | You, by listing every sending vendor | Write the string (IPs + `include:`), publish as TXT |
| DKIM | Your mail-sending software/ESP (keygen) | Publish the public-key half as TXT; keep rotating |
| DMARC | You, as a policy decision | Write the policy string, publish as TXT, read the reports |

#### What each one checks on the receiving end

| Check | What it verifies |
| --- | --- |
| SPF | Did this arrive from an IP the domain authorized? |
| DKIM | Was the message signed by this domain, and untampered in transit? |
| Alignment | Does the domain that passed SPF/DKIM match the visible From address? |
| DMARC | Enforces the consequence when SPF/DKIM fail or don't align (none/quarantine/reject) |

#### DMARC rollout

1. `p=none` for 2–4 weeks; read the reports (parsedmarc, dmarcian, Postmark digest).
2. Fix every legitimate sender that fails (staff mail, CRM, helpdesk, billing).
3. Move to `quarantine` at 25% → 50% → 100%.
4. Move to `reject`.

This is also what stops criminals phishing *in your name* — it builds the trust you're trying to protect.

#### Where to verify these are actually working

- [ ] Send a live test to Gmail/Outlook/Yahoo/iCloud, open "Show original" / message source, confirm SPF/DKIM/DMARC all say **pass** and are **aligned**
- [ ] Score the live template on [mail-tester.com](https://www.mail-tester.com/) (aim 9+/10)
- [ ] Verify every record from outside with [MXToolbox](https://mxtoolbox.com/) and `dig`
- [ ] Check IP/domain reputation on [Spamhaus](https://check.spamhaus.org), [Barracuda](https://www.barracudacentral.org/lookups), [SORBS](https://www.sorbs.net/lookup.shtml)

### Warm-up (mandatory for new IPs/domains) 

* **Estimate your busiest day.** It decides IP count, plan size, and warm-up speed.
`Here is how you estimate it`: `number of victims * emails per person (1)`

Warm up each big provider separately — Outlook is slowest to trust you.

| When | Emails/day/IP |
| --- | --- |
| Days 1–3 | 50–200 |
| Days 4–7 | 500–1,000 |
| Week 2 | 2,000–5,000 |
| Week 3 | 10,000–25,000 |
| Week 4 | 50,000–100,000 |
| Weeks 5–8 | Double only while metrics stay clean |

`I would suggest starting even slower than this If the targeted people are fewer than the numbers mentioned here, send genuine emails to real/targeted people before you start with the operation itself; that will work as a good warm-up method.`

**Pause if:** a major provider starts delaying you, complaints pass 0.1%, bounces pass 2%, or any blocklist lists you; this may force you to burn the entire infrastructure and start from scratch again and again! 

### Sending consistency and post content 

The goal: `Automate sending without it *looking* automated-for-abuse: consistent pacing, real personalization, safe speed, and complete, compliant content on every single message — whether it's message #1 or message #1,000,000.`

#### 1. Pacing and Speed (don't let code outrun your reputation)

- [ ] **Rate-limit your own send service**, not just rely on the provider's cap — a bug should never be able to blast at unnatural speed
- [ ] **Respect the warm-up ramp even when fully automated** — automation doesn't skip the schedule, it just executes it reliably:
- [ ] **Smooth bursts, don't eliminate speed** — spread a scheduled campaign over minutes/hours instead of firing the whole list in one instant; sudden all-at-once spikes read as a mass-blast pattern
- [ ] **Queue-based sending** (app → queue → worker → provider), never direct-from-request sending, so volume is naturally throttled and controllable
- [ ] **Per-recipient and per-provider rate limits** — don't hammer Gmail/Outlook harder than your warmed-up reputation allows, even if your own infra could technically go faster

#### 2. Consistency Mechanisms (what stops "bot-loop" behavior)

- [ ] **Idempotency keys on every send** — a retry must never cause a duplicate/triple send to the same recipient
- [ ] **Exponential backoff on retries**, with a cap — not an infinite retry loop
- [ ] **Dead-letter queue** for sends that keep failing, instead of endlessly retrying
- [ ] **Do-not-contact check immediately before every send**, not cached from earlier in the day
- [ ] **Circuit breaker / auto-pause** — if bounce or complaint rate spikes mid-send, the system halts the remaining batch automatically rather than finishing a bad run
- [ ] **Template version locking** — a send references a specific, tested template version, so a mid-flight template edit can't corrupt messages already queued

#### 3. Personalisation (what keeps content from looking like a mass-identical blast)

- [ ] **Dynamic fields minimum**: recipient name, relevant account/order/reference number, relevant date
- [ ] **Content blocks driven by real recipient state** — e.g., different body section if they're a new vs. returning user, rather than one static block for everyone
- [ ] **Avoid 100% identical body text at scale** — even small genuine variation (relevant details, not filler) helps; completely identical mass bodies are a classic abuse pattern
- [ ] **Send-time personalisation where relevant** — e.g., trigger off the recipient's own action (their login, their signup) rather than one global blast time for unrelated-to-them content
- [ ] **Never fake personalization** — don't insert a name field with no real backing data just to "look personalised"; broken merge tags (`Hi {{first_name}}`) are worse than no personalisation at all

#### 4. What every automated email must contain

- [ ] A valid **From** on a domain that passes SPF/DKIM/DMARC; **same sender name every time**
- [ ] `Message-ID` on your own domain, generated uniquely per send
- [ ] `Date` header, accurate send timestamp (not defaulted/stale)
- [ ] Both **plain-text and HTML** versions — never HTML-only
- [ ] **Reply-To** on the same domain, unless there's a specific reason to route replies elsewhere
- [ ] **One-click unsubscribe headers** (`List-Unsubscribe`, `List-Unsubscribe-Post`) on all marketing mail
- [ ] Unsubscribe **honoured within 48 hours, instant is better** — automate this as part of the same pipeline, not a manual weekly job
- [ ] **Footer**: company name, physical address, why they received it
- [ ] **Consistent branding/footer/signature block** across every automated template — inconsistency between templates reads as spoofing
- [ ] **A real, working Reply-To or support contact** — not a no-reply black hole for anything a recipient might need to respond to
- [ ] **Accurate subject line matching body content** — no bait subject lines, even for marketing
- [ ] **List-ID header** on bulk/marketing sends, so providers can group and evaluate the stream consistently
- [ ] **Correct Precedence/Auto-Submitted headers** on fully automated system mail, so providers correctly classify it as transactional vs. bulk where relevant
- [ ] **Content-Language header** if sending in a specific language, for correct filtering/classification

### Constant monitoring 

**All-in-one tools that pull most/all of these into one dashboard:**
- [Validity Everest](https://www.validity.com/everest/) — inbox placement, reputation, engagement, blocklist monitoring, all in one
- [GlockApps](https://glockapps.com/) — inbox placement testing + spam filter testing across providers
- Your ESP's built-in analytics (SendGrid, Mailgun, Postmark) — open/click/bounce/complaint rates live in their dashboard already, no extra tool needed for these four

Engagement Rates (is anyone actually reading this)

- [ ] **Open rate** = opens ÷ delivered — healthy: 15–25%+ (transactional runs higher)
- [ ] **Click rate** = clicks ÷ delivered — healthy: 2–5%+ for marketing
- [ ] **Click-to-open rate** = clicks ÷ opens — shows if content works, independent of subject line
- [ ] **Reply/response rate** = replies ÷ delivered — relevant for transactional/1:1 sends
- [ ] **Unsubscribe rate** = unsubscribes ÷ delivered — keep under 0.5%; rising = content/frequency problem

Delivery & Harm Signals (what actually gets you flagged)

- [ ] **Delivery rate** = delivered ÷ sent — should stay 95%+
- [ ] **Bounce rate** = bounces ÷ sent — keep under 2%; split into:
  - [ ] **Hard bounce rate** (invalid address) — suppress instantly
  - [ ] **Soft bounce rate** (temporary) — retry, suppress after repeats
- [ ] **Spam complaint rate** = complaints ÷ delivered — keep under 0.1%, never reach 0.3%
- [ ] **Block rate** = rejected-at-server ÷ sent — spikes mean a provider is actively blocking you
- [ ] **Inbox placement rate** = landed in inbox ÷ landed anywhere (inbox + spam) — check via GlockApps, not just your ESP

Infrastructure Health

- [ ] **Blocklist status** — IP/domain listed on any blocklist (yes/no)
- [ ] **Domain/IP reputation score** — Google Postmaster Tools, Microsoft SNDS
- [ ] **Authentication pass rate** — % of sends passing SPF/DKIM/DMARC aligned
- [ ] **TLS/MTA-STS failure rate** — from TLS-RPT reports

Daily Reputation Checklist (the few that actually matter most)

- [ ] Check [Spamhaus](https://check.spamhaus.org) — domain and every sending IP
- [ ] Check **Google Postmaster Tools** — domain reputation, IP reputation, spam-rate graph
- [ ] Review yesterday's **complaint rate** vs. 0.1% threshold
- [ ] Review yesterday's **bounce rate** vs. 2% threshold
- [ ] Check [MXToolbox blacklist scan](https://mxtoolbox.com/blacklists.aspx) — catches anything Spamhaus/Postmaster missed

**Alert threshold rule:** trigger an internal alert at **half** of each danger number (e.g. alert at 0.05% complaints, not 0.1%) so you catch the trend before it becomes a block.


#### Bounces and complaints management

```
1. Give every message its own return address (`bounce+ID@bounce.mail.example.com`) so each bounce maps to one send.
2. Route incoming bounces to a small reader for standard bounce reports (DSN) and spam-complaint reports (ARF).
3. Rules: **"user unknown" (5.1.1) → suppress immediately.** Temporary failures retry, then suppress after repeats. **Any complaint → suppress forever.**
4. Monitor `abuse@`, `postmaster@`, `dmarc@`, `tlsrpt@`. Answer abuse reports within 24 hours.
5. Register with **Google Postmaster Tools**, **Microsoft SNDS + JMRP**, and **Yahoo's complaint feedback loop**.
```

#### Small Troubleshooting Guide 

`for when things go wrong.`

| Problem | Likely cause | Fix |
| --- | --- | --- |
| Spam folder despite passing checks | Reputation or content | Check complaints, list quality, links, engagement; run seed tests |
| `dmarc=fail` | Domains don't align | Make signing and return-path domains match the From domain |
| SPF error | More than 10 lookups | Remove includes; list IPs directly |
| Gmail "suspicious" warning | Link/domain reputation, new domain, spoof-like look | Check links vs. Safe Browsing/DBL; no shorteners; fix display name; age the domain |
| Outlook blocks your IP | IP reputation | Register SNDS; send a delisting request; slow down |
| High bounces | Old/unverified list | Verify, suppress, pause |

### Content and Link Quality

Links are the single biggest phishing signal filters look at — more than wording, more than sender reputation alone. Get this wrong and even a perfectly authenticated domain (SPF/DKIM/DMARC all passing) still lands in spam or gets quarantined as phishing.

`use these know tactics to your advantage here , make mail that keep these points during the operations and engagement` 

Why links get you flagged

- **Mismatched link text and destination** — text says one domain, the actual href goes somewhere else
- **Link shorteners** (bit.ly, tinyurl, etc.) — filters can't see the real destination, so they treat it as hiding something
- **Redirect chains** — link A redirects to B redirects to C; each hop looks more suspicious
- **New or unverified linked domains** — a domain with no history, no HTTPS, or no real content behind it
- **Domains flagged elsewhere** — if the linked domain shows up on Google Safe Browsing, Spamhaus DBL, SURBL, or URIBL, your email inherits that risk
- **Too many different domains in one email** — looks like a spray of unrelated links rather than one coherent sender
- **Raw IP links** instead of a domain name — almost always a phishing pattern, filters treat it as such by default
- **QR-code-only links** — filters can't scan what's inside the image, so this is read as *hiding* the destination, not avoiding detection. Treated as a stronger phishing signal, not a weaker one.
- **Urgency/scare wording near the link** — "verify within 24 hours," "confirm your login now," "account suspended" — pairs badly with any link and triggers content-based phishing filters even if the link itself is clean

#### How to actually avoid it

- [ ] Use **your own tracking domain** (`track.yourdomain.com`) over HTTPS instead of a public shortener — one hop, one domain you control
- [ ] Keep link text and the real destination **identical** — never disguise a URL
- [ ] Link only to domains that are **aged, HTTPS, with real content**, not freshly registered or parked pages
- [ ] Check every linked domain against **Google Safe Browsing**, **Spamhaus DBL**, **SURBL**, **URIBL** before a campaign goes out
- [ ] Minimize redirects — one hop maximum from your tracking domain to the final page
- [ ] Stick to **one or two domains per email** (your site + maybe one trusted partner), not a scatter of different links
- [ ] Never link via raw IP address — always a real domain name
- [ ] **Never send a QR-code-only email** — always include the actual clickable/readable link as well, so filters (and humans) can see exactly where it goes
- [ ] Avoid urgency/threat language anywhere near a call-to-action link — state the action plainly instead ("View your receipt" not "Verify immediately or lose access")
- [ ] Keep sender name, domain, and footer **identical across every send** — inconsistency itself reads as spoofing, independent of the links

---

## Test before launch / the launch checklist 

ANTI-DETECTION CHECKLIST BEFORE LAUNCH 

**ID and servers**
- [ ] SPF, DKIM, DMARC all pass *and align* with the visible From domain
- [ ] A record, PTR, and server name match on every IP
- [ ] TLS on all connections
- [ ] IPs checked daily against blocklists
- [ ] Registered with Google, Microsoft, Yahoo tools

**Links — the biggest phishing signal**
- [ ] Link text and real destination are the same domain
- [ ] No link shorteners, free-hosting, or redirect-heavy sites
- [ ] Your own tracking domain over HTTPS, few redirects
- [ ] Every linked domain is old enough, HTTPS, has real content, and is clean on Safe Browsing/Spamhaus DBL/SURBL/URIBL
- [ ] No raw IP links; avoid many different domains in one email

**Content and identity**
- [ ] No scam wording ("verify within 24 hours," threats, "confirm your login")
- [ ] No HTML/ZIP/ISO/EXE attachments; prefer links; no QR-only emails
- [ ] Not image-only; no hidden text, giant fonts, ALL-CAPS subjects, fake Re:/Fwd:
- [ ] Same look, name, and footer every time — inconsistency reads as spoofing

**Reputation**
- [ ] Permission-only lists (double opt-in best); never buy or scrape lists
- [ ] Bounces under 2%, complaints under 0.1% (never reach 0.3%)
- [ ] Stop mailing people who've ignored you for 6 months
- [ ] Verify addresses at signup to avoid spam traps
- [ ] Steady volume — sudden spikes trigger blocks

**Account security**
- [ ] MFA on registrar, DNS, cloud, email service, admin tools
- [ ] DNS as code; delete unused records (prevents subdomain takeover)
- [ ] Outbound content scan to catch hacked templates
- [ ] Watch for look-alike domains registered against your brand

#### Final Pre-Execution Checks (do these right before you hit send)

- [ ] Send a live test to Gmail, Outlook, Yahoo, and iCloud test accounts — open "Show original" / message source and confirm SPF, DKIM, DMARC all say **pass**, not just configured
- [ ] Run the exact live template through **mail-tester.com** — confirm a 9+/10 score
- [ ] Click every link in the actual rendered email (not the template source) — confirm each one resolves to the correct, clean, HTTPS destination
- [ ] Confirm the **unsubscribe link actually works** end-to-end — click it, verify it processes
- [ ] Trigger one deliberate bounce (send to a fake address) and confirm it's suppressed correctly
- [ ] Trigger one deliberate spam-mark (mark your own test send as spam) and confirm the complaint handler suppresses it
- [ ] Confirm **staging cannot reach real recipient addresses** — double-check the environment config, not just assume it
- [ ] Check today's date against blocklists one more time — [Spamhaus](https://check.spamhaus.org), [MXToolbox](https://mxtoolbox.com/blacklists.aspx) — for every sending IP/domain you're about to use
- [ ] Confirm current **complaint rate and bounce rate** from your last batch are still under threshold before sending the next one
- [ ] Confirm you're within your **current warm-up week's volume cap** — not jumping ahead of schedule
- [ ] Double-check the **recipient list was pulled fresh** against the do-not-contact/suppression list, not a cached/stale export
- [ ] Confirm the **sender name, From domain, and footer** match exactly what's been used in every prior send — no last-minute branding changes
- [ ] Have the **rollback/pause mechanism tested and ready** — confirm you can actually halt a send mid-flight if something looks wrong in the first few minutes

`CONSIDER THE OPERATION IS NOW LIVE !!... :)` 

---

## INFRA QUICK BURN & REBUILD ☑️

The goal here is: you can rebuild correctly in hours, not weeks — without ever touching the rotate-to-dodge-filters pattern that gets you blocked.

`First of all, I highly recommend a backup infra with a different domain and provider, pre-ready in the background. Nothing to be done with yet; first go through the diagnostic process, fix your mistake before picking up with the second one.`

### Diagnosis checklist and rerunning 

Hour 1 — Stop and diagnose

- [ ] Pause sending on the flagged channel immediately (email domain/IP, SMS campaign, or phone number)
- [ ] Identify exactly what got flagged (which domain / IP / number / campaign ID)
- [ ] Pull the actual reason, not a guess:
  - [ ] Email: check Spamhaus listing reason at [https://check.spamhaus.org](https://check.spamhaus.org)
  - [ ] Email: check Google Postmaster Tools spam-rate/reputation tab
  - [ ] Email: pull recent bounce report (DSN) and complaint report (ARF)
  - [ ] SMS: check provider's filtered/blocked error codes on recent sends
  - [ ] SMS: check if campaign content matches registered sample messages
  - [ ] Phone: check labelling-company status (Free Caller Registry / Hiya / TNS) for the number
  - [ ] Phone: check recent abandoned-call rate and answer rate
- [ ] Write down the specific cause before doing anything else

Hours 1–4 — Fix the root cause

- [ ] If complaints are high → identify which list/segment generated them, remove/suppress those contacts
- [ ] If bounces are high → run list verification, remove invalid addresses/numbers
- [ ] If content/links were flagged → fix the actual message content or the linked domain
- [ ] If SMS content differs from registered campaign sample → align wording or re-register
- [ ] If phone abandoned-call rate is high → fix dialer pacing, reduce call volume
- [ ] Confirm the underlying cause is actually fixed before re-enabling anything

Hours 2–6 — Fail over to pre-vetted backup (not a new one)

- [ ] Switch to your already-configured backup provider/IP/number
- [ ] Confirm the backup was pre-checked clean (Spamhaus/MXToolbox/Talos for email IPs; labelling status for phone numbers; registration status for SMS)
- [ ] Do NOT spin up a brand-new domain/IP/number under pressure — it starts with zero reputation and can trigger pattern-detection (snowshoeing / number-cycling)
- [ ] Resume sending at reduced volume on the backup, not full volume immediately

After recovery — prevent repeat

- [ ] Re-check complaint rate is under 0.1% and bounce rate under 2% before scaling volume back up
- [ ] Re-warm the affected IP/domain/number gradually, don't jump back to full volume
- [ ] Update your runbook with the specific root cause found, for next time
- [ ] Re-verify your backup/spare infra is still clean and ready for the next incident


* for one tool that do this best is --> [Red Baron](https://github.com/Coalfire-Research/Red-Baron) {profession red team spin up infra tool}

`use this tool once you know how to do everything manually , then this tool would become useful without ever becoming a dependency itself`

### Red Baron — Infrastructure Automation Usage Guide

**Repository:** [github.com/Coalfire-Research/Red-Baron](https://github.com/Coalfire-Research/Red-Baron)

Red Baron is a Terraform-based toolkit that automates the creation of resilient, disposable red team infrastructure — redirectors, teamservers, domains, and TLS certificates — across multiple cloud providers.

#### Prerequisites

- Linux x64 host (Red Baron does not support other platforms)
- Terraform `v0.11.0` or newer
- Accounts/API credentials for the providers you intend to use:
  - AWS / Azure / Google Cloud / DigitalOcean / Linode (compute)
  - GoDaddy (domain registration)
  - Let's Encrypt via ACME (TLS certificates)

#### Installation

```bash
git clone https://github.com/Coalfire-Research/Red-Baron
cd Red-Baron
```

#### Setting Credentials

Export the credentials for whichever providers your configuration uses:

```bash
export AWS_ACCESS_KEY_ID="accesskey"
export AWS_SECRET_ACCESS_KEY="secretkey"
export AWS_DEFAULT_REGION="us-east-1"

export LINODE_API_KEY="apikey"
export DIGITALOCEAN_TOKEN="token"

export GODADDY_API_KEY="gdkey"
export GODADDY_API_SECRET="gdsecret"

export ARM_SUBSCRIPTION_ID="azure_subscription_id"
export ARM_CLIENT_ID="azure_app_id"
export ARM_CLIENT_SECRET="azure_app_password"
export ARM_TENANT_ID="azure_tenant_id"
```

> For Google Cloud Compute, follow the [Terraform GCP provider docs](https://www.terraform.io/docs/providers/google/index.html#configuration-reference) and set the corresponding environment variables.

#### Building a Configuration

Copy an example configuration from the `examples/` directory and adapt it to the engagement:

```bash
cp examples/complete_c2.tf .
```

Edit the copied file to set:
- Target cloud provider(s) per module
- Domain name(s) to register/use
- Redirector type (e.g., Apache `mod_rewrite`) and the user-agent or URI pattern it should filter on
- Teamserver backend address

#### Deploying Infrastructure

```bash
terraform init     # pulls required providers/modules
terraform plan      # review what will be created
terraform apply     # provisions hosts, DNS, certs, and redirector config
```

`terraform apply` provisions the compute instances, registers/updates DNS, issues TLS certificates via ACME, and runs remote provisioning commands to configure the redirector (e.g., installing and configuring Apache with `mod_rewrite`).

#### Tearing Down

Once the engagement is complete, destroy all provisioned infrastructure:

```bash
terraform destroy
```

#### Known Limitations

- Let's Encrypt cert issuance via the TLS challenge method currently does not work due to a limitation in the third-party ACME provider — use the HTTP challenge method instead.
- Only tested on Linux x64; no Windows/macOS support.
- Bundled providers (GoDaddy, ACME, Linode) are third-party/community-maintained and may lag behind current API versions — verify compatibility before an engagement.

#### Reference Material

- [Red Baron Wiki](https://github.com/Coalfire-Research/Red-Baron/wiki) — per-module documentation
- [Terraform Documentation](https://www.terraform.io/docs) — underlying IaC engine
- Rasta Mouse — *Automated Red Team Infrastructure Deployment with Terraform* (original inspiration series)
- bluscreenofjeff — [Red Team Infrastructure Wiki](https://github.com/bluscreenofjeff/Red-Team-Infrastructure-Wiki)
  
`Here are a few tips for email servers specifically.`
- **One Terraform module per concern** (domains/DNS, redirector, mail-server/IP config, queues, alerts) so you can redeploy just the broken piece.
- **A documented "new sending identity" runbook** for *legitimate* new brands/products: domain aging plan, DNS template, warm-up schedule — not for replacing a burned domain to dodge a block.
- **Pre-vetted spare IPs** checked against blocklists *before* you need them, held in reserve for genuine failover (a dedicated IP going bad from someone else's history, not your own sending abuse).
    - where to check -> [click here](https://mxtoolbox.com/blacklists.aspx) and [here](https://check.spamhaus.org/) and where to buy -> `ip ranges are given by the cloud VPS provider themself.`
- **Key rotation as a drill**, not a crisis: practice rotating DKIM keys and API keys on a schedule so doing it after an incident is routine.
- **Config, not manual steps, for DMARC stage changes** — store your current `p=` value in code/version control so rollback after a bad rollout is one commit.

`SINCE NOW YOUR EMAIL INFRA IS UP, LET'S MOVE TO THE NEXT SECTION NOW.....` 

---

`Go to part 3 for the rest of the information.` 
