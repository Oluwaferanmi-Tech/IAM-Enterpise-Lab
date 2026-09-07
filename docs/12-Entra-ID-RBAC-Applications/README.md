# Microsoft Entra ID — RBAC & Applications

## What I did

Closed out the lab with Azure RBAC: deployed Azure VMs specifically to have real resources to test role assignments against, then worked through Azure's scope hierarchy by assigning the Reader role at both the individual resource level and the resource group level, and finished with User Access Administrator — the one role in Azure RBAC that's less about accessing resources and more about controlling who else can.

## Steps

### Azure Virtual Machines core concepts, revisited

Went back through VM deployment, this time through an RBAC lens rather than an infrastructure-troubleshooting one. The earlier lab setup work already covered the mechanics of standing up Azure VMs the hard way — this was mostly about having real resources to actually assign roles against.

### Deploying Azure VMs (Linux and Windows)

Deployed a Linux VM and a Windows VM as RBAC test subjects. Having two different resource types made the scope and inheritance labs concrete instead of abstract.

### Assigning the Reader role at the resource level

Assigned Reader — read-only, no ability to modify or delete — directly on a single resource. This is the narrowest scope Azure RBAC supports: access to exactly one thing and nothing else.

### Assigning the Reader role at the resource group level

Assigned the same Reader role, but one level up in Azure's scope hierarchy (Management Group → Subscription → Resource Group → Resource), on the resource group instead of an individual resource. Permissions assigned at a higher scope inherit down to everything inside it, so one assignment now covers every resource in that group — current and future — without touching each one individually. Watching the same role behave differently depending on where it's assigned was the actual lesson, not the Reader role itself.

### Assigning the User Access Administrator role

This one's worth sitting with. Most Azure RBAC roles, like Reader or Contributor, control what someone can do *to resources*. User Access Administrator is different: it controls who can *manage other people's access*, which means someone holding it can grant themselves or anyone else broader permissions later, even ones they don't currently have. It's a privilege-escalation control point in exactly the same way Domain Admins was during the on-prem AD hygiene work earlier in this lab — worth auditing carefully and never handed out casually, since holding it is functionally holding the ability to grant any other access down the line.

---

## Skills demonstrated

- Deploying Azure VMs (Linux and Windows) as practical test infrastructure
- Understanding Azure's RBAC scope hierarchy (Management Group, Subscription, Resource Group, Resource) and permission inheritance
- Assigning roles at different scopes and observing how inheritance changes effective access
- Recognizing User Access Administrator as a privilege-escalation-sensitive role, distinct from resource-access roles like Reader
- Connecting Azure RBAC back to the same least-privilege and privileged-account-auditing principles applied earlier to on-prem Active Directory
