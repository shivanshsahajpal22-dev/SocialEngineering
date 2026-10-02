# PHISHING AND RED TEAM INFRA SETUP GUIDE 

`Email · SMS · Phone · Other operations — cloud-hosted, built to stay out of spam and phishing filters and not get tracked and flagged.` 

> One guide, three self-contained sections. Each section (Email / SMS / Phone) repeats the shared foundation in short form so you can jump straight to the channel you need.

`We will start by, in general, red team infra before specifically molding it for the social engineering-based attacks.`

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

> Part of a 3-guide set (Email / SMS / Phone). Shared foundation pieces are repeated here in short form so this guide stands alone.

## The one idea behind everything

Inbox providers ask three questions. Fail one and you're "Spam" or "Phishing."

| Question | Plain meaning | How email answers it |
| --- | --- | --- |
| **1. Who are you?** | Proof you're really you | SPF, DKIM, DMARC |
| **2. Do you behave?** | People want your mail | Low complaints, clean list |
| **3. Do you look honest?** | Nothing looks like a scam | Honest links, steady branding |

```
WHAT TO DO ABOUT SPF, DKIM, AND DMARC as red teamer 
[data must be filled here]
```
`Once this is handled, check your own mail to see if it's caught or not on [MXToolbox Email Header Analyzer](https://mxtoolbox.com/EmailHeaders.aspx)` --> this will tell you if you are gonna get caught or not 


## Words you'll see

| Term | Simple meaning |
| --- | --- |
| MTA | The mail server that sends email |
| IP reputation | The trust score inbox providers give your sending IP |
| DNS record | A public note about your domain (who may send for it) |
| SPF / DKIM / DMARC | Approved senders / tamper-proof signature / what to do if checks fail |
| Bounce / complaint | Couldn't deliver / recipient clicked "report spam" |
| Suppression list | "Do not contact again" list |
| Warm-up | Slowly raising volume so providers learn to trust you |
| IaC | Infrastructure as code: servers/records set up from files, not clicks |

---

## 1. Decide first

- **Estimate your busiest day.** It decides IP count, plan size, and warm-up speed.
`Here is how you estimate it`: `number of victims * emails per person (1)`
- **Name one owner** for abuse reports, blocklists, and on-call.
- **Get legal review** of consent, opt-out and footer rules (CAN-SPAM, GDPR, CASL, etc.) — required almost everywhere.

## 2. Cloud setup, so you can change fast

1. Separate **staging and production** cloud accounts. Staging only ever hits a mail catcher (e.g. Mailpit), never real inboxes.
2. **Everything as code** (Terraform): DNS records, servers, queues, alerts — reviewed in Git, applied in minutes.
3. **Secrets manager** for every API key; one key per service per environment.
4. **A queue** (SQS/Pub-Sub) between your app and the mail service. Nothing sends straight from a web request.
5. Your app calls *your own* "send email" service, not the provider directly — so switching providers means changing one place.
6. **A backup email provider**, pre-configured and ready to flip to.
7. **Spending alerts and caps** on the provider account.

## 3. The shared "do-not-contact" brain

- One consent record per person (when, how, what wording).
- One opt-out list, checked by every channel before every send.
- An event log: send, bounce, complaint, reply — searchable for support.
- Every webhook signature verified, or attackers can fake delivery events.

---

## 4. Redirector (Front Door) — put this in front of everything public, *before* you touch the mail server itself

A redirector/reverse proxy is the one thing the internet is allowed to touch directly. It sits in front of your APIs and pages, checks the traffic, then passes it inward. Build and prove this out *before* the real mail-sending setup, because every other piece (webhooks, tracking, unsubscribe) depends on it being solid.

Put a redirector in front of:

- Your **send API** (where internal apps submit email requests)
- **Webhook receivers** (where the provider reports bounces, complaints, opens)
- Your **tracking domain** (`track.example.com`)
- Your **unsubscribe/preference pages**
- The **MTA-STS policy site** (`mta-sts.example.com`)

Checklist:

- [ ] HTTPS only, auto-renewing certs, HTTP redirects to HTTPS
- [ ] Rate limits and request-size limits on every endpoint
- [ ] A web application firewall (WAF) for bots and common attacks
- [ ] Webhook routes: allow only the provider's published IPs where available, **and still verify every signature** — IP allow-listing alone is not authentication
- [ ] Internal servers never exposed directly to the internet
- [ ] Two or more redirector instances behind a health check, so one failure doesn't take the front door down
- [ ] Unsubscribe/tracking links stay fast and always up — a broken unsubscribe link *causes* spam complaints
- [ ] Use a cloud-managed load balancer + WAF, or Nginx/Envoy/Caddy, all defined in your IaC

**One hard rule specific to email:** the redirector is for *inbound* traffic (webhooks, pages). It must never sit between your mail server and the internet for *outbound* delivery — email has to leave from the exact IP that carries your PTR and SPF records. Routing delivery through a proxy breaks that match and tanks deliverability.

**Never:** send mail through rotating, shared, free, or "residential" proxies, or rotate sending IPs to dodge filters — providers read that as hiding, which is itself a phishing-style signal.

**Fast-rebuild notes for the redirector layer** (legitimate disaster recovery, not filter-evasion):

- Keep the redirector's config as its own small IaC module, separate from the mail-server module, so you can redeploy just the front door in minutes if it's compromised or misconfigured.
- Pre-bake a WAF ruleset and rate-limit profile as versioned config, so a fresh redirector inherits the same protections immediately rather than starting wide-open.
- Keep a second, idle redirector stack (different AZ/region) that can be promoted with a DNS/load-balancer switch, so a redirector failure doesn't take down webhook intake or unsubscribe pages while you rebuild.

---

## 5. Choose how to run the mail server itself

| Path | What it is | Pros | Cons |
| --- | --- | --- | --- |
| **A. Cloud email service in your own account** (SES, Postmark, SendGrid, Mailgun) with dedicated IPs | Provider runs the servers; you own domains and settings | Fastest to change, no port-25 hassle, easiest to rebuild | Less low-level control |
| **B. Your own mail servers on cloud machines** (Postfix, KumoMTA) | You run the servers | Full control | You own reputation, blocklists, and 24/7 care; many clouds block outbound port 25 |

**Path A is the right default for "cloud + change fast."** Everything below applies to both; Path B carries the extra work noted in Section 6.

## 6. Domains — the most important step

Use a separate subdomain per purpose so one problem can't hurt the others:

| Name | Use |
| --- | --- |
| `example.com` | Website and staff mailboxes only — **never** bulk mail |
| `mail.example.com` | System emails |
| `news.example.com` | Marketing |
| `bounce.mail.example.com` | Where "could not deliver" replies go |
| `track.example.com` | Your own link-tracking domain |

**Domain hygiene (this is what phishing filters actually check):**

- Buy domains **4+ weeks before launch** — brand-new domains look suspicious.
- Put a **real website** on the root domain: company name, address, contact, privacy policy.
- Lock the registrar, turn on MFA, limit access, use DNSSEC where available.
- No look-alikes of other brands; avoid cheap/abused TLDs.
- **Protect unused domains** you own but don't send from: `SPF v=spf1 -all`, null MX, `DMARC p=reject` — so nobody can spoof them.

**Fast-rebuild note:** keep a domain-provisioning Terraform module (subdomain structure + protective records for unused domains) as a template, so standing up a new brand/domain set is a config change, not a from-scratch DNS session.

## 7. Servers and IPs

- Use **dedicated IPs**, not shared: one for system mail, one or two for marketing, one spare.
- **Check every IP before using it** against Spamhaus, Barracuda, SORBS, MXToolbox. Reject recycled IPs with bad history.
- **Path B only:** confirm outbound port 25 works; run 2+ mail servers; firewall so inbound 25 is only open on the bounce receiver and submission only from your apps; install fail-ban tooling; sign every message and refuse to send unsigned mail; never run an open relay; keep clocks synced.
- **Reverse DNS (PTR):** each IP points to a name (`mta1.mail.example.com`); that name resolves back to the same IP; the server announces the same name. All three must match.
- IPv4 first; IPv6 optional.

## 8. Identity records — your email "ID card"

| Record | Plain meaning | Key rules |
| --- | --- | --- |
| **SPF** | Lists who may send for your domain | End with `-all` once confident; max 10 lookups (list IPs directly to save lookups); also set on the bounce domain |
| **DKIM** | Tamper-proof signature | 2048-bit key; one new key name per rotation; rotate every 6–12 months; signing domain must match the From domain |
| **DMARC** | What to do when SPF/DKIM fail, plus reports | Roll out in steps (below) |
| **Custom Return-Path** | Your own bounce domain | Needed so SPF *aligns* with the visible From domain |
| **MX** | Where replies/abuse reports land | Every sending domain must also be able to receive |
| **PTR** | See Section 7 | Set at the IP owner |
| **MTA-STS + TLS-RPT** | Forces encrypted delivery to you, reports failures | Start in "testing" mode |
| **BIMI** (optional) | Shows your logo in the inbox | Needs DMARC at quarantine/reject |

**DMARC rollout:**

1. `p=none` for 2–4 weeks; read the reports (parsedmarc, dmarcian, Postmark digest).
2. Fix every legitimate sender that fails (staff mail, CRM, helpdesk, billing).
3. Move to `quarantine` at 25% → 50% → 100%.
4. Move to `reject`.

This is also what stops criminals phishing *in your name* — it builds the trust you're trying to protect.

## 9. Bounces and complaints

1. Give every message its own return address (`bounce+ID@bounce.mail.example.com`) so each bounce maps to one send.
2. Route incoming bounces to a small reader for standard bounce reports (DSN) and spam-complaint reports (ARF).
3. Rules: **"user unknown" (5.1.1) → suppress immediately.** Temporary failures retry, then suppress after repeats. **Any complaint → suppress forever.**
4. Monitor `abuse@`, `postmaster@`, `dmarc@`, `tlsrpt@`. Answer abuse reports within 24 hours.
5. Register with **Google Postmaster Tools**, **Microsoft SNDS + JMRP**, and **Yahoo's complaint feedback loop**.

## 10. The sending pipeline

App → queue → worker → email service → internet. Events (delivered/bounced/complained) flow back into the shared do-not-contact brain.

- Idempotency keys so a retry never double-sends; retries with growing delays; a dead-letter queue for permanent failures.
- Check the do-not-contact list before *every* send.
- Rate limits per app/customer and per receiving provider; auto-pause if bounces or complaints spike — a stolen key can wreck your reputation in hours.
- Template versions with test renders.
- Never log full bodies or unnecessary personal data.

## 11. What every email must contain

- A valid From on a domain that passes checks; same sender name every time.
- `Message-ID` on your domain, `Date`, and both plain-text + HTML versions.
- Reply-To on the same domain unless there's a real reason not to.
- One-click unsubscribe headers (`List-Unsubscribe`, `List-Unsubscribe-Post`) on all marketing mail, honoured within 48 hours (instant is better).
- Footer: company name, physical address, why they received it.

## 12. Anti-phishing checklist

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

- [ ] Never impersonate another brand or person
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

---

## DEFENCE EVASION TACTICS 

### Domain Aging (You don't want the domain flag)

### Good Ip range accruing (you want reputable ip)

### Warm-up (mandatory for new IPs/domains) ☑️

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

---

## Test before launch

### The final list 
1. Send to Gmail, Outlook, Yahoo, iCloud test accounts; use "Show original" and confirm SPF/DKIM/DMARC all pass.
2. Score with mail-tester.com (aim 9+/10); run an inbox-placement test.
3. Verify all records with MXToolbox and `dig` from outside.
4. Test unsubscribe, a deliberate bounce, and a deliberate spam-mark end to end.
5. Confirm staging genuinely cannot reach real addresses.

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

---

## 16. Monitoring

Watch: delivered/delayed/bounced by provider; complaints; blocklists; Google Postmaster + SNDS; DMARC/TLS reports; certificate, key, and domain expiry; sudden drops in volume (a silent block can look like an empty queue). Alert at **half** of each danger threshold, not at the threshold itself.

## 17. Troubleshooting

| Problem | Likely cause | Fix |
| --- | --- | --- |
| Spam folder despite passing checks | Reputation or content | Check complaints, list quality, links, engagement; run seed tests |
| `dmarc=fail` | Domains don't align | Make signing and return-path domains match the From domain |
| SPF error | More than 10 lookups | Remove includes; list IPs directly |
| Gmail "suspicious" warning | Link/domain reputation, new domain, spoof-like look | Check links vs. Safe Browsing/DBL; no shorteners; fix display name; age the domain |
| Outlook blocks your IP | IP reputation | Register SNDS; send a delisting request; slow down |
| High bounces | Old/unverified list | Verify, suppress, pause |

## 18. Go-live checklist

- [ ] Prod/staging separated; all setup in code
- [ ] Secrets in manager; spending alerts on provider
- [ ] Redirector live with WAF, rate limits, 2+ instances, verified webhook signatures
- [ ] Domains aged; website live; registrar locked; MFA on
- [ ] IPs clean; A, PTR, server name match
- [ ] SPF, DKIM (2048-bit), DMARC published and passing
- [ ] Bounce/complaint handling writes to the do-not-contact list
- [ ] Unsubscribe headers and footer in place
- [ ] Google, Microsoft, Yahoo tools registered
- [ ] Warm-up and DMARC-tightening schedules set
- [ ] Backup provider configured and tested
- [ ] Rebuild runbooks written for: redirector loss, stolen key, blocklisted IP, DKIM rotation

---

## Social engineering molding: Part 1 : for SMS 

> Part of a 3-guide set (Email / SMS / Phone). Shared foundation pieces are repeated here in short form so this guide stands alone.

## The one idea behind everything

Carriers and phones ask three questions. Fail one and you get filtered, blocked, or labelled.

| Question | Plain meaning | How SMS answers it |
| --- | --- | --- |
| **1. Who are you?** | Proof you're really you | Registered brand + campaign |
| **2. Do you behave?** | People want your texts | Opt-in, and STOP actually works |
| **3. Do you look honest?** | Nothing looks like a scam | Real links, no shorteners |

## Words you'll see

| Term | Simple meaning |
| --- | --- |
| 10DLC / DLT | Business registration for text messages (US / India) |
| Brand / campaign | Your registered company identity and the registered *purpose* of a message stream |
| Opt-in / opt-out | Explicit permission to text / the STOP mechanism |
| Throughput | Messages per second your registration is allowed to send |
| IaC | Infrastructure as code: config, numbers, and alerts set up from files, not clicks |

---

## 1. Decide first

- **Split system vs. marketing.** Separate numbers/campaigns/credentials for codes & alerts vs. promotions — trouble on one doesn't stop the other.
- **Estimate your busiest day.** It decides number type (local vs. toll-free vs. short code) and ramp speed.
- **Name one owner** for abuse reports, carrier registrations, and on-call.
- **Get legal review**: TCPA, CASL, India DLT/TRAI, and local consent/opt-out rules apply almost everywhere.

## 2. Cloud setup, so you can change fast

1. Separate **staging and production** — staging uses sandbox numbers only, never real customers.
2. **Everything as code**: queues, throughput settings, webhook config, alerts — reviewed in Git.
3. **Secrets manager** for provider API keys.
4. **A queue** between your app and the SMS send call. Nothing texts straight from a web request.
5. Your app calls *your own* "send SMS" service, not the provider directly — so switching providers is a one-place change.
6. **A second SMS provider/route**, pre-configured, ready as backup.
7. **Spending alerts and caps.**

## 3. The shared "do-not-contact" brain

- One consent record per person (when, how, wording).
- One opt-out list, checked before every send, shared with email/phone where the law or your promise requires it.
- An event log of every send, delivery, reply, and STOP.
- Every inbound webhook (replies, STOP, delivery receipts) signature-verified.

---

## 4. Redirector (Front Door) — build this before the sending pipeline itself

Put a redirector/reverse proxy in front of every public endpoint related to SMS, *before* wiring up the actual send pipeline, since replies and delivery receipts depend on it from day one.

Put a redirector in front of:

- Your **send API** (where internal apps submit SMS requests)
- **Inbound webhook receivers** (STOP/HELP replies, delivery receipts, filtered/blocked events)
- Your **own short link domain** (the one you use instead of a public shortener)
- Any **click-to-text or consent-capture web forms**

Checklist:

- [ ] HTTPS only, auto-renewing certs
- [ ] Rate limits and request-size limits on every endpoint
- [ ] A WAF for bots and common attacks
- [ ] Webhook routes: allow only the provider's published IPs where available, **and still verify every signature**
- [ ] Internal services never exposed directly to the internet
- [ ] Two or more redirector instances with health checks
- [ ] Your short-link domain stays fast and always up — slow or broken links get reported as spam
- [ ] Load balancer + WAF, or Nginx/Envoy/Caddy, defined in IaC

**Never:** route messages through rotating, shared, or "residential" proxies, or constantly rotate sending numbers to dodge carrier filters — carriers read number-hopping as abuse and will block the whole pattern, not just one number.

**Fast-rebuild notes for the redirector layer** (legitimate disaster recovery, not filter-evasion):

- Keep the redirector config as its own IaC module so you can redeploy the front door independently of your SMS send logic if it's compromised or needs scaling.
- Version your WAF/rate-limit rules so a fresh redirector inherits the same protections immediately.
- Keep an idle secondary redirector ready to promote via DNS/load-balancer switch if the primary fails, so inbound STOP/HELP replies are never silently dropped during a rebuild.

---

## 5. Pick provider(s)

Use a cloud messaging provider (Twilio, Telnyx, Vonage, Plivo, Sinch, or your cloud's own messaging service). Prefer one offering: registration help, delivery receipts, one-time-code protection, per-country routing, and signed webhooks. **Connect a second provider as backup.**

## 6. Choose the right sender type

| Sender | Best for | Notes |
| --- | --- | --- |
| **10-digit local number (10DLC)** (US) | Alerts, support, medium marketing | Must register brand + campaign; unregistered traffic is blocked or heavily filtered; speed depends on trust score |
| **Toll-free number** (US/Canada) | Alerts, support, small–medium volume | Verification required; takes days to weeks |
| **Short code** (5–6 digits) | High volume, urgent | Fastest sending; 8–12 weeks carrier approval; costs more |
| **Sender name** (e.g. "ACMEBANK") | Alerts in many countries | Not allowed in US/Canada; some countries require pre-registration |
| **Country-specific** | Everywhere outside the US | Rules vary — e.g. **India requires company, sender names, and every message template registered on DLT**; UK/EU and others have their own rules |

## 7. Registration steps (start these first — they take the longest)

1. Prepare: legal company name, tax ID, address, website, support contact, and a real privacy policy on the site.
2. Register the **brand** with your provider.
3. Register each **campaign** (purpose): who gets texts, how they opted in, sample messages, opt-out wording.
4. Buy or port numbers. Porting takes 1–4 weeks — **don't cancel the old service until it completes.**
5. Use **separate numbers/campaigns** for system vs. marketing messages.
6. Send only what you registered — a different kind of message than what's on file gets blocked.

**Fast-rebuild note:** keep your brand/campaign application details (company info, sample messages, opt-in wording) as a maintained template so registering a *new, legitimate* campaign or porting to a *new* provider is a paperwork exercise, not a research project. This is about shortening legitimate onboarding time — not about re-registering under a new identity after a campaign gets shut down for cause.

## 8. Consent and opt-out (legally required)

- Get clear permission before texting: a plain tick-box or text-in keyword naming your company, message type, frequency, "msg & data rates may apply," and how to stop. **Keep proof.**
- **Never** pre-tick the box, bundle consent into terms, or text a purchased list.
- The first message names your business; every message is clearly from you.
- **STOP** (also STOPALL, UNSUBSCRIBE, CANCEL, END, QUIT) must stop messages immediately with one confirmation reply. **HELP** must reply with contact details.
- Don't text between roughly 9pm and 8am recipient local time (some places are stricter).
- Add numbers to the shared do-not-contact list instantly.

## 9. Content and link rules (what carrier filters actually check)

- **No public link shorteners** (bit.ly, etc.) — use your own short domain on HTTPS.
- Keep links few, pointing to your own real, registered website.
- Be clear who you are; avoid urgent threats or "verify your account" wording.
- Avoid content that gets blocked or needs special approval: illegal items, payday loans, adult content, unlicensed gambling, tobacco/vape, cannabis, firearms, hate, "get rich quick," or debt-relief offers.
- **Never** rotate through many numbers to dodge filters — it's treated as abuse and risks the whole account.
- Keep wording consistent with your registered samples.

## 10. Build

- App → SMS queue → worker → provider, with idempotency keys and growing-delay retries, same pattern as email.
- **Throttle to your registered speed limit**; queue the rest.
- Use delivery receipt webhooks (sent/delivered/failed/filtered) and store them.
- **Protect one-time codes from fraud** ("SMS pumping" — attackers trigger codes to numbers they profit from, and you pay): country allow-lists, per-number and per-IP/device limits, CAPTCHA before sending, short code expiry, and the provider's fraud-guard feature. Alert on any spike.
- Handle inbound replies (STOP/HELP) via webhook; verify signatures.
- Account for long messages: non-standard characters (emoji) shorten the per-segment length and raise cost.

## 11. Ramp up and monitor

- Start with engaged recipients and low volume; raise slowly over 2–4 weeks.
- Watch: delivery rate (aim 95%+), filtered/blocked error codes, opt-out rate, cost per message, spikes by country.
- If a campaign shows many "filtered" results: pause, compare wording/links against what you registered, and contact the provider — don't just switch numbers and retry.

## 12. Fast, repeatable rebuilds (legitimate resilience, not filter evasion)

- **One IaC module per concern** (redirector, queue/throttle config, webhook handlers, fraud-guard rules) so a broken piece can be redeployed alone.
- **Pre-vetted backup numbers/routes** held with your second provider, ready to fail over to if your primary provider has an outage — not as a rotation pool to outrun filtering.
- **A documented new-campaign runbook** for genuinely new, legitimate message streams, so launching one is fast — separate from, and never a substitute for, fixing a campaign that got filtered for cause.
- **Key/credential rotation as a scheduled drill**, so rotating a compromised API key is routine, not a scramble.
- If a number gets labeled or a campaign gets throttled: the fix is content/consent/volume changes plus working with the provider and carrier — not replacing the number to start the trust clock over elsewhere.

## 13. Monitoring

Watch: delivery rate, filtered/blocked error codes, opt-out rate, cost per country, one-time-code volume spikes. Alert at **half** of each danger threshold.

## 14. Troubleshooting

| Problem | Likely cause | Fix |
| --- | --- | --- |
| "Filtered" or not-registered errors | Registration missing or message differs from sample | Finish registration; match registered wording; remove shorteners |
| Cost spike | Code-pumping fraud | Country allow-list, rate limits, CAPTCHA, fraud guard |
| Low delivery rate suddenly | Carrier-side filtering or volume spike | Check against registered samples; slow down; contact provider |

## 15. Go-live checklist

- [ ] Brand, campaigns, and numbers registered and approved
- [ ] Redirector live with WAF, rate limits, 2+ instances, verified webhook signatures
- [ ] Consent wording and proof saved; STOP/HELP tested end to end
- [ ] Own short-link domain in use; no public shorteners
- [ ] One-time-code fraud limits on (country allow-list, rate limits, CAPTCHA)
- [ ] Separate system vs. marketing numbers/campaigns
- [ ] Backup provider configured and tested
- [ ] Rebuild runbooks written for: redirector loss, stolen key, provider outage, filtered campaign

---

# ## Social engineering molding: Part 3 : for phone 

> Part of a 3-guide set (Email / SMS / Phone). Shared foundation pieces are repeated here in short form so this guide stands alone.

## The one idea behind everything

Carriers and phones ask three questions. Fail one and your calls show "Spam Likely" or get blocked.

| Question | Plain meaning | How phone answers it |
| --- | --- | --- |
| **1. Who are you?** | Proof you're really you | Caller verification (STIR/SHAKEN) |
| **2. Do you behave?** | People want your calls | Low hang-ups, sensible call volume |
| **3. Do you look honest?** | Nothing looks like a scam | Real caller name, no spoofing |

## Words you'll see

| Term | Simple meaning |
| --- | --- |
| STIR/SHAKEN | Caller-ID verification so your number shows as genuine |
| SIP trunk | A cloud phone line |
| CNAM | The caller name shown on the recipient's screen |
| IVR | Interactive voice response — "press 1 for support" menus |
| Toll fraud | Attackers using your account to call premium or foreign numbers |
| IaC | Infrastructure as code: call flows and config set up from files, not clicks |

---

## 1. Decide first

- **Split system vs. marketing calls.** Separate numbers for support/alerts vs. sales outreach — trouble on one doesn't stop the other.
- **Estimate your busiest hour.** It decides trunk capacity, number count, and agent/queue sizing.
- **Name one owner** for carrier registrations, number reputation, and on-call.
- **Get legal review**: TCPA and similar consent/recording/Do-Not-Call rules apply in most places.

## 2. Cloud setup, so you can change fast

1. Separate **staging and production** — staging uses sandbox numbers/test calls only.
2. **Everything as code**: call flows, trunk config, alerts — reviewed in Git.
3. **Secrets manager** for provider API keys.
4. **A queue** for outbound dial jobs, so pacing and retries are controlled centrally, not fired straight from the app.
5. Your app calls *your own* "place call" / call-flow service, not the provider directly — switching carriers is then a one-place change.
6. **A second carrier/trunk**, pre-configured, so one carrier's problem doesn't stop calling.
7. **Spending alerts and caps.**

## 3. The shared "do-not-contact" brain

- One consent record per person for marketing/auto-dialled calls.
- One opt-out list, checked before every outbound call.
- An event log of call attempts, outcomes, and "don't call me again" requests.
- Every webhook (call events from the provider) signature-verified.

---

## 4. Redirector (Front Door) — set this up before the call-flow service itself

Put a redirector/reverse proxy in front of every public endpoint related to calling, *before* building the actual IVR/call-flow logic behind it.

Put a redirector in front of:

- Your **call-control API** (where your app triggers outbound calls or configures flows)
- **Webhook receivers** for call events (ringing, answered, ended, failed)
- Any **web-based IVR builder or recording-playback endpoint** you expose
- **Click-to-call** web widgets, if you have them

Checklist:

- [ ] HTTPS only, auto-renewing certs
- [ ] Rate limits and request-size limits on every endpoint
- [ ] A WAF for bots and common attacks
- [ ] Webhook routes: allow only the provider's published IPs where available, **and still verify every signature**
- [ ] Internal call-flow services never exposed directly to the internet
- [ ] Two or more redirector instances with health checks
- [ ] Load balancer + WAF, or Nginx/Envoy/Caddy, defined in IaC

**Never:** route calls through anonymizing relays, or rotate through many caller-ID numbers to dodge spam-likely labels — carriers and labelling databases treat number-hopping itself as a strong spam signal.

**Fast-rebuild notes for the redirector layer** (legitimate disaster recovery, not label-evasion):

- Keep the redirector config as its own IaC module, separate from call-flow logic, so the front door can be redeployed independently if compromised or overloaded.
- Version your WAF/rate-limit rules so a fresh redirector inherits the same protections immediately.
- Keep an idle secondary redirector ready to promote via DNS/load-balancer switch, so call-event webhooks aren't silently dropped during a rebuild.

---

## 5. What you need

| Need | Example |
| --- | --- |
| Outbound calls | Sales, reminders, support callbacks |
| Inbound + menu (IVR) | "Press 1 for support" |
| Staff phone system | Desk phones, apps, extensions, transfer |
| Contact center | Queues, agents, recording, reports |

## 6. Pick provider and build style

| Option | When to use |
| --- | --- |
| **Cloud phone platform** (Twilio, Telnyx, Vonage, Plivo) with APIs | Custom call flows, fastest to change |
| **Cloud contact-center service** (Amazon Connect or similar) | Agents, queues, reports without building |
| **Own phone software** (Asterisk, FreeSWITCH) on cloud servers | Only if you need deep control; you own quality, security, uptime |

Connect lines by **SIP trunk**. Use **two carriers**, so one carrier's problem doesn't stop calling.

## 7. Numbers and caller trust — what actually stops "Spam Likely"

1. **Verify your business with the carrier/provider** so calls are signed as genuine (STIR/SHAKEN — "A-level" attestation means they vouch for you and your numbers).
2. **Use only numbers you own or are authorized to use.** Never fake or "neighbour-spoof" caller ID.
3. Set your **caller name (CNAM)** and, where offered, **branded calling** (name/logo on screen).
4. **Register your numbers with call-labelling companies** (via your provider, or Free Caller Registry, Hiya, TNS) and re-check regularly.
5. **Check number reputation** before use and weekly after; replace any number that gets labelled.
6. **Don't rotate numbers constantly** — use a small, steady set of numbers so they can build trust.
7. Make sure a number **works when called back**, with a clear greeting naming your business.
8. Move existing numbers by **porting** (1–4 weeks).

**Fast-rebuild note:** keep a documented, reusable process for verifying a *new, legitimate* number with STIR/SHAKEN and registering it with labelling companies, so onboarding additional genuine capacity is fast. This should never be used as a pool of fresh numbers to swap in after one gets labelled for cause — a labelled number means something about your calling pattern needs to change, not that you need a new number.

## 8. Calling rules

- **Marketing calls and robo-dialling need written consent** in many places (e.g. TCPA in the US). Check consent before each call.
- Clean calling lists against national and your own Do-Not-Call registries.
- Call only during allowed hours (roughly 8am–9pm recipient local time; some states stricter).
- **Recording laws differ** — some places need everyone's consent. Announce recording at the start; store recordings securely with a retention limit.
- Auto-dialers: keep **abandoned calls under 3%** and play a clear message when one happens.
- Taking card payments by phone: use the provider's secure payment pause so card numbers never reach recordings (PCI).
- **Emergency calling must work** for staff phones, with a registered address (e.g. E911 in the US) — test it.
- Always honour "do not call me again" instantly.

## 9. Build

- Number → provider → your call-flow service (menu, queues, transfer) running in your cloud.
- Provider reports call events by webhook (ringing, answered, ended, failed); verify signatures; store them.
- **Failover:** if your app is down, the number forwards to a backup number/voicemail; keep a second carrier ready.
- **Pace outbound calls** — avoid bursts of very short calls, which look like robo-dialling.
- Voicemail transcripts/recordings go to encrypted storage with access control.

## 10. Quality and monitoring

- Pick provider regions close to your callers.
- Watch: answer rate, call completion rate, call setup delay, dropped calls, echo/jitter/packet loss, spam-label status, cost spikes, and **unusual destinations** (toll fraud — attackers calling premium or foreign numbers on your account).
- Block high-risk countries you never legitimately call; set per-account spending caps.

## 11. Fast, repeatable rebuilds (legitimate resilience, not label evasion)

- **One IaC module per concern** (redirector, trunk config, call-flow logic, recording storage) so any one piece can be redeployed independently.
- **A second, pre-configured carrier/trunk** ready to take over traffic if the primary has an outage.
- **A documented new-number onboarding runbook** (verification, CNAM, labelling-company registration) for genuinely new, legitimate capacity — not a rotation pool.
- **Scheduled credential-rotation drills**, so swapping a compromised API key or trunk credential is routine.
- If a number gets labelled or calls start failing: the fix is reducing abandoned-call rate, tightening consent/targeting, and working the labelling-company dispute process — not cycling to a new number to restart the trust clock.

## 12. Troubleshooting

| Problem | Likely cause | Fix |
| --- | --- | --- |
| Calls show "Spam Likely" | Low trust, no verification | Verify business, register with labelling companies, use steady numbers, lower volume |
| Calls not connecting | Carrier issue or number problem | Switch to backup carrier; check logs and number status |
| Sudden cost spike to unfamiliar destinations | Toll fraud | Block high-risk countries, review account access, rotate compromised credentials |

## 13. Go-live checklist

- [ ] Business verified for caller ID; CNAM set
- [ ] Redirector live with WAF, rate limits, 2+ instances, verified webhook signatures
- [ ] Numbers registered with labelling companies; reputation checked
- [ ] Consent, calling-hour, and recording rules built into the call flow
- [ ] Emergency calling tested; failover carrier tested
- [ ] Backup trunk/carrier configured and tested
- [ ] Rebuild runbooks written for: redirector loss, stolen credential, carrier outage, fraud spike
