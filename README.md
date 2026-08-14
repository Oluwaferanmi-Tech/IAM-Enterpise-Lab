# Enterprise IAM Lab

A hands-on Identity and Access Management lab built while preparing for the Microsoft SC-300 (Identity and Access Administrator) exam.

## Overview

This repo documents a self-built enterprise IAM environment covering Active Directory, identity lifecycle management, authentication, RBAC, PKI, and single sign-on. It's built alongside a structured IAM course, with each module in this repo corresponding to a module in the course curriculum.

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
- [ ] **Module 2** — Active Directory: Core Identity Store
- [ ] **Module 3** — Identity Lifecycle Management
- [ ] **Module 4** — Active Directory Hygiene
- [ ] **Module 5** — Authentication Deep Dive
- [ ] **Module 6** — Service Accounts & Privileged Identities
- [ ] **Module 7** — Role Based Access Control (RBAC)
- [ ] **Module 8** — PKI & Certificate Authority
- [ ] **Module 9** — Single Sign-On (SSO)
- [ ] *(additional modules added as the course progresses)*

## Tech Stack

- **Cloud:** Microsoft Azure (Virtual Machines, Virtual Networks, Resource Groups)
- **Server OS:** Windows Server 2025 (Active Directory Domain Services, DNS, Group Policy)
- **Client OS:** Windows 11
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
│   └── 08-single-sign-on/
├── diagrams/
├── scripts/
└── screenshots/
```

## Connect

Following along on [LinkedIn](www.linkedin.com/in/oluwaferanmi-bamikole-44a222309

) as this lab progresses.
