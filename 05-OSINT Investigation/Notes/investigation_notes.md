# OSINT Investigation — OWASP Foundation

## Target
OWASP Foundation

## Target Domain
owasp.org

## Investigation Type
Passive OSINT

## Scope
Publicly available information only.

## Objective
To identify and document publicly available information about the OWASP Foundation using:
- Search-engine intelligence
- Username investigation
- Email/domain intelligence
- DNS and IP intelligence
- Metadata analysis

## Ethical Boundary
No unauthorized access, exploitation, credential testing,
private-data collection, or intrusive scanning will be performed.

## 1. Search Engine Intelligence

### Search 1
**Operator used:** `site:owasp.org`

**Finding:**  
Two publicly available OWASP pages were identified.

**Source:**  
OWASP official website

**Interpretation:**  
The `site:` operator successfully restricted the search results to the OWASP domain.

**Confidence:** High

---

### Search 2
**Operator used:** `site:owasp.org "OWASP Foundation"`

**Finding:**  
Public pages related to the OWASP Foundation were identified.

**Interpretation:**  
Combining `site:` with an exact phrase helped narrow the results to relevant OWASP Foundation content.

**Confidence:** High

---

### Search 3
**Operator used:** `site:owasp.org filetype:pdf`

**Finding:**  
A publicly available PDF hosted on the OWASP domain was identified.

**Interpretation:**  
The `filetype:pdf` operator can be used to locate publicly indexed PDF documents belonging to a target domain.

**Confidence:** High

---

### OSINT Principle Demonstrated

Search-engine operators can narrow publicly indexed information without directly probing the target infrastructure. This is passive OSINT.

## 2. Username / Organization Investigation

### Target
OWASP

### Platform
GitHub

### Finding
An official public GitHub organization exists under the username/organization identifier `OWASP`.

### Verification clue
The GitHub organization identifies itself as the OWASP Foundation and has verified control of the `owasp.org` and `owasp.com` domains.

### Interpretation
The GitHub account has strong evidence connecting it to the OWASP Foundation. This is stronger than relying on a username match alone.

### OSINT Principle Demonstrated
A username or identifier should be treated as a lead until independent public evidence supports the connection. Organizational verification provides stronger evidence than a simple matching username.

## 3. Email Investigation

### Target Domain
owasp.org

### Public Email Identified
support@owasp.org

### Security Contact
security@owasp.org

### Findings

1. `support@owasp.org` is publicly listed by the OWASP Foundation as a contact for Foundation support.
2. `security@owasp.org` is publicly listed for questions concerning OWASP's security-reporting process.
3. Both addresses use the `owasp.org` domain.
4. The addresses are role-based organizational contacts and should not be interpreted as evidence identifying a specific individual.

### Interpretation

The publicly documented email addresses confirm that OWASP uses its own `owasp.org` domain for organizational communication and security-related contact.

### Source
OWASP Foundation official Contact Us and Security pages.

### Confidence
High

### DNS / A Record

**Command used:**
`nslookup owasp.org`

**Result:**
- 216.150.1.1
- 216.150.16.1

**DNS resolver used:**
192.168.1.1

**Finding:**
The `owasp.org` domain currently resolves to two IPv4 addresses.

**Interpretation:**
The DNS lookup provides the IPv4 addresses associated with the domain at the time of investigation. The response was non-authoritative, meaning it was provided by the configured DNS resolver rather than directly by an authoritative nameserver.

**Confidence:**
High

### DNS / MX Record

**Command used:**
`nslookup -type=MX owasp.org`

**Finding:**
The `owasp.org` domain has five publicly listed MX records, all pointing to Google's `aspmx.l.google.com` mail infrastructure.

**MX priorities observed:**
- Priority 1 → `aspmx.l.google.com`
- Priority 5 → `alt1.aspmx.l.google.com`
- Priority 5 → `alt2.aspmx.l.google.com`
- Priority 10 → `alt3.aspmx.l.google.com`
- Priority 10 → `alt4.aspmx.l.google.com`

**Interpretation:**
The MX records indicate that Google mail infrastructure is publicly configured to handle email for the `owasp.org` domain. Lower preference values indicate higher MX priority.

**Confidence:**
High

### DNS / NS Record

**Command used:**
`nslookup -type=NS owasp.org`

**Finding:**
Two publicly listed nameservers were identified:

- `west.ns.cloudflare.com`
- `fay.ns.cloudflare.com`

**Interpretation:**
The nameservers indicate that Cloudflare infrastructure is being used for authoritative DNS for `owasp.org`.

**Confidence:**
High

### DNS / TXT Record

**Command used:**
`nslookup -type=TXT owasp.org`

**Key finding:**
An SPF TXT record was identified:

`v=spf1 include:_spf.google.com include:servers.mcsv.net include:amazonses.com -all`

**Interpretation:**
The SPF policy authorizes email-sending infrastructure associated with Google, Mailchimp, and Amazon SES for the `owasp.org` domain. The `-all` mechanism indicates that hosts not matching the SPF policy are intended to fail SPF evaluation.

Additional TXT records were observed for domain/service verification, including Google, Microsoft, and Atlassian verification records.

**Confidence:**
High

### Reverse DNS Check

**Command used:**
`nslookup 216.150.1.1`

**Result:**
No reverse-DNS/PTR hostname was returned.

**Interpretation:**
The configured DNS resolver did not return a PTR record for the IP address. This does not indicate that the IP is invalid or malicious; it only means a reverse-DNS name was not available through this lookup.

**Limitation:**
Reverse DNS information is not guaranteed to exist for every IP address.

**Confidence:**
High

## IP Ownership / Reputation

### IP: 216.150.1.1

**Finding:**  
The IP address is associated with Vercel, Inc. and belongs to the `216.150.1.0/24` network. Public IP intelligence identifies AS16509 as the originating ASN, operated by Amazon.com, Inc. The IP is used as CDN/hosting infrastructure.

**Source:**  
Public IP intelligence / WHOIS and ASN information.

**Date checked:**  
2026-10-04

**Evidence:**  
- IP: `216.150.1.1`
- Network: `216.150.1.0/24`
- Organization: Vercel, Inc.
- ASN: AS16509
- ASN operator: Amazon.com, Inc.
- Reverse DNS lookup: no hostname returned

**Interpretation:**  
The IP appears to be part of Vercel's cloud/CDN infrastructure. Because CDN and hosting IPs can be shared by many domains, IP reputation alone cannot be used to attribute malicious activity to OWASP.

**Confidence:**  
High for network ownership; low for attributing any specific activity to OWASP.

## IP Ownership / Reputation

### IP: 216.150.16.1

**Finding:**  
The IP address is associated with AS16509, which is registered to Amazon.com, Inc. (AMAZON-02).

**Source:**  
Public IP/ASN intelligence sources.

**Date checked:**  
2026-10-04

**Evidence:**  
- IP: `216.150.16.1`
- ASN: `AS16509`
- Network operator: Amazon.com, Inc.
- Reverse DNS lookup: no hostname returned

**Interpretation:**  
The IP is part of Amazon's network infrastructure. The ASN information identifies the network operator but does not, by itself, prove that Amazon owns or operates OWASP's website content.

**Confidence:**  
High for ASN/network identification.