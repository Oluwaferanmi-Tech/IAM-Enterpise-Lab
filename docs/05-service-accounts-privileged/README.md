# Service Accounts & Privileged Identities

## What I did

Next was service accounts — the identities that run automated processes and applications rather than people — and specifically the shift from regular service accounts to Group Managed Service Accounts (gMSA). The core takeaway: gMSAs are meaningfully more secure than regular service accounts, not just a "best practice" checkbox.

## Steps

### Why regular service accounts are a real security liability

Regular service accounts are just standard AD accounts with a password someone sets manually — which in practice usually means the password gets set once, never rotated (because rotating it risks breaking whatever service depends on it), and known to at least one human who configured it. That combination is exactly what makes service accounts one of the most common real-world lateral movement targets: static credentials, no monitoring incentive to rotate them, and often over-permissioned because nobody wants to be the one who scoped it too tight and broke production.

### Creating and configuring a gMSA

A Group Managed Service Account solves this structurally rather than procedurally. Its password is generated and rotated automatically by Active Directory on a schedule (roughly every 30 days by default) — no human ever sets it, sees it, or has to remember to rotate it. Only computer accounts explicitly authorized to use the gMSA can actually retrieve its credential from AD, so even if an attacker compromises an unrelated machine, they can't just read a stored password and reuse it elsewhere.

### Replacing a regular service account with a gMSA

Took an existing service running under a regular service account and migrated it to a gMSA instead, going through the actual mechanics: authorizing the specific computer account to use the gMSA, updating the service to run under the new identity, and confirming it still functioned correctly afterward. Seeing the same service work identically under the gMSA — with none of the manual password management overhead — makes the security upgrade concrete rather than theoretical.

---

## Skills demonstrated

- Understanding the security risk profile of regular (manually-managed) service accounts
- Creating and configuring Group Managed Service Accounts (gMSA)
- Authorizing specific computer accounts to retrieve a gMSA's credential
- Migrating a live service from a regular service account to a gMSA without breaking functionality
- Reasoning about credential rotation and least-privilege as structural properties of an account type, not just a manual policy to remember
