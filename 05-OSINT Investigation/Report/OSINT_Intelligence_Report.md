# OSINT Investigation & Intelligence Report

## 1. Executive Summary

This project demonstrates a passive Open-Source Intelligence (OSINT) investigation of the OWASP Foundation using publicly available information.

The investigation focused on search-engine intelligence, public organizational accounts, email/domain information, DNS records, and IP/ASN intelligence.

No unauthorized access, exploitation, credential testing, intrusive scanning, or private-data collection was performed.

The investigation identified publicly observable DNS infrastructure, email infrastructure, domain-verification records, and network information associated with the target domain.

---

## 2. Target

**Organization:** OWASP Foundation  
**Domain:** `owasp.org`

**Investigation Type:** Passive OSINT

**Scope:** Publicly available information only.

---

## 3. Objective

The objectives of this investigation were to:

- Practice passive OSINT techniques.
- Identify publicly available information about an organization.
- Analyze domain and DNS infrastructure.
- Examine publicly visible organizational accounts.
- Understand email infrastructure.
- Identify IP addresses and their associated network information.
- Practice documenting findings with appropriate confidence levels.
- Maintain ethical boundaries during intelligence gathering.

---

## 4. Methodology

The following techniques were used:

- Search-engine operators
- Username/account investigation
- Public email-address analysis
- DNS enumeration using `nslookup`
- MX, NS and TXT record analysis
- IP and ASN investigation
- Reverse DNS checking
- Public IP intelligence

The investigation was intentionally limited to passive information gathering.

---

# 5. Search-Engine Intelligence

## Finding 1 — Domain Search

### Query

`site:owasp.org`

### Observation

The search returned publicly accessible pages hosted on the OWASP domain.

### Interpretation

The `site:` operator can be used to identify publicly indexed content belonging to a specific domain.

### Confidence

High.

---

## Finding 2 — Exact Phrase Search

### Query

`site:owasp.org "OWASP Foundation"`

### Observation

The search returned publicly indexed pages containing the exact phrase "OWASP Foundation".

### Interpretation

Exact-phrase searches can help locate references to a specific organization or topic within a domain.

### Confidence

High.

---

## Finding 3 — PDF Search

### Query

`site:owasp.org filetype:pdf`

### Observation

The search returned publicly indexed PDF documents hosted on the OWASP domain.

### Interpretation

The `filetype:` operator can be used to identify specific document types that are publicly indexed.

### Confidence

High.

---

# 6. Username Investigation

## Finding 4 — Public Organizational Account

A search for the OWASP organization on GitHub identified the public GitHub organization associated with OWASP.

### Observation

The account represents an organization rather than a private individual.

### Interpretation

This demonstrates that username/account investigation can be applied to organizational identities as well as individual usernames.

The presence of an account on another platform should not automatically be interpreted as proof of an individual's identity.

### Confidence

High.

---

# 7. Email Investigation

## Finding 5 — Public Role-Based Email Addresses

Publicly documented OWASP contact information includes role-based addresses such as:

- `support@owasp.org`
- `security@owasp.org`

### Observation

Both addresses use the `owasp.org` domain and represent organizational functions.

### Interpretation

Role-based addresses provide information about an organization's public contact and security processes without revealing the identity of a specific individual.

### Security Note

Public email addresses should not be treated as evidence that a particular person owns or operates the mailbox.

### Confidence

High.

---

# 8. DNS & Domain Intelligence

## Finding 6 — A Records

### Command

`nslookup owasp.org`

### Result

The DNS lookup returned:

- `216.150.1.1`
- `216.150.16.1`

The response was identified as a non-authoritative answer.

### Interpretation

The domain currently resolves to multiple IPv4 addresses through the configured DNS resolver.

The local resolver used during the investigation was:

`192.168.1.1`

This address represents the local DNS resolver/router rather than OWASP's authoritative DNS server.

### Confidence

High.

---

# 9. Mail Infrastructure

## Finding 7 — MX Records

### Command

`nslookup -type=MX owasp.org`

### Observed Mail Servers

- `aspmx.l.google.com` — priority 1
- `alt1.aspmx.l.google.com` — priority 5
- `alt2.aspmx.l.google.com` — priority 5
- `alt3.aspmx.l.google.com` — priority 10
- `alt4.aspmx.l.google.com` — priority 10

### Interpretation

The MX records indicate that Google mail infrastructure handles email delivery for the `owasp.org` domain.

Lower MX preference numbers represent higher delivery priority.

### Confidence

High.

---

# 10. Authoritative DNS Infrastructure

## Finding 8 — NS Records

### Command

`nslookup -type=NS owasp.org`

### Result

The domain returned:

- `west.ns.cloudflare.com`
- `fay.ns.cloudflare.com`

### Interpretation

These nameservers indicate that Cloudflare infrastructure is used for authoritative DNS for the domain.

This demonstrates that an organization can use different providers for different infrastructure components.

For example:

- DNS → Cloudflare
- Email → Google

### Confidence

High.

---

# 11. TXT & SPF Analysis

## Finding 9 — TXT Records

### Command

`nslookup -type=TXT owasp.org`

### Important SPF Record

`v=spf1 include:_spf.google.com include:servers.mcsv.net include:amazonses.com -all`

### Interpretation

The SPF record authorizes sending infrastructure associated with:

- Google
- Mailchimp infrastructure
- Amazon SES

The `-all` mechanism indicates that mail from sources not matching the SPF mechanisms is intended to fail SPF evaluation.

Other TXT records observed included domain-verification records associated with services such as Google, Atlassian, and Microsoft.

### Security Significance

TXT records can reveal information about an organization's email-security configuration and third-party services.

### Confidence

High.

---

# 12. Reverse DNS Investigation

## Finding 10 — Reverse DNS

Reverse DNS lookups were performed against both IPv4 addresses returned for `owasp.org`.

### IP 1

`216.150.1.1`

Result:

No reverse-DNS hostname was returned.

### IP 2

`216.150.16.1`

Result:

No reverse-DNS hostname was returned.

### Interpretation

The configured DNS resolver did not return PTR hostnames for either IP address.

The absence of a PTR record does not indicate that an IP address is malicious or invalid.

### Confidence

High.

---

# 13. IP & ASN Intelligence

## Finding 11 — IP Network Information

### IP

`216.150.1.1`

### Observed Network Information

- Network: `216.150.1.0/24`
- Organization: Vercel, Inc.
- ASN: AS16509
- ASN operator: Amazon.com, Inc.

### Interpretation

The IP appears to be part of cloud/CDN/hosting infrastructure.

Because hosting and CDN infrastructure can be shared, the IP association alone cannot be used to attribute malicious activity to OWASP.

### Confidence

High for network information.

Low for attributing any specific activity to OWASP.

---

## Finding 12 — Second IP

### IP

`216.150.16.1`

### Observed Network Information

- ASN: AS16509
- Network operator: Amazon.com, Inc.

### Interpretation

The IP is associated with Amazon's AS16509 network infrastructure.

ASN information identifies the network operator but does not by itself prove that Amazon directly operates OWASP's website content.

### Confidence

High for ASN/network identification.

---

# 14. Ethical Boundaries

The investigation followed a passive OSINT methodology.

The following activities were intentionally avoided:

- Unauthorized access
- Password testing
- Credential attacks
- Exploitation
- Intrusive vulnerability scanning
- Accessing private accounts
- Collecting leaked passwords
- Downloading stolen databases
- Attempting to bypass authentication
- Targeting individuals with personal-data collection

Only publicly available information was analyzed.

---

# 15. Key Lessons Learned

Through this investigation, the following OSINT concepts were practiced:

1. Search-engine operators can narrow publicly indexed information.
2. A username match does not automatically prove identity.
3. Role-based email addresses reveal organizational functions.
4. DNS records can reveal infrastructure relationships.
5. MX records identify mail-delivery infrastructure.
6. NS records identify authoritative DNS providers.
7. TXT records can reveal domain-verification and email-security information.
8. SPF records can identify authorized email-sending infrastructure.
9. IP addresses can be investigated through ASN and network information.
10. Reverse DNS does not always return a hostname.
11. Shared infrastructure makes IP attribution difficult.
12. Public information must still be interpreted carefully.
13. OSINT findings should be verified and documented with appropriate confidence levels.

---

# 16. Conclusion

The OSINT investigation successfully demonstrated how publicly available information can be collected, analyzed, verified, and documented without directly interacting with or compromising the target's systems.

The investigation revealed information about the OWASP Foundation's publicly observable search presence, organizational accounts, email infrastructure, DNS configuration, SPF policy, and network infrastructure.

The project also demonstrated an important principle of cybersecurity intelligence:

> A technical finding is not automatically evidence of malicious activity.

Infrastructure information must be interpreted in context, verified where possible, and documented with appropriate confidence.

This project therefore provides practical evidence of foundational OSINT and reconnaissance skills while maintaining responsible and ethical boundaries.