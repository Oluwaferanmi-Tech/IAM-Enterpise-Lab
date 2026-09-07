# Enterprise IAM Lab

A self-built enterprise Identity and Access Management environment — Active Directory, PKI, RBAC, SSO, MFA, Identity Governance, and Microsoft Entra ID — architected, deployed, and documented from scratch, including every real infrastructure problem hit along the way.

## Overview

This repo documents a working IAM environment spanning both on-premises and cloud identity: a Windows Server domain controller, a domain-joined client, and a Kali Linux box, all provisioned on Azure, running Active Directory alongside a self-hosted Keycloak SSO stack and Microsoft Entra ID.

The goal wasn't to follow a checklist — it was to build something real enough to break, debug, and actually understand. Every module below is documented with what was built and, just as importantly, what went wrong getting there.

## Architecture

Three VMs, provisioned on Azure, sharing a single virtual network:

| VM | Role |
|---|---|
| Windows Server 2025 | Domain Controller — Active Directory Domain Services, DNS, Group Policy, AD CS, NPS |
| Windows 11 | Domain-joined client machine |
| Kali Linux | Keycloak (Identity Provider), Nextcloud and Grafana (Service Providers), security testing |

Plus a Microsoft Entra ID tenant for cloud identity, users, groups, and Azure RBAC.

*(Architecture diagram coming soon — see `/diagrams`)*

## Why Azure instead of a local hypervisor?

The original plan was to run this entirely locally using a hypervisor. That ran into real, well-documented limitations — architecture mismatches and emulation constraints that made a stable local Windows Server environment impractical. Rather than keep fighting it, I pivoted to Azure.

That turned out to be a feature, not a bug: it added hands-on Azure platform experience (resource groups, virtual networking, VM provisioning, RBAC) directly relevant to SC-300, on top of the IAM concepts the lab was originally built for.

Full writeup: [`docs/00-lab-setup/README.md`](docs/00-lab-setup/README.md)

## Roadmap

| # | Topic | Status |
|---|-------|--------|
| 00 | [Lab Setup](docs/00-lab-setup/README.md) | ✅ Complete |
| 01 | [Active Directory — Core Identity Store](docs/01-active-directory-core/README.md) | ✅ Complete |
| 02 | [Identity Lifecycle Management](docs/02-identity-lifecycle-management/README.md) | ✅ Complete |
| 03 | [Active Directory Hygiene](docs/03-active-directory-hygiene/README.md) | ✅ Complete |
| 04 | [Authentication Deep Dive](docs/04-authentication-deep-dive/README.md) | ✅ Complete |
| 05 | [Service Accounts & Privileged Identities](docs/05-service-accounts-privileged-identities/README.md) | ✅ Complete |
| 06 | [Role Based Access Control (RBAC)](docs/06-rbac/README.md) | ✅ Complete |
| 07 | [PKI & Certificate Authority](docs/07-pki-certificate-authority/README.md) | ✅ Complete |
| 08 | [Single Sign-On (SSO)](docs/08-single-sign-on/README.md) | ✅ Complete |
| 09 | [Multi-Factor Authentication (MFA)](docs/09-multi-factor-authentication/README.md) | ✅ Complete |
| 10 | [Identity Governance & Administration (IGA)](docs/10-identity-governance-administration/README.md) | ✅ Complete |
| 11 | [Microsoft Entra ID — Fundamentals, Users & Groups](docs/11-entra-id-fundamentals/README.md) | ✅ Complete |
| 12 | [Microsoft Entra ID — RBAC & Applications](docs/12-entra-id-rbac-applications/README.md) | ✅ Complete |

**Status: Lab build complete.** All 13 parts documented, on-prem and cloud. May continue adding to this over time as new topics or tools come up.

## Skills & Topics Covered

**Identity Fundamentals**
- Active Directory Domain Services — installation, forest/domain promotion, OU design
- Group Policy Objects and security baselines
- Full identity lifecycle: onboarding, offboarding, disable-before-delete retention
- PowerShell automation of bulk AD operations via CSV-driven scripts

**Security & Governance**
- Privileged account auditing, ACL hygiene, and AdminSDHolder
- Honeypot/decoy account detection design
- Role Based Access Control: group-based permissions, AD delegation, tiered administration (Tier 0/1/2)
- Access validation testing (positive and negative), not just configuration
- Identity Governance & Administration: access request/approval workflows, compliance and audit-trail reporting, IGA tooling landscape (SailPoint, Microsoft Entra ID Governance, One Identity Manager)

**Authentication & Access**
- Kerberos ticket-based authentication and Fine-Grained Password Policies (PSOs)
- RADIUS authentication via NPS, including full client-to-server troubleshooting
- PKI: internal Certificate Authority setup, certificate issuance and revocation, CRLs
- Group Managed Service Accounts (gMSA) for credential rotation without human exposure
- Single Sign-On via Keycloak, OAuth 2.0 / OIDC / SAML 2.0, multi-application integration
- TOTP-based MFA with centralized enforcement across connected applications

**Cloud Identity**
- Microsoft Entra ID: users, groups, dynamic membership rules, B2B guest access
- Azure RBAC: scope hierarchy (Management Group → Subscription → Resource Group → Resource) and permission inheritance
- Privilege-sensitive role awareness (User Access Administrator vs. resource-access roles like Reader)
- Cloud identity automation via Microsoft Graph / Cloud Shell scripting

**Cloud Infrastructure & Troubleshooting**
- Azure VM provisioning, virtual networking, and resource group management
- Diagnosing Azure-specific platform issues (VM Agent connectivity, DHCP-delivered management routes, NSG rules, Accelerated Networking) distinct from standard on-prem troubleshooting
- Azure Serial Console and Cloud Shell for out-of-band access and diagnostics
- Docker and Docker Compose deployment and credential/config troubleshooting
- Diagnosing silent configuration failures (typo'd config keys, escape-character bugs, realm/scope mismatches) rather than ones with obvious error messages

## Tech Stack

- **Cloud:** Microsoft Azure (Virtual Machines, Virtual Networks, Resource Groups), Microsoft Entra ID
- **Server OS:** Windows Server 2025 (AD DS, DNS, Group Policy, AD CS, NPS)
- **Client OS:** Windows 11
- **Identity Provider:** Keycloak (Docker), Microsoft Entra ID
- **Service Providers:** Nextcloud, Grafana
- **Security tooling:** Kali Linux
- **Target certification:** SC-300 (Microsoft Identity and Access Administrator)

## Repo Structure

```
iam-enterprise-lab/
├── README.md
├── docs/
│   ├── 00-lab-setup/
│   ├── 01-active-directory-core/
│   ├── 02-identity-lifecycle-management/
│   ├── 03-active-directory-hygiene/
│   ├── 04-authentication-deep-dive/
│   ├── 05-service-accounts-privileged-identities/
│   ├── 06-rbac/
│   ├── 07-pki-certificate-authority/
│   ├── 08-single-sign-on/
│   ├── 09-multi-factor-authentication/
│   ├── 10-identity-governance-administration/
│   ├── 11-entra-id-fundamentals/
│   └── 12-entra-id-rbac-applications/
├── diagrams/
├── scripts/
└── screenshots/
```

Each module folder contains its own `README.md` (what was built, the steps, and issues hit and how they were resolved) and an `images/` subfolder with supporting screenshots.

## Connect

Following along on [LinkedIn](https://www.linkedin.com/in/oluwaferanmi-bamikole-44a222309/).
