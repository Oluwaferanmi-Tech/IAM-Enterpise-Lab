# Role-Based Access Control (RBAC)

## What I did

Next was implementing RBAC properly in Active Directory: building out the group structure to support it, delegating administrative control instead of handing out broad admin rights, and separating administration into tiers so a lower-value compromise can't cascade into a higher-value one. Just as important as building it was not assuming it worked — every permission got tested from both directions before I called it done.

## Steps

### Designing and implementing RBAC in Active Directory

Building this out meant going back to identity fundamentals: created new OUs to represent the organizational structure, created new user accounts to actually test with, and created security groups representing each role rather than assigning permissions to individual people directly. Permissions get granted to the groups, not to specific users — so moving someone into a different role is just a group membership change, not a rebuild of their entire permission set.

### AD Delegation

Delegated administrative control over specific OUs to the groups responsible for them, instead of making anyone a full Domain Admin just so they could manage their own department's accounts. This is least privilege in practice: the IT group can manage the IT OU without ever touching Finance, Sales, or HR, and without holding domain-wide rights they don't need.

### Tiered Administration Model (Tier 0 / 1 / 2)

Learned the tiered admin model Microsoft recommends for exactly this reason: Tier 0 covers domain controllers and the highest-value assets, Tier 1 covers servers, Tier 2 covers workstations. The core rule is that credentials used at a lower tier should never be usable to reach a higher one — an admin account for managing workstations shouldn't also be able to touch a domain controller, so that a compromised workstation can't become a compromised domain.

### Validating that permissions actually worked

Configuring access and confirming it actually behaves as intended are two different things, and it's easy to stop at the first one. After setting up the groups, delegation, and tiering, I logged in as different test accounts to check both directions: confirming that accounts that *should* have access actually got it, and — just as important — confirming that accounts that shouldn't have access were genuinely denied, not just assumed to be blocked. This is where a misconfigured inherited permission or an overly broad ACL would actually surface, rather than staying hidden until someone abuses it.

---

## Skills demonstrated

- RBAC design and implementation in Active Directory using groups, OUs, and role-based permission assignment
- AD delegation of administrative control for least-privilege departmental management
- Tiered administration model (Tier 0/1/2) to contain lateral privilege escalation
- Positive and negative access validation testing, rather than assuming configuration equals correct behavior
- Organizational structure design (OUs, groups, and test users) built to support role-based access at scale
