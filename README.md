# DIY Email Server

**WORK IN PROGRESS** Currently gets you most of the way there, but the user experience is not ideal

This guide will help you set up a full-featured email server on an inexpensive, low-resource VPS instance using Docker Compose.
Deploying an email server is an advanced task and this project assumes a level of familiarity with various tools and concepts.

## Goals
- Minimal footprint MTA and MDA
- Protection against spam
- Provide email authentication

## Prerequisites

- Server with a **static IP address**
- **Docker Compose** installed on the server
- Ability to edit **DNS Record**s
- Expose ports for message submission
  - 25  (MTA-MTA communication)
  - 465 (Implicit TLS submission)
  - 587 (STARTLS)
- Expose ports for message retrieval
  - 995 (encrypted POP3)
  - 993 (encrypted IMAP)
- Ports we won't expose/use
  - 110 (unencrypted POP3)
  - 143 (unencrypted IMAP)

## Components

We'll use Docker Compose to deploy the following services:

- **Mail Delivery Agent (MDA)** - Handles email storage and retrieval.
  - **Dovecot**: Provides IMAP and POP3 functionality.

- **Mail Transfer Agent (MTA)** - Handles email routing and sending.
  - **Postfix**: Manages outgoing and incoming email transfers.

## Additional Security & Configuration Settings

To ensure secure and reliable email delivery, configure the following:

- **DKIM (DomainKeys Identified Mail)**: Authenticates your domain's emails to prevent spoofing.
- **SPF (Sender Policy Framework)**: Specifies allowed IP addresses for sending email on behalf of your domain.
- **DMARC (Domain-based Message Authentication, Reporting, and Conformance)**: Enforces domain-level protection policies and generates reports.
- **rDNS (Reverse DNS)**: Validates that the server IP address maps back to your domain.

> **Note**: Most of these features require adding specific TXT records in your DNS settings, which may need to be done manually.

### DKIM

We'll use **OpenDKIM** for DKIM. The final product is a DNS record like the following:

```plaintext
<selector>._domainkey.example.com  TXT "v=DKIM1; k=rsa; p=MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAx2o55j0C6ICu56VEjwYJ08SzfxSiOhkxagGBPUwHpxzy"
```
- The selector is sent as part of the email header and identifies the DKIM key.
### SPF
Create a DNS TXT record listing the IPs authorized to send email for your domain:
```plaintext
example.com  TXT "v=spf1 ip4:<your-mail-server-ip> -all"
```

### DMARC
Create a DNS TXT record to establish your DMARC policy:
```plaintext
_dmarc.example.com  TXT "v=DMARC1; p=none; rua=mailto:dmarc-reports@example.com"
```

### rDNS
While you may not have direct control over rDNS, ensure that your VPS provider sets it to match your domain name. To verify the rDNS configuration, use:
```shell
dig -x <ip_address>
```

# Server Setup
Clone the repository
```shell
git clone https://github.com/joeknoll/email_server.git
cd email_server
```
###

## Configure Deployment

## Start Server

# DNS Configuration
## DKIM
## SPF
## DMARC
## MX Record

# Maintenance
Updating should be as simple as navigating to your ***email_server*** directory and running
```shell
git pull origin main
docker compose pull
docker compose up -d --build
```

