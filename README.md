# WRCCDC 2026 Quals - CBU Cyber Club Toolkit
## Competition: February 7, 2026 | 9:00 AM - 5:00 PM PST

---

## Quick Deploy

SSH/RDP into a box, then:
```bash
# Linux boxes:
curl -k -sO https://raw.githubusercontent.com/sperumean/Cyber_Defense/main/linux-audit.sh && chmod +x linux-audit.sh

# Alpine router (ontario .2) — uses wget, not curl:
wget --no-check-certificate -O alpine-router-audit.sh https://raw.githubusercontent.com/sperumean/Cyber_Defense/main/alpine-router-audit.sh && chmod +x alpine-router-audit.sh

# Kubernetes (mead .16):
curl -k -sO https://raw.githubusercontent.com/sperumean/Cyber_Defense/main/k8s-audit.sh && chmod +x k8s-audit.sh
```

Windows boxes - open PowerShell as admin:
```powershell
Invoke-WebRequest -Uri "https://raw.githubusercontent.com/sperumean/Cyber_Defense/main/windows-audit.ps1" -OutFile "C:\windows-audit.ps1" -SkipCertificateCheck
Set-ExecutionPolicy Bypass -Scope Process -Force
.\windows-audit.ps1
```

---

## Topology Quick Reference

```
                    Internet
                       |
                [ontario .2] Alpine Router (GATEWAY)
                       |
            ┌──────────┼──────────────────────┐
            │    192.168.220.0/24              │
            │    External: 10.100.1XX.0/24     │
            │                                  │
  .3  VPN Gateway (OUT OF SCOPE)               │
  .10 arrowhead    Debian     ICS              │
  .14 tahoe        Win2019    AD + DNS         │
  .16 mead         Linux      KubeVirt + K8S   │
  .18 superior     Linux      Web Server       │
  .20 stupidlake   Rocky      Web Server       │
  .22 victoria     Win2022    Wiki/Tickets     │
  .23 wikey        Void       Wiki             │
  .24 pychgynmygytgyn Linux   File Share       │
  .26 elsinore     Debian     Water Valves     │
  .28 baikal       Win2025    Billing/HR       │
  .240 berryessa   Debian     SOC              │
  ─────────────────────────────────────────────┘
```

**CRITICAL:** Red team LOVES the router. If they fireball your firewall, everything goes down.

---

## FIRST 15 MINUTES CHECKLIST

### Minute 0-2: Get In
- [ ] Connect VPN (NetBird, port 51820)
- [ ] Verify Proxmox console access
- [ ] Get default credentials from Discord at 9:00 AM
- [ ] Assign boxes to team members

### Minute 2-5: Snapshot EVERYTHING
```bash
# On EVERY Linux box:
./linux-audit.sh --snapshot

# On the router:
./alpine-router-audit.sh --snapshot

# On K8S box:
./k8s-audit.sh --snapshot

# On Windows boxes:
.\windows-audit.ps1 -Snapshot
```

### Minute 5-10: Change ALL Passwords
```bash
# Linux:
./linux-audit.sh --passwords

# Windows:
.\windows-audit.ps1 -Passwords

# IMMEDIATELY submit PCRs in Quotient!
```

### Minute 10-15: Audit
```bash
# Run audit on every box:
./linux-audit.sh
./alpine-router-audit.sh
./k8s-audit.sh
.\windows-audit.ps1
```

### Minute 15+: Harden & Monitor
```bash
# Harden (backs up first):
./linux-audit.sh --harden
./alpine-router-audit.sh --harden
.\windows-audit.ps1 -Harden

# Start monitoring on router (KEEP RUNNING ALL DAY):
./alpine-router-audit.sh --monitor

# Periodic diff checks:
./linux-audit.sh --diff
```

---

## ROLE ASSIGNMENTS (suggested)

| Role | Boxes | Priority |
|------|-------|----------|
| **Router Lead** | ontario (.2) | Firewall, NAT, monitoring |
| **Windows Lead** | tahoe (.14), victoria (.22), baikal (.28) | AD, DNS, passwords |
| **Linux Lead** | stupidlake (.20), wikey (.23), pychgynmygytgyn (.24) | Web, wiki, shares |
| **ICS/K8S** | arrowhead (.10), mead (.16), elsinore (.26) | ICS, K8S, valves |
| **SOC/IR** | berryessa (.240) | Logging, monitoring, incident reports |
| **Inject Lead** | N/A | Read injects, delegate, write reports |
| **Discord/Orange** | N/A | ALWAYS have someone in voice channel! |

---

## SCORING REMINDERS

- **~25 services** scored every 1-2 minutes
- **SLA violation** = 5 failed checks → 6th check = penalty
  - Before 11:00 AM: **50 points per SLA**
  - After 11:00 AM: **25 points per SLA**
- **Box reset:** 60 points (100 for router!)
- **No resets after 2:30 PM**
- Red team persistence: up to **100 pts/method/hour**
  - **50% reduction** if you file Incident Report with eradication proof!

## POINT DEDUCTIONS (Red Team)
- Root access: **-100**
- User access: **-25** (+ -100 if escalated)
- Password/hash dump: **-50**
- Sensitive files: **-25**
- Credit card numbers: **-50**
- Full PII (name + address + CC): **-200**
- Encrypted DB: **-25** (-25 more if decrypted)

---

## INJECT RULES
- File format: `inject04_teamXX.pdf` (always PDF!)
- **NO AI** unless inject says otherwise (20%+ AI = 0 points)
- Use **10.100.1XX.Y** addresses in injects (public/external)
- Cite sources if using templates
- Read inject **twice** before starting, once before submitting

---

## CRITICAL COMMANDS

### Password Change Request (PCR)
Submit through Quotient scoring engine EVERY TIME you change a password!

### If you get locked out of the router:
1. File a ticket at https://tickets.wccomps.org
2. They'll provide admin credentials via Discord
3. Router reset = -100 points (last resort)

### If Orange Team contacts you:
1. Be polite!
2. Respond quickly
3. Give them the **10.100.1XX.Y** address, NOT 192.168.220.Y
4. @ping them when fixed

### Secret code (read the packet!):
> "Swampy the Alligator"

---

## INCIDENT REPORT TEMPLATE

If red team breaches a box, document eradication for 50% penalty reduction:

```
INCIDENT REPORT
Date/Time: 
Affected System:
Type of Compromise:
Evidence Found:
  - [what you found]
Eradication Steps:
  - [account deletion]
  - [service removed]
  - [backdoor cleaned]
  - [passwords changed]
Verification:
  - [how you confirmed it was clean]
```

**Submit by 6:00 PM!**
