# Identity Governance & Administration (IGA)

## What I did

No hands-on lab this time — this was a conceptual deep dive into access request workflows, compliance reporting, and the current IGA tooling landscape. Worth documenting anyway, since IGA is the governance layer that sits on top of everything built hands-on so far in this lab. Provisioning, RBAC, PKI, SSO, MFA are all about performing and securing access. IGA is about proving that access is correct and being able to answer for it later.

## Steps

### IAM vs. IGA — the distinction that actually matters

Most of this lab up to this point has been IAM in the practical sense: provisioning accounts, granting and revoking access, configuring authentication. IGA is a layer on top of that, not a replacement for it. It's not about performing access changes — it's about governing them: deciding whether access *should* be granted in the first place, proving it was granted correctly, and catching it when it's wrong. IAM does the work. IGA answers for it.

### Access request and approval workflows

The formal version of granting access: someone requests it, the request routes through an approval chain (typically the resource owner, sometimes a manager too), access only gets granted after approval, and the whole exchange gets logged. This replaces the informal version — someone emailing an admin asking to "please give me access to X" — with a structured, reviewable record: who asked, who approved, what justification was given, and what the outcome was.

### Compliance reporting: current state vs. historical state

Learned the distinction that actually matters for audits. Current state answers "who has access to what right now" — straightforward to pull directly from a live directory. Historical state is harder and more important during an actual audit: "who had access to what, when, and why did they get it." That requires genuinely retained audit trails, not just a live snapshot, since auditors care about what access existed during the period under review, not just what it looks like today.

### Access request audit trails

Every request, approval, denial, and revocation needs a durable record tied to who requested it, who approved it, and when. This is what makes a real access review or compliance audit possible after the fact, instead of relying on someone's memory of a decision made months earlier.

### Compliance dashboards

Dashboards that aggregate current and historical access data into something reviewable for auditors and compliance teams, rather than requiring someone to manually query the directory every time. A good one surfaces things like outstanding access reviews, orphaned accounts, and policy violations at a glance instead of burying them in raw data.

### The IGA tooling landscape

Looked at the actual market rather than assuming AD or Entra alone covers this. **SailPoint** is one of the dominant enterprise IGA platforms, known for depth across large, multi-platform environments. **Microsoft Entra ID Governance** is Microsoft's own governance layer built directly on top of Entra ID, a natural fit for cloud-first organizations already standardized on Microsoft's identity stack. **One Identity Manager** is a strong option specifically for on-premises and hybrid environments rather than cloud-native ones. Which one fits depends heavily on the environment: cloud-first leans toward Entra ID Governance, hybrid or legacy-heavy leans toward One Identity, and large complex multi-platform enterprises often reach for SailPoint specifically for its cross-platform governance depth.

### Why good IGA actually matters operationally, not just for compliance

The case for IGA isn't only about passing an audit. A mature IGA program measurably reduces IT labor — automated access request and approval workflows, plus periodic access reviews, replace what would otherwise be manual ticket-by-ticket admin work. Done well, governance is an efficiency gain for the IT team, not purely a cost imposed by compliance requirements.

---

## Skills demonstrated

- Understanding the distinction between IAM (performing access operations) and IGA (governing and proving them)
- Access request and approval workflow design
- Compliance reporting concepts, including current-state vs. historical-state auditing
- Access request audit trail requirements for audit readiness
- Awareness of the enterprise IGA tooling landscape (SailPoint, Microsoft Entra ID Governance, One Identity Manager) and how environment type drives tool selection
- Recognizing IGA as an operational efficiency lever for IT, not purely a compliance obligation
