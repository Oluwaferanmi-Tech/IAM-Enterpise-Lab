# Enterprise IAM Lab

A hands-on Identity and Access Management lab built while preparing for the Microsoft SC-300 (Identity and Access Administrator) exam.

## Overview

This repo documents a self-built enterprise IAM environment covering Active Directory, identity lifecycle management, authentication, RBAC, PKI, single sign-on, MFA, identity governance, and Microsoft Entra ID. It's built alongside a structured IAM course, with each module in this repo corresponding to a module in the course curriculum.

The goal isn't just to follow along — it's to have a working, documented environment that demonstrates practical IAM skills beyond what a certification alone shows.

## Architecture

Three VMs, provisioned on Azure, sharing a single virtual network:

| VM | Role |
|---|---|
| Windows Server 2025 | Domain Controller — Active Directory Domain Services, DNS, Group Policy |
| Windows 11 | Domain-joined client machine |
| Kali Linux | Security testing and identity attack-surface exploration |

*(Architecture diagram coming soon — see `/diagrams`)*

## Why Azure instead of a local hypervisor?

The original plan was to run this entirely locally using a hypervisor. That ran into real, well-documented limitations — architecture mismatches and emulation constraints that made a stable local Windows Server environment impractical. Rather than keep fighting it, I pivoted to Azure.

That turned out to be a feature, not a bug: it added hands-on Azure platform experience (resource groups, virtual networking, VM provisioning) directly relevant to SC-300, on top of the IAM concepts the lab was originally built for.

Full writeup: [`docs/00-lab-setup/local-attempt-and-pivot.md`](docs/00-lab-setup/local-attempt-and-pivot.md)

## Progress

- [x] **Module 1** — Introduction & Complete Lab Setup
- [x] **Module 2** — Active Directory: Core Identity Store
- [x] **Module 3** — Identity Lifecycle Management
- [x] **Module 4** — Active Directory Hygiene
- [x] **Module 5** — Authentication Deep Dive
- [x] **Module 6** — Service Accounts & Privileged Identities
- [ ] **Module 7** — Role Based Access Control (RBAC)
- [ ] **Module 8** — PKI & Certificate Authority
- [ ] **Module 9** — Single Sign-On (SSO)
- [ ] **Module 10** — Multi-Factor Authentication (MFA)
- [ ] **Module 11** — Identity Governance & Administration (IGA)
- [ ] **Module 12** — Microsoft Entra ID: Fundamentals, Users & Groups
- [ ] **Module 13** — Microsoft Entra ID: RBAC & Applications
- [ ] *(additional features)*

## Skills & Topics Covered

**Identity Fundamentals**
- Active Directory Domain Services (AD DS) installation, promotion, and domain design
- DNS configuration in a domain environment
- Organizational Unit (OU) design and identity lifecycle management
- Group Policy Objects (GPOs) and security baselines
- Authentication protocols and Windows domain authentication

**Access & Security**
- Role Based Access Control (RBAC) design
- Service accounts and privileged identity management
- PKI and Certificate Authority setup
- Single Sign-On (SSO) configuration
- Multi-Factor Authentication (MFA)
- Identity Governance & Administration (IGA)

**Cloud Identity**
- Microsoft Entra ID (Azure AD) — users, groups, RBAC, and enterprise applications

**Cloud Infrastructure & Troubleshooting**
- Azure VM provisioning, virtual networking, and resource group management
- Diagnosing Azure-specific platform issues (VM Agent connectivity, NSG rules, effective routes, Accelerated Networking) distinct from standard on-prem/local-hypervisor troubleshooting
- Azure Serial Console and Cloud Shell for out-of-band VM access and diagnostics
- Documenting infrastructure decisions and incident writeups as part of the engineering process

## Tech Stack

- **Cloud:** Microsoft Azure (Virtual Machines, Virtual Networks, Resource Groups)
- **Server OS:** Windows Server 2025 (Active Directory Domain Services, DNS, Group Policy)
- **Client OS:** Windows 11
- **Identity platform:** Microsoft Entra ID (Azure AD)
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
│   ├── 05-service-accounts-privileged/
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

Each `docs/` module folder contains its own `README.md` (what I did, steps, issues hit and how they were resolved) and an `images/` subfolder with supporting screenshots.

## Connect

Following along on [LinkedIn](https://www.linkedin.com/in/oluwaferanmi-bamikole-44a222309/) as this lab progresses.
